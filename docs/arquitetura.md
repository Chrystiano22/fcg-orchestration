# Arquitetura - Fase 2

## Visao geral

A Fase 2 refatora o MVP monolitico da Fase 1 para uma arquitetura de microsservicos orientada a eventos.

Servicos:

- `UsersAPI`: cadastro, autenticacao, JWT e usuarios.
- `CatalogAPI`: catalogo, promocoes, biblioteca e inicio de compra.
- `PaymentsAPI`: processamento simulado de pagamento.
- `NotificationsAPI`: simulacao de notificacoes por log.
- `RabbitMQ`: broker de mensageria assincrona.
- `fcg-orchestration`: Docker Compose, Kubernetes e documentacao de execucao.

## Comunicacao

- Chamadas HTTP sao usadas apenas na entrada dos fluxos por cliente/API.
- Comunicacao entre servicos usa eventos RabbitMQ via MassTransit.
- Nao ha chamada HTTP direta entre `CatalogAPI`, `PaymentsAPI` e `NotificationsAPI` no fluxo principal de compra.

## Persistencia

- `UsersAPI`: SQLite proprio em `/app/data/users.db`.
- `CatalogAPI`: SQLite proprio em `/app/data/catalog.db`.
- `PaymentsAPI`: sem banco no MVP, processamento demonstrado por evento e log.
- `NotificationsAPI`: sem banco no MVP, notificacoes demonstradas por log.

## Fluxo de cadastro

1. Cliente chama `POST /usuarios` no `UsersAPI`.
2. `UsersAPI` cria o usuario.
3. `UsersAPI` publica `UserCreatedEvent`.
4. `NotificationsAPI` consome `UserCreatedEvent`.
5. `NotificationsAPI` registra log de boas-vindas.

## Fluxo de compra

1. Cliente chama `POST /compras` no `CatalogAPI`.
2. `CatalogAPI` publica `OrderPlacedEvent`.
3. `PaymentsAPI` consome `OrderPlacedEvent`.
4. `PaymentsAPI` simula pagamento.
5. `PaymentsAPI` publica `PaymentProcessedEvent`.
6. `CatalogAPI` consome `PaymentProcessedEvent`.
7. Se o pagamento for `Approved`, `CatalogAPI` adiciona o jogo a biblioteca.
8. `NotificationsAPI` consome `PaymentProcessedEvent` e registra log de confirmacao.

## Execucao local

Docker Compose:

```powershell
docker compose -f compose\docker-compose.yml up --build -d
```

Kubernetes local com Kind:

```powershell
kind create cluster --config k8s\kind-config.yaml --name fcg-local
kind load docker-image compose-users-api:latest --name fcg-local
kind load docker-image compose-catalog-api:latest --name fcg-local
kind load docker-image compose-payments-api:latest --name fcg-local
kind load docker-image compose-notifications-api:latest --name fcg-local
kubectl apply -k k8s
```
