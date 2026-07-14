# Eventos - Fase 2

## Broker

- Broker: RabbitMQ.
- Biblioteca .NET: MassTransit.
- Serializacao: raw JSON.
- Retry basico: 3 tentativas com intervalo de 5 segundos.

## Exchanges

| Exchange | Publicador | Consumidores |
| --- | --- | --- |
| `fcg.user-created` | `UsersAPI` | `NotificationsAPI` |
| `fcg.order-placed` | `CatalogAPI` | `PaymentsAPI` |
| `fcg.payment-processed` | `PaymentsAPI` | `CatalogAPI`, `NotificationsAPI` |

## Filas

| Fila | Servico consumidor | Evento |
| --- | --- | --- |
| `notifications-user-created` | `NotificationsAPI` | `UserCreatedEvent` |
| `payments-order-placed` | `PaymentsAPI` | `OrderPlacedEvent` |
| `catalog-payment-processed` | `CatalogAPI` | `PaymentProcessedEvent` |
| `notifications-payment-processed` | `NotificationsAPI` | `PaymentProcessedEvent` |

## UserCreatedEvent

Publicado quando um usuario e cadastrado.

Campos minimos:

- `UserId`
- `Name`
- `Email`
- `CreatedAt`

Consumidor:

- `NotificationsAPI`: registra log de e-mail de boas-vindas.

## OrderPlacedEvent

Publicado quando uma compra e iniciada no catalogo.

Campos minimos:

- `OrderId`
- `UserId`
- `GameId`
- `Price`
- `PlacedAt`

Consumidor:

- `PaymentsAPI`: simula o pagamento.

## PaymentProcessedEvent

Publicado quando o pagamento simulado e processado.

Campos minimos:

- `OrderId`
- `UserId`
- `GameId`
- `Price`
- `Status`
- `ProcessedAt`

Consumidores:

- `CatalogAPI`: adiciona o jogo a biblioteca se `Status` for `Approved`.
- `NotificationsAPI`: registra log de confirmacao se `Status` for `Approved`.

## Validacao

Com Docker Compose:

```powershell
docker compose -f compose\docker-compose.yml exec rabbitmq rabbitmqctl list_queues name messages consumers
```

Com Kubernetes:

```powershell
kubectl exec -n fcg deploy/rabbitmq -- rabbitmqctl list_queues name messages consumers
```

Resultado esperado:

- 4 filas.
- `0` mensagens pendentes.
- `1` consumidor por fila.
