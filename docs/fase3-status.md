# Status da Fase 3

## Etapa 1 - Requisitos

Status: concluido.

Faltante: 0%.

Itens cobertos:

- PDF da Fase 3 localizado e analisado.
- Requisitos obrigatorios identificados.
- Stack definida: Kong, Prometheus, Grafana, MongoDB e Redis.

## Etapa 2 - Branches

Status: concluido.

Faltante: 0%.

Itens cobertos:

- Branch `codex/fase3-progress` criada nos repositorios envolvidos.

## Etapa 3 - Orquestracao

Status: concluido.

Faltante: 0%.

Itens cobertos:

- Docker Compose atualizado com Kong, Prometheus, Grafana, MongoDB e Redis.
- Configuracao declarativa do Kong versionada.
- Configuracao do Prometheus versionada.
- Provisionamento inicial do Grafana versionado.
- Manifests Kubernetes adicionados para Kong, Prometheus, Grafana, MongoDB e Redis.

Validacoes executadas:

- Validacao sintatica do Docker Compose executada.
- Validacao sintatica dos manifests Kubernetes executada com kustomize.

## Etapa 4 - Instrumentacao UsersAPI e CatalogAPI

Status: concluido.

Faltante: 0%.

Itens cobertos:

- UsersAPI expoe endpoint `/metrics`.
- CatalogAPI expoe endpoint `/metrics`.
- CatalogAPI usa Redis para cache da listagem de jogos.
- CatalogAPI usa MongoDB para persistir avaliacoes de jogos.

Validacoes executadas:

- Testes automatizados da UsersAPI executados com sucesso.
- Testes automatizados da CatalogAPI executados com sucesso.

## Etapa 5 - Notifications serverless

Status: concluido.

Faltante: 0%.

Itens cobertos:

- Criar ou separar a funcao serverless de notificacoes.
- Documentar trigger por mensageria.
- Versionar infraestrutura como codigo da funcao.
- Validar build da funcao.
- Validar simulacao local de `UserCreatedEvent`.
- Validar simulacao local de `PaymentProcessedEvent`.

## Etapa 6 - Validacao e entrega

Status: em andamento.

Faltante: 20%.

Itens cobertos:

- Testes automatizados executados nas APIs alteradas.
- Build da funcao serverless executado com sucesso.
- Simulacao local da funcao serverless validada.
- Ambiente completo subido com Docker Compose.
- Health checks de UsersAPI, CatalogAPI, PaymentsAPI, Kong, Prometheus e Grafana validados.
- Login via Gateway Kong validado com JWT.
- Rota protegida de catalogo validada via Gateway Kong.
- Criacao de jogo validada via Gateway Kong.
- Avaliacao de jogo persistida no MongoDB.
- Cache Redis confirmado na listagem de jogos.
- Targets do Prometheus confirmados como ativos.
- Fluxo de compra validado com RabbitMQ e processamento assicrono.
- Relatorio final atualizado com as validacoes tecnicas.

Itens pendentes:

- Gravar video.
- Inserir link do video no relatorio.
