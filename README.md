# FCG Orchestration

Repositorio de orquestracao da Fase 2 do Tech Challenge FIAP Cloud Games.

## Finalidade

- Centralizar a execucao local com Docker Compose.
- Centralizar ou referenciar os manifests Kubernetes.
- Documentar o fluxo completo entre os microsservicos.
- Apoiar a demonstracao em video e o relatorio final.

## Repositorios relacionados

- `fcg-users-api`
- `fcg-catalog-api`
- `fcg-payments-api`
- `fcg-notifications-api`

## Estrutura

```text
/compose
  docker-compose.yml
/k8s
  *.yaml
/docs
  arquitetura.md
  eventos.md
README.md
RELATORIO_ENTREGA_FASE2.txt
```

## Portas locais

| Servico | URL |
| --- | --- |
| UsersAPI | `http://localhost:5101` |
| CatalogAPI | `http://localhost:5102` |
| PaymentsAPI | `http://localhost:5103` |
| NotificationsAPI | `http://localhost:5104` |
| RabbitMQ | `amqp://localhost:5672` |
| RabbitMQ Management | `http://localhost:15672` |

## Executar com Docker Compose

Na raiz deste repositorio:

```powershell
docker compose -f compose\docker-compose.yml up --build -d
```

Validar containers:

```powershell
docker compose -f compose\docker-compose.yml ps
```

Validar health checks:

```powershell
Invoke-WebRequest http://localhost:5101/health
Invoke-WebRequest http://localhost:5102/health
Invoke-WebRequest http://localhost:5103/health
Invoke-WebRequest http://localhost:5104/health
```

Validar filas RabbitMQ:

```powershell
docker compose -f compose\docker-compose.yml exec rabbitmq rabbitmqctl list_queues name messages consumers
```

Parar a stack:

```powershell
docker compose -f compose\docker-compose.yml down
```

## Status validado

- Docker Compose criado e validado.
- RabbitMQ sobe com Management UI.
- Os quatro microsservicos sobem em containers.
- Health checks das quatro APIs retornam HTTP `200`.
- Fluxo completo de cadastro, compra, pagamento e biblioteca validado via eventos RabbitMQ.
- Filas validadas com `0` mensagens pendentes e `1` consumidor cada.

## Kubernetes

Os manifests ficam em `k8s` e usam as imagens locais criadas no passo do Docker Compose:

- `compose-users-api:latest`
- `compose-catalog-api:latest`
- `compose-payments-api:latest`
- `compose-notifications-api:latest`
- `rabbitmq:3.13-management-alpine`

Aplicar em um cluster local:

```powershell
kubectl apply -k k8s
```

Para criar um cluster local com Kind usando os NodePorts documentados:

```powershell
kind create cluster --config k8s\kind-config.yaml --name fcg-local
kind load docker-image compose-users-api:latest --name fcg-local
kind load docker-image compose-catalog-api:latest --name fcg-local
kind load docker-image compose-payments-api:latest --name fcg-local
kind load docker-image compose-notifications-api:latest --name fcg-local
kubectl apply -k k8s
```

Validar recursos:

```powershell
kubectl get pods -n fcg
kubectl get services -n fcg
kubectl get pvc -n fcg
```

URLs via NodePort em ambiente local compativel:

| Servico | URL |
| --- | --- |
| UsersAPI | `http://localhost:30101` |
| CatalogAPI | `http://localhost:30102` |
| PaymentsAPI | `http://localhost:30103` |
| NotificationsAPI | `http://localhost:30104` |
| RabbitMQ AMQP | `amqp://localhost:30672` |
| RabbitMQ Management | `http://localhost:31672` |

Validar filas RabbitMQ no pod:

```powershell
kubectl exec -n fcg deploy/rabbitmq -- rabbitmqctl list_queues name messages consumers
```

Remover os recursos:

```powershell
kubectl delete -k k8s
```

## Documentacao de entrega

- Arquitetura: `docs/arquitetura.md`
- Eventos: `docs/eventos.md`
- Relatorio final: `RELATORIO_ENTREGA_FASE2.txt`

## Proximas etapas

1. Preencher nome do grupo e participantes no relatorio.
2. Gravar o video.
3. Inserir o link do video no relatorio.
