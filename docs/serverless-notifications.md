# Notifications serverless

Na Fase 3, o fluxo principal de notificacoes deixa de depender de um container HTTP rodando continuamente.

## Repositorio

```text
fcg-notifications-api
```

## Projeto da funcao

```text
src/Fcg.Notifications.Function
```

## Handler

```text
Fcg.Notifications.Function::Fcg.Notifications.Function.NotificationFunction::HandleAsync
```

## Infraestrutura como codigo

```text
serverless/template.yaml
```

## Eventos processados

- `UserCreatedEvent`
- `PaymentProcessedEvent`

## Filas esperadas

- `notifications-user-created`
- `notifications-payment-processed`

## Como demonstrar localmente

No repositorio `fcg-notifications-api`:

```powershell
dotnet run --project src\Fcg.Notifications.Function -- --event samples\user-created-function-event.json
dotnet run --project src\Fcg.Notifications.Function -- --event samples\payment-processed-function-event.json
```

O retorno mostra o evento processado e se a notificacao simulada foi enviada. Os logs mostram a notificacao de boas-vindas ou confirmacao de compra.

## Como demonstrar na nuvem

O template SAM espera:

- ARN do broker Amazon MQ for RabbitMQ
- ARN do secret com credenciais RabbitMQ
- virtual host do RabbitMQ
- e-mail remetente usado nos logs

Com isso, o provedor cloud entrega as mensagens para a funcao, evitando manter um container da NotificationsAPI ativo 24/7.

## Modo legado

O Docker Compose ainda possui o servico `notifications-api`, mas ele fica atras do profile `legacy-notifications-api` e nao sobe por padrao.

Para subir o modo legado:

```powershell
docker compose -f compose\docker-compose.yml --profile legacy-notifications-api up --build -d
```
