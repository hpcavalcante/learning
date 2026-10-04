# CI Watcher determinístico para agentes

## Problema

Atualmente o agente acompanha pipelines do GitHub Actions fazendo polling diretamente:

```text
agent
  → gh
  → recebe estado/logs
  → interpreta
  → espera
  → repete
```

Uma esteira pode durar 20 minutos ou mais. Durante quase todo esse período não existe nenhuma decisão a ser tomada, mas o agente continua consumindo contexto e raciocínio apenas para concluir que deve esperar novamente.

## Proposta

Mover o acompanhamento da esteira para um processo determinístico chamado `ci-watch`.

```text
agent
  │
  └── ci-watch RUN_ID
          │
          ├── consulta metadata
          ├── classifica jobs/steps
          ├── espera
          ├── consulta novamente
          └── retorna somente quando necessário
                    │
                    ▼
                  agent
```

O agente faz uma única chamada bloqueante.

O tempo da esteira pode ser 30 segundos ou 30 minutos; isso não altera o fluxo.

O `ci-watch` realiza polling internamente sem enviar os resultados intermediários para o contexto do agente.

## Política dos steps

Os steps podem ser classificados como:

- **blocking**: uma falha exige intervenção imediata.
- **soft**: a falha é registrada, mas não interrompe a esteira.
- **ignore**: não é relevante para a decisão do agente.
- **audit**: mesmo em sucesso pode executar alguma validação adicional específica.

Logs não fazem parte do fluxo normal de observação.

Eles são material de diagnóstico e só são recuperados quando uma política exige.

## Eventos retornados

O contrato entre `ci-watch` e o agente deve ser pequeno e estável.

Exemplo de sucesso:

```json
{
  "event": "pipeline_passed",
  "run_id": 123456,
  "head_sha": "abc123",
  "soft_failures": ["sonarqube"]
}
```

Exemplo de falha:

```json
{
  "event": "blocking_failure",
  "run_id": 123456,
  "head_sha": "abc123",
  "job": "integration-tests",
  "step": "Run integration tests",
  "log_path": "/tmp/ci-watch/123456/failure.log"
}
```

Quando ocorrer uma falha bloqueante, o watcher busca apenas o log necessário e devolve o controle ao agente.

## Princípio

O agente não deve ser infraestrutura de monitoring.

Polling, espera, classificação de estados e detecção de transições são problemas determinísticos e devem ser resolvidos por software tradicional.

O agente deve participar apenas quando existe uma decisão que exige interpretação:

```text
monitoramento → código

diagnóstico → agente

decisão → agente

espera → código
```

Assim, tokens e capacidade de raciocínio são gastos apenas onde realmente agregam valor.

---

# Integração opcional com tmux

A integração com tmux é uma camada de apresentação e observabilidade humana.

O `ci-watch` não deve depender dela para funcionar.

```text
ci-watch
   │
   ├── dentro de tmux → pode abrir UI auxiliar
   │
   └── fora de tmux   → funciona headless
```

A mesma lógica de polling, classificação, políticas e geração de eventos deve funcionar nos dois modos.

## Ambiente

O tmux roda dentro do mesmo container persistente em que já existem:

```text
container
├── gh
├── git
├── agente
├── ci-watch
├── código
└── tmux
```

O terminal do host serve apenas como interface para acessar o container:

```text
host
  │
  │ docker exec
  ▼
container
  │
  ▼
tmux
```

Um launcher no host pode fazer:

```bash
docker exec -it work-container \
  tmux new-session -A -s work
```

Assim, a sessão é criada quando necessário ou reutilizada quando já existe.

## Execução do watcher

Quando o agente executar:

```bash
ci-watch RUN_ID
```

o CLI pode detectar se está dentro de tmux através de `$TMUX`.

Se estiver, pode separar controller e worker:

```text
tmux
│
├── pane A
│   │
│   └── agent
│        │
│        └── ci-watch RUN_ID
│             │
│             └── controller
│                   espera evento
│
└── pane B
    │
    └── ci-watch worker
          │
          ├── polling
          ├── state machine
          ├── UI humana
          └── GitHub API
```

O pane do watcher pode exibir informações úteis para acompanhamento manual:

```text
CI Watch · run #928173

✓ checkout
✓ dependencies
✓ build
→ integration tests
○ deploy

last poll: 14:32:18
```

Essa saída existe exclusivamente para observação humana.

## Separação entre stdout humano e protocolo do agente

A saída visual do watcher não deve ser usada como canal de comunicação com o agente.

```text
                    ci-watch worker
                     /           \
                    /             \
                   ▼               ▼
             stdout/UI         result.json
             para humano       para máquina
                   │               │
                   ▼               ▼
             tmux pane         controller
                                   │
                                   ▼
                                 agent
```

O worker pode imprimir quanto quiser em seu próprio pane sem que isso entre no contexto do agente.

O processo chamado pelo agente deve permanecer silencioso enquanto espera.

Quando houver um evento relevante, o worker grava um resultado estruturado:

```text
/tmp/ci-watch/<execution-id>/
├── result.json
└── failure.log
```

Por exemplo:

```json
{
  "event": "blocking_failure",
  "run_id": 928173,
  "job": "integration-tests",
  "step": "Run tests",
  "log_path": "/tmp/ci-watch/928173/failure.log"
}
```

O controller então retorna somente esse resultado ao agente.

Portanto:

```text
watcher stdout → humano

result.json → controller → agente
```

O agente não recebe a telemetria visual do watcher.

## Sincronização

Se ambos estiverem na mesma sessão tmux, `tmux wait-for` pode ser usado apenas como mecanismo de sinalização.

Fluxo:

```text
controller
   │
   ├── cria worker
   │
   └── espera sinal
            │
            │
worker      │
   │        │
   ├── acompanha pipeline
   ├── detecta evento
   ├── grava result.json
   └── sinaliza controller
            │
            ▼
controller lê result.json
            │
            ▼
          agent
```

O tmux transporta apenas o sinal de que existe um resultado disponível.

O payload continua sendo um arquivo estruturado ou outro IPC explícito.

## Responsabilidades

```text
ci-watch core
    GitHub API
    polling
    state machine
    policies
    classificação de steps
    recuperação seletiva de logs

ci-watch tmux adapter
    criação de pane/window
    apresentação visual
    lifecycle da UI
    sincronização local

agent
    diagnóstico
    decisão
    correção
    validações finais
    merge
```

A skill do agente não precisa conhecer tmux.

Ela continua tendo apenas uma interface:

```bash
ci-watch RUN_ID
```

A existência de uma UI em tmux deve ser um detalhe interno e opcional do `ci-watch`.
