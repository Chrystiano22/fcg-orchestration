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

Itens pendentes:

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

Itens pendentes:

- Testes automatizados da UsersAPI executados com sucesso.
- Testes automatizados da CatalogAPI executados com sucesso.

## Etapa 5 - Notifications serverless

Status: pendente.

Faltante: 100%.

Itens pendentes:

- Criar ou separar a funcao serverless de notificacoes.
- Documentar trigger por mensageria.
- Versionar infraestrutura como codigo da funcao.

## Etapa 6 - Validacao e entrega

Status: pendente.

Faltante: 100%.

Itens pendentes:

- Rodar testes.
- Subir ambiente completo.
- Atualizar relatorio final.
- Gravar video.
- Inserir link do video no relatorio.
