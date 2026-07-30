# Formação em Arquitetura de Software, Cloud e Machine Learning em Produção

> Uma trilha autodidata para construir e operar sistemas — inclusive sistemas de machine learning — a partir dos fundamentos que os sustentam, e não das ferramentas que os embrulham.
>
> **Sobre o escopo de ML:** esta formação assume que operar ML sem entender ML é uma posição frágil. O objetivo não é virar pesquisador, mas conseguir treinar modelos, implementar arquiteturas do zero, diagnosticar por que um modelo está errado, ler um paper novo e decidir se ele se aplica ao seu caso. "Fazer deploy no SageMaker" é consequência, não objetivo.

---

## Sumário

- [A tese desta formação](#a-tese-desta-formação)
- [Princípios](#princípios)
- [Mapa geral](#mapa-geral)
- [Orçamento de tempo](#orçamento-de-tempo)
- [Fase 0 — Fatia Vertical](#fase-0--fatia-vertical)
- [Fase I — Máquina, Rede e Dados](#fase-i--máquina-rede-e-dados)
- [Fase II — Sistemas Distribuídos e Arquitetura](#fase-ii--sistemas-distribuídos-e-arquitetura)
- [Fase III — Cloud e Plataforma](#fase-iii--cloud-e-plataforma)
- [Fase IV — Machine Learning de Verdade](#fase-iv--machine-learning-de-verdade)
- [Fase V — ML em Produção](#fase-v--ml-em-produção)
- [Fase VI — O Ofício do Arquiteto](#fase-vi--o-ofício-do-arquiteto)
- [Trilhas alternativas](#trilhas-alternativas)
- [Sistema de avaliação](#sistema-de-avaliação)
- [Biblioteca](#biblioteca)

---

## A tese desta formação

Arquitetura, cloud e ML em produção são as áreas da computação com **a maior taxa de rotatividade de ferramentas e a menor taxa de mudança de conceitos**. O serviço gerenciado da moda muda a cada 18 meses. Os problemas que ele resolve — nomear, rotear, replicar, isolar, reconciliar, observar, versionar, reverter — são os mesmos desde os anos 1970. E a matemática por baixo de um modelo treinado hoje é essencialmente a mesma de 1986.

Isso cria uma armadilha específica: é possível construir uma carreira inteira sabendo operar ferramentas sem nunca entender o problema que elas resolvem. Quem está nessa posição precisa reaprender tudo a cada ciclo, e não consegue avaliar se a ferramenta nova é melhor ou apenas diferente.

A saída é uma inversão de ordem:

> **Para cada ferramenta importante, estude primeiro o problema, implemente uma versão ingênua sua, e só então aprenda a ferramenta — que passa a ser óbvia.**

Você não vai decorar `kubectl`. Vai escrever um loop de reconciliação, sentir por que ele é difícil, e aí o Kubernetes vira uma implementação bem-feita de algo que você já entende. Você não vai decorar a API do PyTorch. Vai escrever autograd, sofrer com gradientes que explodem, e aí o framework vira conveniência em cima de algo que você domina.

### Por que ML entra a fundo, e não como apêndice

Existe uma leitura comum de "MLOps" que é essencialmente DevOps com um arquivo `.pkl` no meio: empacotar um artefato opaco, subir, monitorar latência. Essa leitura produz alguém que não consegue responder as perguntas que realmente importam:

- O modelo caiu de qualidade — isso é drift, bug de feature, ou apenas ruído estatístico?
- Esse paper novo melhora 2% num benchmark — vale a complexidade? A avaliação dele é honesta?
- O modelo está lento — quantizar vai custar quanta qualidade, e por quê?
- A métrica subiu depois da mudança — isso é real ou é vazamento?
- Precisamos de um modelo maior, mais dados, ou uma função de perda diferente?

Nenhuma dessas é respondível sem entender ML por dentro. Por isso a **Fase IV é uma formação de ML de verdade** — matemática aplicada, modelos clássicos implementados do zero, deep learning com autograd próprio, transformers linha a linha, e leitura de papers — e a **Fase V é a operação disso**, que é onde arquitetura e cloud reencontram o assunto.

---

## Princípios

### 1. Implementar antes de usar

Todo capítulo de ferramenta é precedido por uma implementação ingênua. Você escreve seu próprio agregador de métricas antes de usar Prometheus, seu próprio runtime de container antes de usar Docker a sério, seu próprio autograd antes de usar PyTorch a sério, seu próprio registro de modelos antes de usar MLflow.

### 2. Vendor-neutral por padrão, específico por escolha

Conceitos são estudados em abstrato. A prática usa **um** provedor escolhido por você, com um exercício explícito de tradução para os outros dois. Amarrar o entendimento a uma nuvem é o erro mais comum e mais caro da área.

### 3. Trade-off é a unidade de conhecimento

Não existe "arquitetura correta", nem "modelo correto". Cada capítulo termina com um documento comparando pelo menos duas abordagens, com as condições em que cada uma vence. Se você não consegue argumentar o lado que não escolheu, não entendeu a decisão.

### 4. Todo sistema seu deve falhar em produção simulada

Um sistema que você nunca quebrou de propósito é um sistema que você não conhece. Isso vale para infraestrutura (injeção de falha) e para modelos (dados fora de distribuição, entradas adversariais, drift injetado).

### 5. Medir com variância, nunca com número único

Um resultado sem múltiplas execuções e intervalo de confiança não é um resultado. Esse princípio, aplicado com disciplina, já coloca você acima de boa parte do que se publica.

### 6. Escrever é o entregável final

Arquitetura e ML se comunicam por escrito. Cada capítulo produz código **e** um documento. A parte escrita não é burocracia — é onde o entendimento aparece ou não.

### 7. Dois níveis abaixo

Você deve entender um nível abaixo da abstração em que trabalha, e ter noção do segundo. Usa Kubernetes → entenda containers → saiba que existem namespaces. Usa transformers → entenda atenção e backprop → saiba que existe uma jacobiana ali.

---

## Mapa geral

```
FASE 0 — Fatia Vertical                        ~40h    Ver o todo, mal e rápido
   │
FASE I — Máquina, Rede e Dados                ~170h    O que existe abaixo da nuvem
   │
FASE II — Sistemas Distribuídos e Arquitetura ~210h    Como sistemas se coordenam
   │
FASE III — Cloud e Plataforma                 ~220h    Como isso vira infraestrutura
   │
FASE IV — Machine Learning de Verdade         ~300h    Como modelos funcionam  ← núcleo
   │
FASE V — ML em Produção                       ~210h    Como modelos viram sistemas
   │
FASE VI — O Ofício do Arquiteto               ~150h    Como decidir, ler e comunicar
                                             ───────
                                              ~1.300h
```

**Dependências.**

```
Fase 0 → Fase I → Fase II → Fase III ─┐
                                       ├→ Fase V → Fase VI
Fase I (cap. 4) ──────→ Fase IV ──────┘
```

A **Fase IV pode ser cursada em paralelo** com as Fases II e III — ela depende pouco delas (só do Capítulo 4, de dados). Muita gente vai preferir intercalar: um capítulo de sistemas, um de ML. Isso é encorajado; alternar reduz fadiga e as conexões aparecem mais cedo.

A Fase V depende de verdade das Fases III e IV. A Fase VI deve ser iniciada em paralelo a partir da Fase II — escrever decisões é prática contínua, não capítulo final.

---

## Orçamento de tempo

| Ritmo | Duração |
|---|---|
| 8h/semana | ~3 anos |
| 12h/semana | ~2 anos |
| 16h/semana | ~1,5 ano |
| 20h/semana | ~1,2 ano |

As horas assumem implementação real, não leitura. Se você já trabalha com desenvolvimento de software, a Fase I tende a andar mais rápido que o estimado.

Se isso não couber, corte de propósito — veja [Trilhas alternativas](#trilhas-alternativas). Cortar conscientemente é muito melhor que abandonar no Capítulo 9.

---

# Fase 0 — Fatia Vertical

**~40h · Objetivo: colocar um sistema de ML em produção antes de saber fazer isso direito.**

O ponto não é fazer bem. É atravessar o caminho inteiro uma vez, colecionar dúvidas, e ter contexto para as fases seguintes. Tudo aqui será refeito com rigor depois.

### 0.1 — Um serviço de ML no ar (~20h)

Treine um modelo trivial (regressão logística em um dataset tabular público). Empacote em uma API HTTP. Coloque em um container. Suba em uma nuvem qualquer. Aponte um domínio. Coloque HTTPS. Faça uma requisição do seu celular.

**Artefato:** `vertical-slice/`
**Critério:** funciona a partir de uma máquina que não é a sua.

### 0.2 — Uma rede neural do zero (~12h)

Em NumPy puro, sem PyTorch e sem autograd: um MLP de duas camadas classificando MNIST, com backprop escrito à mão.

**Artefato:** `vertical-slice/neural-net-naive/`
**Critério:** acerta >95% no MNIST. Você não precisa entender tudo agora — precisa ter sofrido com isso uma vez.
**O que isso ancora:** a Fase IV inteira. Quando chegar lá, você vai reconhecer cada peça.

### 0.3 — Quebre e observe (~5h)

Com o serviço no ar: mate o processo. Estoure a memória. Envie 1000 requisições simultâneas. Envie entradas inválidas. Envie dados de um mês diferente do treino. Anote o que aconteceu em cada caso e o que você *não* conseguiu descobrir por falta de instrumentação.

**Artefato:** `vertical-slice/incidents.md`

### 0.4 — Diário de dúvidas (~3h)

Liste tudo que funcionou por mágica e tudo que você não entendeu. Este arquivo é o seu currículo pessoal — revisite-o ao fim de cada fase e risque o que já sabe explicar.

**Artefato:** `learning-journal/questions.md`

---

# Fase I — Máquina, Rede e Dados

**~170h · Objetivo: entender o que a nuvem está abstraindo.**

Cloud é uma abstração sobre máquinas, redes e discos. Quem não entende a camada de baixo não sabe por que a de cima se comporta como se comporta — e vira refém de tentativa e erro quando algo dá errado.

---

### Capítulo 1 — Linux e Sistemas Operacionais na Prática (~50h)

**Objetivo.** Enxergar processos, memória, arquivos e I/O como coisas concretas.

**Competências.**
- Explicar processos, threads, syscalls, memória virtual e escalonamento.
- Diagnosticar um processo travado, vazando memória ou saturando I/O, com ferramentas de linha de comando.
- Explicar limites de recursos, sinais e o ciclo de vida de um processo.
- Entender file descriptors, I/O bloqueante vs. não-bloqueante, e o modelo de eventos (epoll).
- Ler `/proc` e entender o que cada métrica significa.

**Materiais.**
- *Obrigatório:* Arpaci-Dusseau — **Operating Systems: Three Easy Pieces** (gratuito), partes de virtualização e persistência.
- *Complementar:* Gregg — *Systems Performance*, caps. 1–6; `man 7 signal`, `man 7 epoll`.
- *Opcional:* Kerrisk — *The Linux Programming Interface* (referência definitiva).

**Projetos.**
- *Fundamental:* mini-shell com pipes, redirecionamento e controle de processos filhos.
- *Intermediário:* monitor de recursos próprio, lendo `/proc` diretamente, sem bibliotecas.
- *Mestre:* servidor com loop de eventos (epoll) e comparação de throughput contra um modelo thread-per-connection.

**Trade-off a documentar.** Processo vs. thread vs. corrotina: em que carga cada modelo vence, e por quê.

**Artefato.** `linux-systems-lab/`

---

### Capítulo 2 — Redes (~45h)

**Objetivo.** Entender a camada por onde todo sistema distribuído sofre.

**Competências.**
- Explicar o caminho completo de uma requisição: DNS → TCP → TLS → HTTP → aplicação.
- Explicar handshake TCP, controle de congestão, retransmissão, e por que conexões custam caro.
- Entender TLS o suficiente para depurar um erro de certificado sem chutar.
- Comparar HTTP/1.1, HTTP/2 e gRPC pelo que mudam no comportamento de rede.
- Explicar balanceamento de carga (L4 vs. L7), proxies reversos, NAT e por que redes privadas existem.
- Raciocinar sobre latência, throughput e o custo real de um round-trip.

**Materiais.**
- *Obrigatório:* Kurose & Ross — **Computer Networking: A Top-Down Approach** (caps. 1–4, seletivamente).
- *Complementar:* Grigorik — *High Performance Browser Networking* (gratuito); Beej's Guide to Network Programming.
- *Opcional:* Stevens — *TCP/IP Illustrated Vol. 1*.

**Projetos.**
- *Fundamental:* servidor HTTP do zero em sockets — parsing manual, keep-alive, chunked encoding.
- *Intermediário:* proxy reverso próprio com balanceamento round-robin e health check.
- *Mestre:* capturar e dissecar um handshake TLS real byte a byte; depois implementar um protocolo confiável simples sobre UDP.

**Trade-off a documentar.** Balanceamento L4 vs. L7: o que cada um pode e não pode fazer, e o custo de cada.

**Artefato.** `networking-lab/`

---

### Capítulo 3 — Concorrência (~35h)

**Objetivo.** Raciocinar sobre execução simultânea sem produzir bugs invisíveis.

**Competências.**
- Identificar race conditions, deadlocks e starvation por leitura de código.
- Usar corretamente locks, filas, semáforos, canais e async/await.
- Explicar a diferença entre concorrência e paralelismo e escolher o modelo certo.
- Aplicar backpressure e dimensionar pools de forma justificada, não por chute.
- Entender idempotência e por que ela é o antídoto de metade dos problemas distribuídos.

**Materiais.**
- *Obrigatório:* OSTEP, parte de concorrência; Herlihy & Shavit — *The Art of Multiprocessor Programming*, caps. 1–3.
- *Complementar:* documentação de concorrência do Go ou do Rust (ambas são material didático de qualidade).
- *Opcional:* Lamport — *Specifying Systems* (TLA+), para quem quiser verificar em vez de torcer.

**Projetos.**
- *Fundamental:* thread pool e fila bloqueante do zero, com testes que provocam contenção.
- *Intermediário:* rate limiter (token bucket e sliding window) implementado e testado sob carga concorrente.
- *Mestre:* reproduzir deliberadamente três bugs clássicos de concorrência e corrigi-los documentando o raciocínio.

**Trade-off a documentar.** Fila com bloqueio vs. rejeição vs. buffer ilimitado — o que cada escolha faz com a latência de cauda.

**Artefato.** `concurrency-lab/`

---

### Capítulo 4 — Dados e Armazenamento (~40h)

**Objetivo.** Entender persistência a fundo, porque é onde os dados (e os problemas) ficam.

**Competências.**
- Explicar armazenamento em páginas, índices, B-Trees e LSM-Trees — e quando cada família vence.
- Explicar transações, ACID, níveis de isolamento e MVCC; reconhecer cada anomalia por sintoma.
- Explicar WAL e como ele fundamenta durabilidade, replicação e recuperação.
- Comparar modelos de dados (relacional, documento, colunar, chave-valor, vetorial) por perfil de acesso.
- Modelar dados relacionais com normalização consciente e desnormalização justificada.
- Entender armazenamento de objetos (S3-like) e por que ele mudou a arquitetura de dados.

**Materiais.**
- *Obrigatório:* CMU 15-445 — **Database Systems** (aulas gratuitas de Andy Pavlo) + Kleppmann — *Designing Data-Intensive Applications*, caps. 2–4.
- *Complementar:* Petrov — *Database Internals*.
- *Opcional:* documentação interna do PostgreSQL sobre MVCC e VACUUM.

**Projetos.**
- *Fundamental:* engine de armazenamento chave-valor com WAL e índice em B-Tree ou LSM, persistindo em disco.
- *Intermediário:* laboratório de isolamento — provocar dirty read, phantom e write skew num banco real, e mostrar qual nível resolve cada um.
- *Mestre:* comparar B-Tree vs. LSM na sua própria implementação, medindo amplificação de escrita e de leitura.

**Trade-off a documentar.** B-Tree vs. LSM-Tree: para que carga cada uma foi projetada.

**Artefato.** `storage-lab/`

---

### ✅ Conclusão da Fase I

- [ ] diagnosticar um processo problemático sem adivinhar;
- [ ] explicar cada etapa de uma requisição HTTPS;
- [ ] escrever código concorrente e justificar o modelo escolhido;
- [ ] explicar índices, transações e durabilidade em nível de implementação;
- [ ] ter quebrado cada um dos seus artefatos de propósito.

---

# Fase II — Sistemas Distribuídos e Arquitetura

**~210h · Objetivo: projetar sistemas que continuam funcionando quando partes deles não funcionam.**

Esta é a fase conceitualmente mais densa e a que mais protege contra modismo. Padrões de arquitetura são, quase todos, respostas a restrições descobertas aqui.

---

### Capítulo 5 — Fundamentos de Sistemas Distribuídos (~60h)

**Objetivo.** Internalizar que falha parcial é o estado normal, não a exceção.

**Competências.**
- Explicar falha parcial, timeouts, e por que "a rede está lenta" e "o serviço morreu" são indistinguíveis.
- Explicar relógios físicos vs. lógicos, e por que timestamp não ordena eventos distribuídos.
- Explicar modelos de consistência (linearizável, causal, eventual) e o que cada um custa.
- Explicar CAP corretamente — e por que quase toda citação dele na internet está errada.
- Explicar replicação (líder-seguidor, multi-líder, sem líder), quórum e particionamento.
- Explicar consenso (Raft) em nível de mecanismo, não de slogan.
- Explicar entrega exactly-once como o mito que é, e idempotência como a solução real.

**Materiais.**
- *Obrigatório:* Kleppmann — **Designing Data-Intensive Applications**, caps. 5–9 (o núcleo da fase) + Kleppmann — *Distributed Systems* (curso em vídeo, gratuito).
- *Complementar:* Ongaro & Ousterhout — *In Search of an Understandable Consensus Algorithm* (Raft); *The Raft Visualization*.
- *Opcional:* MIT 6.824 (labs em Go, excelentes e difíceis); Lamport — *Time, Clocks, and the Ordering of Events*.

**Projetos.**
- *Fundamental:* key-value store replicado com líder e seguidores, e um cliente que sobrevive à troca de líder.
- *Intermediário:* implementar Raft (ou os labs 6.824 até eleição + replicação de log).
- *Mestre:* testes de consistência sob injeção de partição de rede no seu próprio sistema — encontrar uma violação real e corrigi-la.

**Trade-off a documentar.** Consistência forte vs. eventual: o custo em latência, disponibilidade e complexidade de aplicação.

**Artefato.** `distributed-systems-lab/`

---

### Capítulo 6 — APIs, Contratos e Fronteiras (~35h)

**Objetivo.** Projetar as costuras do sistema — que é o que arquitetura de fato é.

**Competências.**
- Projetar APIs REST, gRPC e GraphQL, e escolher entre elas com justificativa.
- Versionar interfaces com compatibilidade para frente e para trás.
- Definir contratos explícitos (OpenAPI, Protobuf, JSON Schema) e testá-los.
- Projetar semântica de erro, paginação, idempotência e limites de taxa.
- Identificar fronteiras de domínio: o que deve ser um serviço e o que definitivamente não deve.

**Materiais.**
- *Obrigatório:* Google — **API Design Guide** (gratuito); Newman — *Building Microservices*, caps. de integração.
- *Complementar:* Evans — *Domain-Driven Design* (partes de contextos delimitados); Vernon — *Implementing DDD*.
- *Opcional:* Fielding — dissertação original sobre REST (curta e frequentemente mal citada).

**Projetos.**
- *Fundamental:* mesma API implementada em REST e gRPC, com contratos versionados e testes de contrato.
- *Intermediário:* evoluir a API por três versões incompatíveis mantendo clientes antigos funcionando.
- *Mestre:* decompor um monolito de exemplo em serviços — e escrever a justificativa de cada fronteira escolhida, incluindo as que você decidiu **não** cortar.

**Trade-off a documentar.** REST vs. gRPC vs. GraphQL, por tipo de consumidor e de carga.

**Artefato.** `api-design-lab/`

---

### Capítulo 7 — Mensageria e Arquitetura Orientada a Eventos (~40h)

**Objetivo.** Entender comunicação assíncrona, base de quase toda arquitetura moderna — e de quase todo pesadelo operacional.

**Competências.**
- Comparar filas e logs de eventos (RabbitMQ-like vs. Kafka-like) estruturalmente.
- Explicar garantias de entrega (at-most-once, at-least-once) e implementar idempotência de consumidor.
- Explicar ordenação, particionamento, grupos de consumidores e reprocessamento.
- Projetar com dead letter queue, retry com backoff e tratamento de mensagem-veneno.
- Explicar a saga e o outbox transacional como respostas a transações distribuídas.
- Reconhecer quando eventos são a ferramenta errada.

**Materiais.**
- *Obrigatório:* Kleppmann — DDIA, cap. 11 (log-based messaging) + documentação conceitual do Kafka.
- *Complementar:* Richardson — *Microservices Patterns* (saga, outbox, CQRS); Stopford — *Designing Event-Driven Systems* (gratuito).
- *Opcional:* Kleppmann — *Turning the Database Inside-Out* (palestra; muda a perspectiva).

**Projetos.**
- *Fundamental:* broker de mensagens próprio, com log append-only, offsets e grupos de consumidores.
- *Intermediário:* implementar o padrão outbox transacional de ponta a ponta e provar que ele não perde eventos sob falha.
- *Mestre:* saga distribuída com compensação, testada sob falha em cada etapa.

**Trade-off a documentar.** Orquestração vs. coreografia em processos distribuídos.

**Artefato.** `messaging-lab/`

---

### Capítulo 8 — Padrões de Arquitetura e Trade-offs (~40h)

**Objetivo.** Conhecer o repertório de estruturas — e, principalmente, o custo de cada uma.

> Este capítulo é onde a maior parte do conteúdo popular da internet erra, tratando padrões como receitas de sucesso em vez de escolhas com custo. Estude-os como trade-offs.

**Competências.**
- Explicar e comparar: monolito, monolito modular, microserviços, arquitetura em camadas, hexagonal, CQRS, event sourcing.
- Explicar a Lei de Conway e por que arquitetura é parcialmente um problema organizacional.
- Aplicar acoplamento e coesão como critérios operacionais, não como jargão.
- Reconhecer o custo real de microserviços: rede, observabilidade, consistência, deploy, cognição.
- Projetar para evolução: costuras que permitem mudar de ideia depois.
- **Reconhecer quando a solução da moda é a errada** — inclusive quando a moda é IA. Saber argumentar, com número, que um problema determinístico resolvido por regra, índice ou consulta é mais barato, mais rápido, mais testável e mais auditável que o mesmo problema resolvido por modelo. Essa competência só existe em quem entende as duas opções por dentro: quem só sabe usar LLM não consegue argumentar contra usar LLM.

**Materiais.**
- *Obrigatório:* Ford, Richards et al. — **Fundamentals of Software Architecture** + Ousterhout — *A Philosophy of Software Design* (curto, denso, excelente).
- *Complementar:* Newman — *Monolith to Microservices*; Fowler — *Patterns of Enterprise Application Architecture* (referência).
- *Opcional:* Ford et al. — *Building Evolutionary Architectures*.

**Projetos.**
- *Fundamental:* mesmo sistema implementado como monolito modular e como microserviços, com comparação medida de latência, complexidade de deploy e esforço de mudança.
- *Intermediário:* aplicar CQRS e event sourcing a um subdomínio — e documentar honestamente onde eles atrapalharam.
- *Mestre:* revisão de arquitetura escrita de um sistema open source real, apontando decisões e seus custos.

**Trade-off a documentar.** O documento central da fase: *quando microserviços valem a pena e quando são um erro caro*.

**Artefato.** `architecture-patterns-lab/`

---

### Capítulo 9 — Confiabilidade e Resiliência (~35h)

**Objetivo.** Projetar sistemas que degradam com graça em vez de cair.

**Competências.**
- Definir SLI, SLO e error budget, e usá-los para decidir prioridades de engenharia.
- Implementar timeout, retry com backoff e jitter, circuit breaker, bulkhead e backpressure.
- Explicar falhas em cascata, tempestade de retry e efeito manada — e como preveni-los.
- Projetar degradação graciosa e modo de operação reduzida.
- Conduzir engenharia de caos de forma responsável.
- Escrever post-mortem sem culpa (blameless) que gere mudança real.

**Materiais.**
- *Obrigatório:* Google — **Site Reliability Engineering** (gratuito), caps. de SLO, gerenciamento de risco e post-mortem; Dean & Barroso — *The Tail at Scale*.
- *Complementar:* Nygard — *Release It!* (o catálogo de padrões de estabilidade); Google — *The Site Reliability Workbook*.
- *Opcional:* relatórios públicos de incidentes de grandes provedores (extremamente educativos).

**Projetos.**
- *Fundamental:* biblioteca própria de resiliência — timeout, retry com jitter, circuit breaker, bulkhead — com testes que provocam cada condição.
- *Intermediário:* provocar uma falha em cascata no seu sistema e depois preveni-la, com medições antes/depois.
- *Mestre:* definir SLOs para um sistema seu, instrumentá-los, e conduzir um exercício de caos com post-mortem escrito.

**Trade-off a documentar.** Retry vs. falha rápida: quando insistir piora tudo.

**Artefato.** `reliability-lab/`

---

### ✅ Conclusão da Fase II

- [ ] explicar consenso, replicação e consistência sem slogans;
- [ ] projetar contratos de API que sobrevivem à evolução;
- [ ] projetar fluxos assíncronos com garantias explícitas;
- [ ] argumentar os dois lados de qualquer decisão arquitetural;
- [ ] definir SLOs e projetar para degradação graciosa;
- [ ] ter derrubado seu próprio sistema de propósito e documentado o aprendizado.

---

# Fase III — Cloud e Plataforma

**~220h · Objetivo: entender infraestrutura moderna como sistema, não como catálogo de serviços.**

Regra desta fase: **para cada serviço gerenciado estudado, saber dizer que problema clássico ele resolve e como você o resolveria sem ele.** Se não souber, você aprendeu a interface, não a ideia.

---

### Capítulo 10 — Modelo Mental de Cloud (~30h)

**Objetivo.** Reduzir centenas de serviços a cinco primitivas.

**Competências.**
- Mapear qualquer serviço gerenciado em uma primitiva: **compute, storage, network, identity, managed state**.
- Explicar regiões, zonas de disponibilidade e domínios de falha — e projetar em função deles.
- Explicar o modelo de responsabilidade compartilhada.
- Comparar IaaS, PaaS, containers gerenciados e serverless por trade-off, não por hype.
- Traduzir um desenho de arquitetura entre AWS, GCP e Azure.

**Materiais.**
- *Obrigatório:* **AWS Well-Architected Framework** ou *Google Cloud Architecture Framework* (gratuitos, estruturalmente vendor-neutral).
- *Complementar:* Barroso, Hölzle & Ranganathan — *The Datacenter as a Computer* (gratuito; o que existe fisicamente por baixo).
- *Opcional:* papers de infraestrutura do Google (Borg, Colossus, Spanner).

**Projetos.**
- *Fundamental:* tabela de tradução entre os três grandes provedores para 20 serviços, com as diferenças reais (não só de nome).
- *Intermediário:* mesma arquitetura desenhada para três modelos de execução (VM, container, serverless), com análise de custo, partida a frio e operabilidade.
- *Mestre:* documento explicando, para 5 serviços gerenciados, como você os implementaria do zero e por que provavelmente não deveria.

**Trade-off a documentar.** Serverless vs. containers: onde a fronteira realmente está, em custo e em controle.

**Artefato.** `cloud-fundamentals/`

---

### Capítulo 11 — Containers por Dentro (~35h)

**Objetivo.** Descobrir que container não é máquina virtual, e sim um processo com mentiras convenientes.

**Competências.**
- Explicar namespaces, cgroups, capabilities e union filesystems.
- Explicar a estrutura de uma imagem: camadas, manifesto, cache, e por que a ordem do Dockerfile importa.
- Construir imagens mínimas, seguras e reprodutíveis (multi-stage, usuário não-root, sem segredos).
- Explicar redes de container e volumes.
- Explicar o que um runtime (containerd, runc) realmente faz.

**Materiais.**
- *Obrigatório:* Liz Rice — **Containers From Scratch** (palestra) + *Container Security* (caps. iniciais); `man 7 namespaces`, `man 7 cgroups`.
- *Complementar:* especificação OCI de imagem e runtime (curta e esclarecedora).
- *Opcional:* código-fonte do runc, para os corajosos.

**Projetos.**
- *Fundamental:* runtime de container próprio, em ~200 linhas, usando namespaces e cgroups diretamente — isolando um processo de verdade.
- *Intermediário:* construir uma imagem do zero (sem `FROM`) e reduzir uma imagem existente em 10x, documentando cada ganho.
- *Mestre:* registry de imagens simples, compatível com a especificação OCI o suficiente para um `docker pull` funcionar.

**Trade-off a documentar.** Container vs. VM vs. microVM (Firecracker): isolamento vs. densidade vs. tempo de partida.

**Artefato.** `containers-lab/`

---

### Capítulo 12 — Orquestração e Sistemas Declarativos (~50h)

**Objetivo.** Entender o **loop de reconciliação** — ideia central da infraestrutura moderna, que reaparece em todo lugar (inclusive em pipelines de ML).

> Kubernetes é estudado aqui como estudo de caso de um sistema declarativo com reconciliação, não como ferramenta a decorar. Essa moldura sobrevive ao Kubernetes.

**Competências.**
- Explicar estado desejado vs. estado observado, e o loop que os aproxima.
- Explicar a arquitetura: API declarativa, armazenamento (etcd), controladores, agente de nó.
- Explicar escalonamento, requests/limits, probes, e o comportamento sob pressão de recursos.
- Explicar service discovery, rede de pods e ingress.
- Explicar operadores e recursos customizados — o padrão importa mais que a plataforma.
- Depurar um pod que não sobe, sem copiar respostas do Stack Overflow.

**Materiais.**
- *Obrigatório:* Burns, Beda & Hightower — **Kubernetes: Up and Running** + Burns et al. — *Borg, Omega, and Kubernetes* (paper curto, explica o *porquê*).
- *Complementar:* Kubernetes *Concepts* (a doc conceitual é boa); Hightower — *Kubernetes the Hard Way*.
- *Opcional:* Verma et al. — *Large-scale cluster management at Google with Borg*.

**Projetos.**
- *Fundamental:* mini-orquestrador próprio — estado desejado num arquivo, loop de reconciliação, reinício de processos mortos, health check. **É o coração do capítulo.**
- *Intermediário:* Kubernetes the Hard Way completo (cluster do zero, sem instalador).
- *Mestre:* operador/controlador customizado que reconcilie um recurso próprio.

**Trade-off a documentar.** Quando Kubernetes é excesso de engenharia: abaixo de que tamanho ele custa mais do que entrega.

**Artefato.** `orchestration-lab/`

---

### Capítulo 13 — Infraestrutura como Código (~30h)

**Objetivo.** Tratar infraestrutura como software versionado, testável e reversível.

**Competências.**
- Explicar declarativo vs. imperativo, e gestão de estado (o arquivo de estado é a parte difícil).
- Estruturar módulos reutilizáveis, ambientes e composição.
- Explicar drift, importação de recursos existentes, e por que mudanças manuais são dívida.
- Planejar, revisar e aplicar mudanças com segurança; projetar rollback.
- Explicar GitOps como modelo de entrega (e reconhecer o loop de reconciliação de novo).
- Gerenciar segredos sem colocá-los no repositório.

**Materiais.**
- *Obrigatório:* Morris — **Infrastructure as Code** (2ª ed.) + documentação conceitual do Terraform/OpenTofu sobre estado.
- *Complementar:* princípios de GitOps; documentação do ArgoCD ou Flux.
- *Opcional:* Pulumi/CDK, para ver a abordagem de linguagem de propósito geral.

**Projetos.**
- *Fundamental:* toda a infraestrutura da sua Fase 0 recriada 100% em código, destruível e recriável por comando.
- *Intermediário:* módulos com três ambientes (dev/stage/prod) e pipeline de plan/apply revisado.
- *Mestre:* mini-GitOps — um agente que observa um repositório e reconcilia o estado real, reusando o Capítulo 12.

**Trade-off a documentar.** DSL declarativa vs. linguagem de propósito geral para IaC.

**Artefato.** `iac-lab/`

---

### Capítulo 14 — Observabilidade (~40h)

**Objetivo.** Poder responder perguntas que você não previu, sobre um sistema que você não está olhando.

**Competências.**
- Distinguir monitoramento (perguntas conhecidas) de observabilidade (perguntas novas).
- Explicar os três sinais — métricas, logs, traces — e o que cada um custa.
- Explicar cardinalidade e por que ela é a causa mais comum de contas absurdas.
- Instrumentar com contexto de correlação e propagação de trace.
- Projetar alertas por sintoma (SLO) e não por causa; evitar fadiga de alerta.
- Construir dashboards que respondem perguntas em vez de decorar paredes.

**Materiais.**
- *Obrigatório:* Majors, Fong-Jones & Miranda — **Observability Engineering** + documentação conceitual do OpenTelemetry.
- *Complementar:* Google SRE, caps. de monitoramento e alerta; Sigelman et al. — *Dapper*.
- *Opcional:* Prometheus — documentação de modelo de dados e PromQL.

**Projetos.**
- *Fundamental:* biblioteca de métricas própria (contador, gauge, histograma) com agregação e endpoint de exposição — implementar antes de usar Prometheus.
- *Intermediário:* tracing distribuído manual em 3 serviços, com propagação de contexto e visualização de span.
- *Mestre:* diagnosticar um problema real de latência de cauda usando apenas telemetria, e escrever o post-mortem.

**Trade-off a documentar.** Amostragem de traces: head-based vs. tail-based, custo vs. cobertura de casos raros.

**Artefato.** `observability-lab/`

---

### Capítulo 15 — Segurança, Identidade e Acesso (~30h)

**Objetivo.** Pensar adversarialmente sobre infraestrutura.

**Competências.**
- Explicar autenticação vs. autorização; OAuth2, OIDC, JWT e suas armadilhas.
- Projetar identidade de máquina (roles, service accounts, federação, credenciais de curta duração).
- Aplicar menor privilégio de verdade, incluindo em pipelines de CI.
- Gerenciar segredos e fazer rotação sem downtime.
- Projetar segurança de rede: segmentação, egress, mTLS, zero trust.
- Explicar cadeia de suprimentos: SBOM, assinatura de imagem, dependências.
- Aplicar modelagem de ameaças a um sistema próprio.

**Materiais.**
- *Obrigatório:* Anderson — **Security Engineering** (gratuito, caps. relevantes) + OWASP Top 10 e OWASP Cloud-Native Security.
- *Complementar:* Rice — *Container Security*; NIST SP 800-207 (Zero Trust Architecture).
- *Opcional:* Cryptopals sets 1–2, para entender criptografia quebrando-a.

**Projetos.**
- *Fundamental:* fluxo OIDC implementado do zero (cliente e validação de token), sem biblioteca de alto nível.
- *Intermediário:* eliminar todos os segredos de longa duração de um sistema seu, substituindo por credenciais efêmeras.
- *Mestre:* modelagem de ameaças completa de um sistema seu, com três mitigações implementadas e testadas.

**Trade-off a documentar.** Segurança de perímetro vs. zero trust: custo operacional vs. superfície de ataque.

**Artefato.** `cloud-security-lab/`

---

### Capítulo 16 — Custo, Capacidade e Performance (~25h)

**Objetivo.** Tratar dinheiro como requisito não-funcional de primeira classe.

**Competências.**
- Modelar o custo de uma arquitetura antes de construí-la.
- Explicar os vetores de custo em nuvem (compute, egress, storage por classe, requisições) e onde estão as surpresas.
- Fazer planejamento de capacidade com base em medição e na lei de Little.
- Projetar autoscaling que funciona (métrica certa, histerese, tempo de aquecimento).
- Comparar decisões arquiteturais por custo total, não só por elegância.

**Materiais.**
- *Obrigatório:* Storment & Fuller — **Cloud FinOps** (caps. principais) + Gregg — *Systems Performance* (metodologia USE).
- *Complementar:* calculadoras de preço dos provedores, usadas com seriedade.
- *Opcional:* Dean — *Latency Numbers Every Programmer Should Know*.

**Projetos.**
- *Fundamental:* modelo de custo de uma arquitetura sua em três escalas de tráfego (1x, 100x, 10.000x), identificando o ponto de ruptura.
- *Intermediário:* reduzir o custo de um sistema seu em 50% sem degradar SLO, documentando cada decisão.
- *Mestre:* teste de carga com dimensionamento derivado de medição, validando a previsão contra o real.

**Trade-off a documentar.** Provisionado vs. sob demanda vs. spot: risco, custo e complexidade.

**Artefato.** `cost-and-capacity-lab/`

---

### ✅ Conclusão da Fase III

- [ ] mapear qualquer serviço gerenciado em uma primitiva e explicar o problema clássico que ele resolve;
- [ ] implementar isolamento de processo com namespaces e cgroups;
- [ ] escrever um loop de reconciliação e explicar Kubernetes através dele;
- [ ] recriar toda a sua infraestrutura por comando, do zero;
- [ ] diagnosticar um problema não previsto usando só telemetria;
- [ ] eliminar segredos de longa duração de um sistema;
- [ ] prever o custo de uma arquitetura antes de construí-la.

---

# Fase IV — Machine Learning de Verdade

**~300h · Objetivo: entender como modelos aprendem, saber construí-los, e conseguir ler e avaliar o que aparece de novo.**

> Esta fase existe porque operar ML sem entender ML é uma posição frágil. Você não vai virar pesquisador aqui — não há teoria de aprendizado estatístico avançada nem pré-treinamento em escala. Mas ao fim dela você treina modelos, implementa arquiteturas do zero, diagnostica por que o modelo está errado, lê um paper novo e decide se ele se aplica ao seu caso.
>
> **A ordem importa:** matemática aplicada → o que é aprender → modelos clássicos → deep learning → transformers → ler papers. Pular para transformers direto produz alguém que monta pipelines e não sabe consertá-los.

---

### Capítulo 17 — Matemática Aplicada a ML (~55h)

**Objetivo.** Adquirir a maquinaria mínima para entender qualquer modelo por dentro — sem virar um curso de matemática pura.

> Este é o capítulo que as pessoas pulam e do qual todas as dificuldades posteriores derivam. Ele não precisa ser rigoroso; precisa ser **operacional** — você deve conseguir mexer nas expressões, não provar teoremas.

**Competências.**

*Álgebra linear*
- Interpretar uma matriz como transformação geométrica, não como tabela.
- Explicar posto, projeção, espaço nulo e o que dimensionalidade significa na prática.
- Explicar autovalores/autovetores e SVD geometricamente; derivar PCA a partir da SVD.
- Raciocinar sobre custo computacional de operações matriciais (o que conecta com o Capítulo 1).

*Probabilidade e estatística*
- Trabalhar com variáveis aleatórias, distribuições, esperança e variância.
- Aplicar Bayes em problemas não triviais.
- Derivar estimadores por máxima verossimilhança.
- Usar desigualdades de concentração para raciocinar sobre tamanho de amostra.
- Distinguir estimativa pontual, intervalo de confiança e teste de hipótese — e saber por que p-valor não é o que a maioria pensa.

*Cálculo e otimização*
- Calcular gradientes e jacobianas de funções vetoriais.
- Explicar a regra da cadeia sobre grafos de computação (a base literal de backprop).
- Distinguir problemas convexos de não-convexos e o que isso implica.
- Derivar e implementar gradiente descendente, momentum, e métodos adaptativos.

**Materiais.**
- *Obrigatório:* Deisenroth, Faisal & Ong — **Mathematics for Machine Learning** (gratuito; escrito exatamente para este propósito) + 3Blue1Brown — *Essence of Linear Algebra* e *Essence of Calculus* (para intuição, antes do livro).
- *Complementar:* Blitzstein & Hwang — *Introduction to Probability* (caps. 1–7); Boyd & Vandenberghe — *Convex Optimization*, caps. 1–3 e 9.
- *Opcional:* The Matrix Cookbook (referência de identidades); Strang — MIT 18.06.

**Projetos.**
- *Fundamental:* implementar do zero — multiplicação de matrizes, eliminação gaussiana, power iteration, SVD — e usar a SVD para comprimir uma imagem, com curva de erro vs. posto.
- *Intermediário:* implementar e comparar GD, SGD, momentum, AdaGrad, RMSProp e Adam em funções-teste mal condicionadas, com visualização de trajetória.
- *Mestre:* **diferenciação automática em modo reverso do zero**, sobre um mini grafo de computação. Este projeto é a semente direta do Capítulo 20 — faça com capricho.

**Trade-off a documentar.** Solução fechada vs. iterativa: quando cada uma é viável, e por que praticamente todo ML moderno usa a segunda.

**Artefato.** `ml-math-lab/`

---

### Capítulo 18 — O que é Aprender: Generalização e Avaliação (~50h)

**Objetivo.** Entender por que aprender a partir de dados finitos funciona — e, mais importante, quando não funciona.

> Se você fizer só um capítulo desta fase, faça este. Arquiteturas mudam a cada dois anos; a capacidade de saber se algo realmente funcionou não muda nunca. É também o capítulo que mais protege você de ser enganado — por papers, por vendedores e por si mesmo.

**Competências.**

*Generalização*
- Explicar treino/validação/teste e por que o teste é sagrado.
- Explicar o trade-off viés-variância com precisão, e reconhecê-lo em curvas de aprendizado.
- Explicar regularização como restrição do espaço de hipótese, não como truque.
- Explicar a maldição da dimensionalidade e o teorema do almoço grátis.
- Explicar (conceitualmente) aprendizado PAC e capacidade de modelo.
- Reconhecer o regime moderno onde a intuição clássica quebra (double descent).

*Avaliação*
- Escolher métricas e explicar o que cada uma esconde: acurácia em dados desbalanceados, precision vs. recall, ROC vs. PR, F1 e suas armadilhas, calibração.
- Projetar validação cruzada correta — inclusive para séries temporais e dados agrupados.
- Detectar vazamento de dados (data leakage) em suas várias formas sutis.
- Construir baselines honestas e reconhecer quando a baseline trivial vence.
- Reportar com variância: múltiplas sementes, intervalos de confiança, testes estatísticos apropriados.
- Projetar testes A/B e entender poder estatístico.

**Materiais.**
- *Obrigatório:* Abu-Mostafa, Magdon-Ismail & Lin — **Learning From Data** (livro curto + curso Caltech gratuito; a melhor porta de entrada ao tema) + Google — *Rules of Machine Learning* (curto, gratuito, indispensável).
- *Complementar:* Raschka — *Model Evaluation, Model Selection, and Algorithm Selection*; Kohavi, Tang & Xu — *Trustworthy Online Controlled Experiments*.
- *Opcional:* Belkin et al. — *Reconciling Modern ML and the Bias-Variance Trade-off*; Bouthillier et al. — *Accounting for Variance in ML Benchmarks*.

**Projetos.**
- *Fundamental:* reproduzir empiricamente as curvas clássicas — erro de treino vs. teste vs. complexidade, em dados sintéticos onde você conhece a verdade.
- *Intermediário:* construir três pipelines com vazamentos sutis diferentes (temporal, de grupo, de pré-processamento) e quantificar quanto cada um infla a métrica falsamente.
- *Mestre:* framework de avaliação próprio — múltiplas sementes, intervalos de confiança, relatório reprodutível — que será reutilizado em todos os capítulos seguintes. Depois use-o para mostrar quanto de uma "melhoria" publicada é ruído de semente.

**Trade-off a documentar.** Métrica única vs. conjunto de métricas: o custo de otimizar para um número só (e o de não ter um número só).

**Artefato.** `learning-and-evaluation-lab/`

---

### Capítulo 19 — Modelos Clássicos, Implementados do Zero (~55h)

**Objetivo.** Dominar o repertório que ainda resolve a maioria dos problemas reais — implementando, não importando.

> Realidade frequentemente ignorada: **em dados tabulares, gradient boosting ainda vence redes neurais na maioria dos casos.** Saber disso, e por quê, é parte do que torna alguém resistente a modismo.

**Competências.**
- Implementar do zero e explicar matematicamente: regressão linear (fechada e iterativa), regressão logística, k-NN, Naive Bayes, árvores de decisão (CART), random forest, gradient boosting, SVM com kernel.
- Implementar k-means, misturas gaussianas com EM, e PCA.
- Para cada modelo: que suposições faz, quando quebra, como escala, e como interpretar.
- Explicar por que ensembles funcionam.
- Escolher a família de modelo adequada ao regime de dados (volume, dimensionalidade, ruído, interpretabilidade exigida).

**Materiais.**
- *Obrigatório:* James et al. — **An Introduction to Statistical Learning** (ISL, gratuito) como base + Hastie, Tibshirani & Friedman — *The Elements of Statistical Learning* (ESL, gratuito) para profundidade nos capítulos que interessarem.
- *Complementar:* Murphy — *Probabilistic Machine Learning: An Introduction*; Bishop — *Pattern Recognition and Machine Learning*, cap. 9 (EM).
- *Opcional:* Breiman — *Statistical Modeling: The Two Cultures* (leitura curta e filosoficamente importante).

**Projetos.**
- *Fundamental:* `mini-sklearn` — todos os modelos acima implementados do zero, com API consistente e testes de equivalência contra o scikit-learn.
- *Intermediário:* gradient boosting do zero, comparado honestamente com XGBoost/LightGBM — incluindo uma explicação de por que eles são mais rápidos.
- *Mestre:* estudo comparativo em 5 datasets reais — que família vence em que regime, com análise das causas, usando o framework de avaliação do Capítulo 18.

**Trade-off a documentar.** Boosting vs. rede neural em dados tabulares: por que a intuição "deep learning é sempre melhor" está errada aqui.

**Artefato.** `classical-ml-lab/`

---

### Capítulo 20 — Deep Learning do Zero (~65h)

**Objetivo.** Nunca mais ver um framework como caixa-preta — e saber treinar redes que realmente convergem.

**Competências.**

*Construir o framework*
- Implementar tensores, grafo de computação, autograd em modo reverso, camadas, funções de perda e otimizadores.
- Derivar backprop à mão para redes arbitrárias e validar gradientes numericamente.

*Treinar de verdade*
- Diagnosticar e resolver: gradientes que somem ou explodem, inicialização ruim, taxa de aprendizado errada, overfitting.
- Explicar e implementar batch/layer normalization, dropout, weight decay, gradient clipping, schedules de learning rate e warmup.
- Explicar por que conexões residuais permitiram profundidade.
- Entender precisão mista e o custo de memória do treino (ativações, gradientes, estados do otimizador).

*Arquiteturas*
- Implementar convolução, pooling e uma ResNet; explicar campo receptivo e viés indutivo.
- Implementar RNN/LSTM e sentir o problema de dependências longas — pré-requisito emocional para o capítulo seguinte.
- Aplicar transfer learning e fine-tuning com critério.

**Materiais.**
- *Obrigatório:* Karpathy — **Neural Networks: Zero to Hero** (série em vídeo; excepcional, e é literalmente construir o framework) + Goodfellow, Bengio & Courville — *Deep Learning* (gratuito), caps. 6–8 e 11.
- *Complementar:* Karpathy — *A Recipe for Training Neural Networks*; Google — *Deep Learning Tuning Playbook* (gratuito, muito prático); Stanford CS231n.
- *Opcional:* Olah — *Understanding LSTM Networks*; código do micrograd e do tinygrad.

**Projetos.**
- *Fundamental:* mini-framework de deep learning próprio — tensores n-dimensionais, autograd, camadas, otimizadores — com gradientes validados contra o PyTorch.
- *Intermediário:* laboratório de ablação — mesma rede variando inicialização, normalização, LR e regularização, com resultados tabelados e múltiplas sementes (Capítulo 18 aplicado). Depois: CNN do zero e uma ResNet, demonstrando empiricamente que a conexão residual é o que permite profundidade.
- *Mestre:* pegar um treino instável e torná-lo estável, documentando o diagnóstico como um caso clínico. Ou: adicionar suporte a GPU ao seu framework.

**Trade-off a documentar.** Modelo maior vs. mais dados vs. melhor regularização: como decidir onde investir quando a métrica trava.

**Artefato.** `deep-learning-lab/`

---

### Capítulo 21 — Transformers e Modelos de Linguagem por Dentro (~50h)

**Objetivo.** Implementar a arquitetura dominante linha por linha, e entender o que realmente acontece num LLM.

> Depois deste capítulo, um paper de arquitetura nova vira leitura de fim de semana em vez de barreira.

**Competências.**
- Derivar atenção como recuperação suave por similaridade — e entender o problema que ela resolve (você já sofreu com ele no Capítulo 20).
- Implementar self-attention, multi-head attention, atenção causal com máscara, codificação posicional (absoluta, aprendida, RoPE) e a arquitetura decoder-only completa.
- Explicar a complexidade quadrática e as variantes que a atacam (FlashAttention, MQA/GQA, atenção esparsa).
- Implementar tokenização BPE do zero e explicar suas consequências (números, código, línguas não-inglesas).
- Explicar pré-treinamento, leis de escala (Kaplan, Chinchilla) e o trade-off compute-ótimo.
- Explicar pós-treinamento: SFT, LoRA, RLHF e DPO — e implementar ao menos SFT com LoRA.
- Explicar amostragem (temperatura, top-p) como o que é: manipulação de uma distribuição de probabilidade.
- Explicar por que inferência de LLM é limitada por largura de banda de memória, não por FLOPs.

**Materiais.**
- *Obrigatório:* Vaswani et al. — **Attention Is All You Need** + Karpathy — *Let's build GPT* (vídeo) + *The Annotated Transformer* (Harvard).
- *Complementar:* Hoffmann et al. — *Training Compute-Optimal LLMs* (Chinchilla); Ouyang et al. — *InstructGPT*; Hu et al. — *LoRA*; Rafailov et al. — *DPO*.
- *Opcional:* Elhage et al. — *A Mathematical Framework for Transformer Circuits*; Stanford CS336 (*Language Modeling from Scratch*); Dao et al. — *FlashAttention*.

**Projetos.**
- *Fundamental:* nanoGPT reimplementado do zero (não copiado), treinado num corpus pequeno, com tokenizador BPE próprio.
- *Intermediário:* implementar KV cache e comparar variantes de atenção (MHA vs. GQA) em qualidade e velocidade; pré-treinar um modelo de 10–50M parâmetros e reproduzir uma lei de escala em miniatura.
- *Mestre:* SFT com LoRA em um modelo aberto pequeno, avaliado antes/depois com o framework do Capítulo 18. Ou: implementar DPO do zero.

**Trade-off a documentar.** Fine-tuning vs. RAG vs. prompt engineering: qual problema cada um realmente resolve, e o custo de cada.

**Artefato.** `transformers-lab/`

---

### Capítulo 22 — Ler, Reproduzir e Avaliar Papers (~25h)

**Objetivo.** Transformar a literatura de barreira em ferramenta de trabalho — que é o que mantém você atualizado sem depender de threads de Twitter.

**Competências.**
- Ler um paper em três passadas e extrair a contribuição real em 15 minutos.
- Identificar o que o paper *não* mostrou: baselines ausentes, avaliação frágil, ganho dentro do ruído, escolha conveniente de benchmark.
- Reproduzir um resultado a partir do texto, sem o código dos autores.
- Traduzir "isso é interessante" em "isso se aplica ao meu caso, e aqui está o teste que provaria".
- Manter um fluxo sustentável de leitura sem virar refém do hype.

**Materiais.**
- *Obrigatório:* Keshav — *How to Read a Paper* (2 páginas) + Lipton & Steinhardt — *Troubling Trends in Machine Learning Scholarship*.
- *Complementar:* Recht et al. — *Do ImageNet Classifiers Generalize to ImageNet?*; revisões públicas no OpenReview (ler revisores discordando é muito educativo).
- *Opcional:* participar de um clube de leitura, ou simular um por escrito.

**Projetos.**
- *Fundamental:* 15 resenhas críticas de papers, uma página cada — "a contribuição foi X, a evidência é Y, a fraqueza é Z, se aplica ao meu contexto quando W".
- *Intermediário:* reproduzir 2 papers do zero, documentando cada divergência encontrada e sua causa provável.
- *Mestre:* pegar um resultado recente e testar se ele se sustenta no seu domínio — com avaliação própria e conclusão honesta, inclusive se a conclusão for "não se sustenta".

**Trade-off a documentar.** Adotar cedo vs. esperar maturidade: como decidir, com critérios explícitos em vez de instinto.

**Artefato.** `paper-reviews/`

---

### ✅ Conclusão da Fase IV

- [ ] derivar gradientes e implementar otimizadores do zero;
- [ ] explicar generalização, viés-variância e regularização com precisão;
- [ ] **projetar uma avaliação que você defenderia publicamente**;
- [ ] implementar o repertório clássico sem bibliotecas, e saber quando ele vence deep learning;
- [ ] construir um framework de deep learning funcional;
- [ ] implementar transformers linha por linha e treinar um modelo de linguagem pequeno;
- [ ] diagnosticar **por que** um modelo está errado, não só que está;
- [ ] ler um paper novo e decidir, com evidência, se ele se aplica ao seu caso.

**Artefatos:** `ml-math-lab/`, `learning-and-evaluation-lab/`, `classical-ml-lab/`, `deep-learning-lab/`, `transformers-lab/`, `paper-reviews/`

---

# Fase V — ML em Produção

**~210h · Objetivo: transformar modelos em sistemas confiáveis, observáveis e reversíveis.**

Agora que você entende ML por dentro, esta fase é sobre o que o torna difícil de operar. O que distingue isto de DevOps comum é específico e enumerável:

1. **O artefato depende de dados** — o pipeline de dados é parte do sistema, não um pré-passo.
2. **O comportamento é probabilístico** — testes determinísticos não bastam.
3. **A degradação é silenciosa** — um modelo quebrado tem 100% de disponibilidade, latência normal e zero erros.
4. **Treino e serving são caros de formas diferentes** — custo é decisão arquitetural.
5. **Reprodutibilidade exige versionar dados, código, ambiente e aleatoriedade juntos.**

Todo o resto é a Fase III aplicada.

---

### Capítulo 23 — Dados: Pipelines, Contratos e Features (~45h)

**Objetivo.** Aceitar que a maior parte do resultado — e dos incidentes — vem dos dados.

**Competências.**
- Projetar pipelines em lote e em fluxo, com idempotência e reprocessamento.
- Definir contratos de dados e validar esquema **e distribuição** na entrada.
- Versionar dados e garantir reprodutibilidade de um conjunto de treino específico.
- Explicar e prevenir **training-serving skew** — a causa raiz mais comum de fracasso em produção.
- Explicar vazamento temporal e *point-in-time correctness*.
- Explicar o que uma feature store realmente resolve (e quando é desnecessária).
- Auditar viés de amostragem e entender como ele se propaga para o modelo.

**Materiais.**
- *Obrigatório:* Huyen — **Designing Machine Learning Systems**, caps. 3–5 + Kleppmann — DDIA, caps. 10–11.
- *Complementar:* Reis & Housley — *Fundamentals of Data Engineering*; Polyzotis et al. — *Data Validation for Machine Learning*.
- *Opcional:* Gebru et al. — *Datasheets for Datasets*; Sculley et al. — *Hidden Technical Debt in ML Systems*.

**Projetos.**
- *Fundamental:* pipeline em lote reprodutível com validação de esquema e distribuição, que falha ruidosamente quando os dados mudam de forma.
- *Intermediário:* provocar training-serving skew deliberadamente (transformação implementada duas vezes) e medir o impacto na métrica em produção.
- *Mestre:* mini feature store própria — features computadas uma vez, servidas em lote e online, com correção point-in-time garantida por teste.

**Trade-off a documentar.** Computar features online vs. pré-computar: latência vs. frescor vs. custo.

**Artefato.** `ml-data-lab/`

---

### Capítulo 24 — Reprodutibilidade e Experimentação (~30h)

**Objetivo.** Garantir que um resultado possa ser refeito — por você, em seis meses, sem os arquivos originais.

**Competências.**
- Versionar conjuntamente código, dados, configuração, ambiente e sementes.
- Rastrear experimentos com metadados suficientes para reconstrução completa.
- Explicar as fontes de não-determinismo (semente, ordem de dados, paralelismo, GPU, versão de biblioteca) e controlá-las quando necessário.
- Construir um registro de modelos com linhagem: dado este modelo, saber exatamente de onde ele veio.
- Distinguir experimento exploratório de experimento que vira produção — com processos diferentes para cada.

**Materiais.**
- *Obrigatório:* Huyen — *Designing ML Systems*, cap. 6 + documentação conceitual do MLflow (modelo de tracking e registry).
- *Complementar:* Pineau et al. — *Improving Reproducibility in Machine Learning Research* (checklist NeurIPS); documentação do DVC.
- *Opcional:* Bouthillier et al. — *Accounting for Variance in ML Benchmarks*.

**Projetos.**
- *Fundamental:* registro de experimentos e de modelos próprio, com linhagem consultável — implementar antes de usar MLflow.
- *Intermediário:* reproduzir um experimento antigo seu apenas a partir dos metadados registrados; documentar tudo o que faltou.
- *Mestre:* pipeline com reprodutibilidade bit-a-bit garantida — ou uma análise honesta de por que ela é impossível no seu caso, e o que se pode garantir no lugar.

**Trade-off a documentar.** Reprodutibilidade exata vs. estatística: qual você realmente precisa, e o que cada uma custa.

**Artefato.** `reproducibility-lab/`

---

### Capítulo 25 — Pipelines de Treino e Orquestração (~35h)

**Objetivo.** Transformar "rodei um notebook" em "o sistema treina sozinho, de forma auditável".

**Competências.**
- Modelar pipelines como DAGs, com passos idempotentes e retomáveis.
- Escolher gatilhos de retreino (agendado, por drift, por volume) com justificativa.
- Gerenciar recursos de treino: GPU, spot instances, checkpointing, tolerância a interrupção.
- Explicar treino distribuído (paralelismo de dados) em conceito e custo.
- Instrumentar pipelines de treino como qualquer sistema de produção.
- Automatizar busca de hiperparâmetros sem enganar a si mesmo (Capítulo 18 aplicado).

**Materiais.**
- *Obrigatório:* Huyen — *Designing ML Systems*, caps. 6–7 + documentação conceitual de um orquestrador (Airflow, Dagster ou Kubeflow — escolha um).
- *Complementar:* Google — *MLOps: Continuous delivery and automation pipelines in ML* (whitepaper com níveis de maturidade).
- *Opcional:* documentação de DDP do PyTorch.

**Projetos.**
- *Fundamental:* orquestrador de DAG próprio — dependências, retomada após falha, retry, logs por passo. Reusa diretamente o loop de reconciliação do Capítulo 12.
- *Intermediário:* pipeline de retreino automático disparado por drift, com aprovação humana antes da promoção.
- *Mestre:* treino em spot instances com checkpointing que sobrevive à interrupção, e análise de economia real vs. tempo perdido.

**Trade-off a documentar.** Retreino agendado vs. disparado por drift: custo, risco e complexidade operacional.

**Artefato.** `training-pipelines-lab/`

---

### Capítulo 26 — Serving e Inferência (~40h)

**Objetivo.** Colocar modelos no caminho crítico de requisições reais, com garantias.

**Competências.**
- Escolher entre inferência em lote, online e em fluxo com base no requisito, não no hábito.
- Otimizar latência: batching dinâmico, cache, pré-computação, quantização, escolha de hardware.
- Explicar e medir por que inferência de modelos grandes é limitada por memória (Capítulo 21 aplicado).
- Dimensionar capacidade de GPU e projetar autoscaling com custo alto de aquecimento.
- Empacotar modelos de forma portátil (ONNX, TorchScript, contêiner com contrato claro).
- Projetar a API do modelo: contrato de entrada, validação, versionamento e degradação (fallback para modelo anterior ou regra simples).

**Materiais.**
- *Obrigatório:* Huyen — *Designing ML Systems*, cap. 7 + documentação conceitual de um servidor de inferência (Triton, KServe ou vLLM).
- *Complementar:* Crankshaw et al. — *Clipper*; Kwon et al. — *PagedAttention (vLLM)*; Pope et al. — *Efficiently Scaling Transformer Inference*.
- *Opcional:* Dettmers et al. — *LLM.int8()*; literatura de destilação.

**Projetos.**
- *Fundamental:* servidor de inferência próprio com batching dinâmico, medindo o ganho de throughput contra o custo em latência p99.
- *Intermediário:* quantizar um modelo do zero (INT8 e INT4) e medir a degradação de qualidade contra o ganho de memória — usando os Capítulos 17, 18 e 21.
- *Mestre:* otimizar um serviço de inferência em 5x com SLO preservado, medindo cada ganho isoladamente.

**Trade-off a documentar.** Batching: o gráfico throughput vs. latência de cauda, com a escolha justificada por SLO.

**Artefato.** `model-serving-lab/`

---

### Capítulo 27 — Monitoramento, Drift e Qualidade em Produção (~35h)

**Objetivo.** Detectar o problema específico de ML: o sistema está perfeitamente saudável e as predições estão erradas.

> Este é o capítulo que mais distingue ML em produção de operação de software comum.

**Competências.**
- Distinguir e monitorar: drift de dados, drift de conceito, drift de predição e queda de desempenho real.
- Escolher testes de drift apropriados (KS, PSI, distância de Wasserstein) e entender seus falsos positivos.
- Projetar coleta de rótulos atrasados e medir desempenho quando o rótulo chega semanas depois.
- Monitorar calibração, não só acurácia.
- Definir SLOs de qualidade de predição, não só de latência.
- Projetar alertas de qualidade que não geram fadiga.
- Detectar loops de feedback, onde o modelo influencia os dados que receberá depois.
- **Distinguir degradação real de ruído estatístico** — isso exige o Capítulo 18.

**Materiais.**
- *Obrigatório:* Huyen — *Designing ML Systems*, cap. 8 + Breck et al. — *The ML Test Score*.
- *Complementar:* documentação conceitual de ferramentas de monitoramento de ML (Evidently, WhyLabs) — pelo modelo mental, não pela ferramenta.
- *Opcional:* literatura sobre feedback loops e viés em sistemas de recomendação.

**Projetos.**
- *Fundamental:* detector de drift próprio, com múltiplos testes estatísticos, aplicado a um fluxo com drift injetado — incluindo a taxa de falso positivo de cada teste.
- *Intermediário:* sistema de avaliação em produção com rótulos atrasados, calculando desempenho retroativo em janela móvel.
- *Mestre:* simular um loop de feedback (um recomendador que treina nos próprios cliques) e demonstrar o colapso de diversidade que ele causa.

**Trade-off a documentar.** Sensibilidade de alerta de drift: falso positivo (fadiga) vs. falso negativo (prejuízo silencioso).

**Artefato.** `ml-monitoring-lab/`

---

### Capítulo 28 — Entrega Contínua de Modelos (~25h)

**Objetivo.** Promover, testar e reverter modelos com a mesma disciplina aplicada a código — e mais.

**Competências.**
- Projetar testes para sistemas de ML: de dados, de features, de modelo, de infraestrutura, de invariância e de comportamento.
- Implementar estratégias de lançamento: shadow, canário, A/B, interleaving, rollout progressivo.
- Projetar reversão de modelo — incluindo os casos em que reverter não resolve, porque o dado já mudou.
- Construir portões de aprovação automáticos por métrica de qualidade e de negócio.
- Aplicar o ML Test Score a um sistema seu.

**Materiais.**
- *Obrigatório:* Breck et al. — **The ML Test Score** + Ribeiro et al. — *Beyond Accuracy: Behavioral Testing of NLP Models* (CheckList).
- *Complementar:* Kohavi, Tang & Xu — *Trustworthy Online Controlled Experiments*.
- *Opcional:* ThoughtWorks — *Continuous Delivery for Machine Learning*.

**Projetos.**
- *Fundamental:* suíte de testes completa para um modelo seu — dados, comportamento, invariância, regressão — rodando em CI.
- *Intermediário:* deploy shadow de um modelo novo, comparando predições contra produção sem afetar usuários.
- *Mestre:* pipeline de promoção com canário automático, portão por métrica e reversão automática ao violar SLO.

**Trade-off a documentar.** Shadow vs. canário vs. A/B: o que cada um detecta, quanto custa, quanto tempo leva.

**Artefato.** `ml-delivery-lab/`

---

### ✅ Conclusão da Fase V

- [ ] prevenir training-serving skew e vazamento por construção;
- [ ] reproduzir um experimento antigo a partir dos metadados;
- [ ] operar pipelines de treino como sistemas de produção;
- [ ] servir modelos dentro de um SLO e justificar o custo;
- [ ] detectar degradação silenciosa antes do time de negócio — e distingui-la de ruído;
- [ ] promover e reverter modelos com segurança.

---

# Fase VI — O Ofício do Arquiteto

**~150h · Objetivo: decidir bem, ler o que outros construíram, e comunicar melhor — que é o trabalho de verdade.**

Comece esta fase em paralelo, a partir da Fase II. Ela não é um capítulo final; é uma prática contínua.

---

### Capítulo 29 — Escrever Decisões (~35h)

**Objetivo.** Produzir os artefatos pelos quais arquitetos são realmente avaliados.

**Competências.**
- Escrever ADRs que registram contexto, opções, decisão e consequências.
- Escrever design docs / RFCs que sobrevivem à revisão de pessoas céticas.
- Escrever post-mortem sem culpa que gere mudança real.
- Construir diagramas em níveis (modelo C4) e saber qual nível serve a qual conversa.
- Apresentar trade-offs a públicos técnicos e não-técnicos sem distorcer.
- Escrever a proposta que você **não** escolheu, com força suficiente para ser convincente.

**Materiais.**
- *Obrigatório:* Nygard — **Documenting Architecture Decisions** (post curto e seminal); Brown — *Software Architecture for Developers* (modelo C4).
- *Complementar:* Google — *Design Docs at Google*; Google SRE, cap. de post-mortem.
- *Opcional:* RFCs públicas de empresas de engenharia (as Rust RFCs são um ótimo modelo de forma).

**Projetos.**
- *Fundamental:* 10 ADRs cobrindo decisões reais dos seus artefatos anteriores.
- *Intermediário:* design doc completo de um sistema seu, com diagramas C4 em três níveis e uma seção honesta de alternativas rejeitadas.
- *Mestre:* escrever duas propostas opostas para a mesma decisão, ambas convincentes, e depois um documento decidindo entre elas com critérios explícitos.

**Artefato.** `architecture-decisions/`

---

### Capítulo 30 — Estudos de Caso e Leitura de Sistemas (~30h)

**Objetivo.** Aprender com sistemas que já existem, em vez de reinventar por conta própria.

**Competências.**
- Ler um paper de sistemas e extrair a decisão central e seu custo.
- Analisar criticamente uma arquitetura pública, separando o essencial do contingente ao contexto daquela empresa.
- Reconhecer quando uma prática de uma empresa gigante é ativamente prejudicial na sua escala.
- Ler relatórios de incidente e extrair princípios generalizáveis.

**Materiais.**
- *Obrigatório:* papers clássicos — **Dynamo**, **Bigtable**, **MapReduce**, **Borg**, **Kafka**, **Spanner**, **Dapper**, **Chubby**.
- *Complementar:* blogs de engenharia (Netflix, Uber, Stripe, Cloudflare) lidos com ceticismo produtivo; relatórios públicos de incidentes.
- *Opcional:* o método do Capítulo 22 aplicado a papers de sistemas.

**Projetos.**
- *Fundamental:* resenha crítica de 10 papers/arquiteturas — "a decisão central foi X, e custou Y".
- *Intermediário:* análise escrita de três arquiteturas públicas, separando o essencial do contingente à escala.
- *Mestre:* reproduzir uma ideia central de um paper (quóruns do Dynamo, modelo de log do Kafka) em escala pequena.

**Artefato.** `systems-case-studies/`

---

### Capítulo 31 — Ler e Auditar Sistemas que Você Não Escreveu (~40h)

**Objetivo.** Construir um modelo mental confiável de um sistema grande e desconhecido, rápido — e saber onde ele mente.

> Toda a formação até aqui é "implemente X". Este capítulo é o inverso, e a assimetria é intencional: na prática profissional você lê muito mais do que escreve, e a proporção só aumenta. É também a defesa concreta contra a única previsão de futuro que parece segura — a de que cada vez mais código chegará até você pronto, vindo de outra pessoa, de um repositório antigo ou de um agente. Ler bem é o que permite ser responsável por algo que você não digitou.

**Competências.**

*Mapear*
- Entrar num repositório desconhecido e, em poucas horas, identificar: pontos de entrada, fluxo principal, estruturas de dados centrais e fronteiras de módulo.
- Alternar entre as três estratégias de leitura conforme o caso: **top-down** (do ponto de entrada para baixo), **bottom-up** (das estruturas de dados para cima) e **seguir uma requisição** de ponta a ponta.
- Usar o histórico do git como arqueologia: `blame`, `log -S`, evolução de um arquivo, e o que os commits revelam sobre decisões esquecidas.
- Distinguir o que o sistema *diz* que faz (documentação, nomes, comentários) do que ele *faz* — e reconhecer que quando divergem, a documentação está errada.

*Julgar*
- Avaliar a qualidade de uma base de código em pouco tempo: cobertura e qualidade dos testes, complexidade das fronteiras, acoplamento, sinais de dívida.
- Identificar as decisões arquiteturais implícitas que ninguém registrou.
- Reconhecer complexidade essencial (do problema) vs. acidental (de quem escreveu).

*Diagnosticar*
- Depurar um bug em código que você não entende, sem ler tudo antes.
- Formular e testar hipóteses em vez de ler linearmente.
- Escrever um teste que reproduz o problema antes de tentar corrigi-lo.

*Auditar*
- Revisar código gerado por outro (pessoa ou agente) com ceticismo produtivo: procurar o caso não tratado, a suposição não declarada, o teste que passa por acaso.
- Reconhecer o modo de falha típico de código gerado: plausível, bem-formatado, e errado numa condição de borda que ninguém testou.
- Decidir o que é seguro aceitar sem ler linha a linha, e o que exige leitura integral.

**Materiais.**
- *Obrigatório:* Feathers — **Working Effectively with Legacy Code** (o livro é sobre mudar código que você não entende, que é exatamente o problema) + Spinellis — *Code Reading: The Open Source Perspective*.
- *Complementar:* Brown & Wilson (eds.) — *The Architecture of Open Source Applications* (gratuito, 4 volumes de sistemas reais explicados pelos autores); Ousterhout — *A Philosophy of Software Design* (relido como critério de julgamento, não de escrita).
- *Opcional:* Zeller — *Why Programs Fail* (método científico aplicado a depuração); guias de contribuição de projetos grandes (o do CPython e o do PostgreSQL são bons).

**Projetos.**
- *Fundamental:* mapear **três** projetos open source de tamanhos crescentes (~5k, ~50k, ~500k linhas) e escrever um documento de arquitetura para cada — diagramas C4, fluxo principal, decisões centrais e seus custos. Faça isso **sem** ler a documentação de arquitetura oficial do projeto primeiro; depois compare o seu mapa com a versão oficial e analise onde você errou.
- *Intermediário:* encontrar um bug real num projeto open source, reproduzi-lo com um teste, corrigi-lo e submeter o patch. Documentar o processo de diagnóstico — as hipóteses erradas incluídas.
- *Mestre:* auditoria adversarial. Pegue um sistema de complexidade não trivial gerado por IA (ou herde uma base de código que ninguém mantém) e ache onde ele está errado — condição de borda, race condition, suposição falsa sobre os dados, teste que passa sem verificar nada. Escreva o relatório de auditoria com evidência.

**Trade-off a documentar.** Ler o código vs. ler a documentação vs. medir o comportamento: qual das três responde qual tipo de pergunta, e por que confiar na segunda é o erro mais comum.

**Artefato.** `code-reading-lab/`

---

### Capítulo 32 — Projeto de Síntese (~45h+)

**Objetivo.** Construir uma coisa só, sua, que só existe porque você fez a formação inteira.

**Requisitos.**

1. **Atravessa as fases.** Usa fundamentos (Fase I), coordenação distribuída (Fase II), infraestrutura declarativa e observável (Fase III), **um modelo que você mesmo treinou e entende** (Fase IV) e operação rigorosa dele (Fase V).
2. **Tem SLOs declarados** — de latência e de qualidade de predição — com telemetria que os mede.
3. **Foi quebrado de propósito.** Falha de infraestrutura e drift de dados, com relatório de caos e post-mortem.
4. **É reprodutível.** Um comando recria a infraestrutura; um comando reproduz o modelo.
5. **Tem custo modelado** em três escalas, com o ponto de ruptura arquitetural identificado.
6. **Tem avaliação honesta.** Baseline, variância medida e limitações declaradas.
7. **É escrito.** Design doc, ADRs, e uma análise do que você faria diferente.

**Escopos adequados.**

- Sistema de recomendação de ponta a ponta — modelo treinado por você, feature store, retreino por drift, canário e detecção de loop de feedback.
- Plataforma de predição multi-tenant com isolamento, cota, roteamento entre modelos e observabilidade por tenant.
- Um modelo de linguagem pequeno treinado do zero em um domínio específico, com engine de inferência própria, avaliação rigorosa e operação completa.
- Plataforma interna mínima (mini-PaaS) que recebe um repositório e entrega um serviço observável no ar, implementando você mesmo o loop de reconciliação.

**Artefato.** `capstone/`

---

### ✅ Conclusão da formação

- [ ] escrever uma decisão arquitetural que outra pessoa consegue avaliar;
- [ ] argumentar convincentemente o lado que você não escolheu;
- [ ] ler um paper (de sistemas ou de ML) e extrair o essencial em 30 minutos;
- [ ] mapear um sistema de 500k linhas que você nunca viu e explicar onde ele vai quebrar;
- [ ] auditar código que você não escreveu e achar o erro que passa nos testes;
- [ ] treinar, avaliar, servir, monitorar e reverter um modelo que você entende por dentro;
- [ ] explicar qualquer ferramenta desta formação pelo problema que ela resolve — e como você a substituiria.

---

## Trilhas alternativas

### Trilha completa — ~1.300h

Todos os 32 capítulos. ~2 anos a 12h/semana.

### Trilha ML-first — ~700h

Para quem já tem base sólida de engenharia de software e quer profundidade em ML rápido.

Fase 0 → Cap. 4 (Dados) → **Fase IV inteira** → Fase V inteira → Cap. 11, 12, 14 (containers, orquestração, observabilidade) → Cap. 29.

*Custo:* fraqueza em sistemas distribuídos e arquitetura. *Ganho:* competência real em ML em ~1 ano a 14h/semana.

### Trilha Arquitetura-first — ~750h

Para quem quer ser arquiteto e ter ML como competência secundária, mas real.

Fase 0 → Fase I → Fase II → Fase III → Cap. 17, 18, 19 (matemática, avaliação, ML clássico) → Cap. 23, 26, 27 → Cap. 29, 31, 32.

*Custo:* sem deep learning nem transformers por dentro. *Ganho:* arquiteto que não é enganado por métricas de ML.

### Trilha mínima — ~480h

| Fase | Capítulos | Horas |
|---|---|---|
| 0 | Completa | 40h |
| I | Cap. 2, 4 | 85h |
| II | Cap. 5, 9 | 95h |
| III | Cap. 11, 12, 14 | 125h |
| IV | Cap. 17 (parcial), 18, 19 | 110h |
| V | Cap. 23, 26, 27 | 120h |

*(Total acima de 480h — corte os projetos "mestre" para chegar lá.)*

### Nunca corte estes quatro

Independente da trilha:

- **Cap. 5 — Sistemas Distribuídos** (alicerce conceitual das Fases III e V)
- **Cap. 17 — Matemática Aplicada a ML** (sem ele, todo o resto de ML vira decoreba)
- **Cap. 18 — Generalização e Avaliação** (sem ele você não sabe se está progredindo, nem se um paper é bom)
- **Cap. 27 — Monitoramento e Drift** (a diferença específica entre operar ML e operar software)

---

## Sistema de avaliação

### Rubrica

| Nível | Critério |
|---|---|
| **Não apto** | Sem evidência suficiente de entendimento. |
| **Apto** | O projeto fundamental funciona; há README explicando o conceito. |
| **Domínio sólido** | Projetos com testes, documento de trade-off escrito, e o sistema quebrado de propósito ao menos uma vez. |
| **Excelência** | Além do anterior: medições com variância, comparação quantitativa entre abordagens, e conexões explícitas com outros capítulos. |

### Os quatro critérios de conclusão de capítulo

Um capítulo só termina quando as quatro coisas existem:

1. **O código roda** — o projeto fundamental funciona e tem teste.
2. **O documento de trade-off está escrito** — com o lado rejeitado argumentado de forma justa.
3. **Você quebrou o sistema** — de propósito (infraestrutura) ou submeteu o modelo a dados fora de distribuição (ML) — e documentou.
4. **Você consegue explicar em 2 páginas** — para alguém que programa mas não conhece a área, sem jargão não explicado e sem consultar. Se travar ao escrever, o capítulo não acabou.

### Revisão espaçada

A cada fim de fase, volte a três artefatos anteriores: rode o código, releia o próprio README, responda uma pergunta nova, e risque itens do `learning-journal/questions.md`. Custa ~4h por fase e preserva meses de trabalho.

### Avaliação consolidada (formato para agente avaliador)

```yaml
phase: 4
status: dominio_solido

artifacts:
  ml-math-lab:                  dominio_solido
  learning-and-evaluation-lab:  excelencia
  classical-ml-lab:             dominio_solido
  deep-learning-lab:            apto
  transformers-lab:             apto
  paper-reviews:                dominio_solido

tradeoff_docs:      6/6
written_summaries:  6/6
recommendation: pode_avancar_para_fase_5
```

Não é preciso Excelência em tudo para avançar.

---

## Biblioteca

### Núcleo indispensável

**Sistemas e arquitetura**

1. Kleppmann — **Designing Data-Intensive Applications** — *o livro central das Fases I–III*
2. Arpaci-Dusseau — **Operating Systems: Three Easy Pieces** (gratuito)
3. Google — **Site Reliability Engineering** (gratuito)
4. Ousterhout — **A Philosophy of Software Design**
5. Majors et al. — **Observability Engineering**
6. Ford & Richards — **Fundamentals of Software Architecture**
7. Feathers — **Working Effectively with Legacy Code** — *o livro do Capítulo 31*

**Machine learning**

8. Deisenroth, Faisal & Ong — **Mathematics for Machine Learning** (gratuito)
9. Abu-Mostafa et al. — **Learning From Data**
10. James et al. — **An Introduction to Statistical Learning** (gratuito)
11. Goodfellow, Bengio & Courville — **Deep Learning** (gratuito)
12. Karpathy — **Neural Networks: Zero to Hero** (curso gratuito, conta como livro)
13. Huyen — **Designing Machine Learning Systems** — *o livro central da Fase V*

### Cursos e recursos gratuitos

| Recurso | Fase |
|---|---|
| OSTEP (Wisconsin) | I |
| CMU 15-445 Database Systems | I |
| Kleppmann — Distributed Systems (Cambridge) | II |
| MIT 6.824 Distributed Systems | II |
| Kubernetes the Hard Way | III |
| AWS Well-Architected / GCP Architecture Framework | III |
| 3Blue1Brown — Essence of Linear Algebra / Calculus | IV |
| Caltech — Learning From Data | IV |
| Stanford CS231n | IV |
| Karpathy — Neural Networks: Zero to Hero | IV |
| Stanford CS336 — Language Modeling from Scratch | IV |
| Google — Rules of Machine Learning | IV/V |
| Google — Deep Learning Tuning Playbook | IV |
| Google — MLOps whitepaper (níveis de maturidade) | V |

### Papers essenciais

**Sistemas**
- DeCandia et al. — *Dynamo* (2007)
- Chang et al. — *Bigtable* (2006)
- Verma et al. — *Borg* (2015)
- Kreps et al. — *Kafka* (2011)
- Sigelman et al. — *Dapper* (2010)
- Ongaro & Ousterhout — *Raft* (2014)
- Dean & Barroso — *The Tail at Scale* (2013)

**Machine learning**
- Breiman — *Statistical Modeling: The Two Cultures* (2001)
- He et al. — *Deep Residual Learning* (2015)
- Vaswani et al. — *Attention Is All You Need* (2017)
- Kaplan et al. — *Scaling Laws for Neural Language Models* (2020)
- Hoffmann et al. — *Training Compute-Optimal LLMs* (2022)
- Ouyang et al. — *InstructGPT* (2022)
- Hu et al. — *LoRA* (2021)
- Rafailov et al. — *Direct Preference Optimization* (2023)

**ML em produção**
- Sculley et al. — *Hidden Technical Debt in Machine Learning Systems* (2015) — *leitura fundadora*
- Breck et al. — *The ML Test Score* (2017)
- Polyzotis et al. — *Data Management Challenges in Production ML* (2017)
- Crankshaw et al. — *Clipper* (2017)
- Kwon et al. — *PagedAttention / vLLM* (2023)

**Método**
- Keshav — *How to Read a Paper*
- Lipton & Steinhardt — *Troubling Trends in Machine Learning Scholarship*
- Recht et al. — *Do ImageNet Classifiers Generalize to ImageNet?*

---

## Regra de ouro

> **Toda ferramenta desta área vai ser substituída. O problema que ela resolve, não.**

E o corolário para a parte de ML:

> **Quem só sabe operar modelos depende de que alguém mais saiba consertá-los. Quem entende o modelo por dentro consegue avaliar o que aparece de novo — e a maior parte do que aparece de novo é uma variação de algo que você já implementou.**

E uma última, sobre o motivo de estudar isso:

> **Esta formação otimiza para entender, não para o mercado. Utilidade profissional é subproduto — e um subproduto confiável, justamente porque não é o alvo.** Currículo que persegue previsão de mercado envelhece quando a previsão erra. Currículo que persegue fundamento continua servindo independente de qual onda vier — inclusive para argumentar contra a onda, que costuma ser a habilidade mais escassa e menos ensinada.

---

*Formação em Arquitetura de Software, Cloud e Machine Learning em Produção — julho de 2026.*
