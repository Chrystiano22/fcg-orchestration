# FCG Orchestration

Repositorio de orquestracao do Tech Challenge FIAP Cloud Games.

## Finalidade

- Centralizar a execucao local com Docker Compose.
- Versionar os manifests Kubernetes.
- Configurar API Gateway, observabilidade, NoSQL e cache da Fase 3.
- Servir como guia central de execucao e entrega.

## Stack da Fase 3

| Requisito | Solucao adotada |
| --- | --- |
| API Gateway | Kong Gateway em modo declarativo |
| Seguranca no Gateway | Validacao JWT via plugin `jwt` |
| Observabilidade | Prometheus e Grafana |
| Metricas | `/metrics` em UsersAPI e CatalogAPI |
| NoSQL | MongoDB para avaliacoes de jogos |
| Cache distribuido | Redis para cache de consulta do catalogo |
| Mensageria | RabbitMQ |
| Serverless | Notifications em funcao serverless acionada por mensagens RabbitMQ |

## Repositorios relacionados

- `fcg-users-api`
- `fcg-catalog-api`
- `fcg-payments-api`
- `fcg-notifications-api`

## Estrutura

```text
/compose
  docker-compose.yml
  kong.yml
  prometheus.yml
  grafana/
/k8s
  *.yaml
/docs
  arquitetura.md
  eventos.md
  fase3-status.md
README.md
RELATORIO_ENTREGA_FASE2.txt
RELATORIO_ENTREGA_FASE3.txt
```

## Portas locais

| Servico | URL |
| --- | --- |
| Gateway Kong | `http://localhost:8000` |
| Kong Admin API | `http://localhost:8001` |
| UsersAPI | `http://localhost:5101` |
| CatalogAPI | `http://localhost:5102` |
| PaymentsAPI | `http://localhost:5103` |
| NotificationsAPI legado | `http://localhost:5104` com profile `legacy-notifications-api` |
| RabbitMQ | `amqp://localhost:5672` |
| RabbitMQ Management | `http://localhost:15672` |
| MongoDB | `mongodb://localhost:27017` |
| Redis | `localhost:6379` |
| Prometheus | `http://localhost:9090` |
| Grafana | `http://localhost:3000` |

Credenciais locais:

- RabbitMQ: `guest` / `guest`
- Grafana: `admin` / `admin`
- Usuario admin da aplicacao: `admin@fcg.local` / `Admin@123`

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
```

Validar metricas:

```powershell
Invoke-WebRequest http://localhost:5101/metrics
Invoke-WebRequest http://localhost:5102/metrics
```

Validar Gateway:

```powershell
Invoke-WebRequest http://localhost:8000/auth/login -Method Post -ContentType "application/json" -Body '{"email":"admin@fcg.local","senha":"Admin@123"}'
```

Depois do login, usar o token JWT retornado para acessar rotas protegidas pelo Gateway:

```powershell
$headers = @{ Authorization = "Bearer SEU_TOKEN_AQUI" }
Invoke-WebRequest http://localhost:8000/jogos -Headers $headers
```

Validar filas RabbitMQ:

```powershell
docker compose -f compose\docker-compose.yml exec rabbitmq rabbitmqctl list_queues name messages consumers
```

Parar a stack:

```powershell
docker compose -f compose\docker-compose.yml down
```

## Kubernetes

Aplicar em um cluster local:

```powershell
kubectl apply -k k8s
```

Para criar um cluster local com Kind:

```powershell
kind create cluster --config k8s\kind-config.yaml --name fcg-local
kind load docker-image compose-users-api:latest --name fcg-local
kind load docker-image compose-catalog-api:latest --name fcg-local
kind load docker-image compose-payments-api:latest --name fcg-local
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
| Gateway Kong | `http://localhost:30080` |
| Kong Admin API | `http://localhost:30081` |
| UsersAPI | `http://localhost:30101` |
| CatalogAPI | `http://localhost:30102` |
| PaymentsAPI | `http://localhost:30103` |
| RabbitMQ AMQP | `amqp://localhost:30672` |
| RabbitMQ Management | `http://localhost:31672` |
| Prometheus | `http://localhost:30090` |
| Grafana | `http://localhost:30300` |

Remover os recursos:

```powershell
kubectl delete -k k8s
```

## Funcionalidades da Fase 3

- Kong recebe as chamadas externas em `localhost:8000`.
- Kong valida JWT nas rotas protegidas.
- UsersAPI e CatalogAPI expoem metricas Prometheus em `/metrics`.
- Prometheus coleta metricas de UsersAPI e CatalogAPI.
- Grafana sobe com datasource Prometheus e dashboard inicial.
- CatalogAPI usa Redis para cache da listagem de jogos.
- CatalogAPI usa MongoDB para armazenar avaliacoes flexiveis de jogos.
- Notifications usa funcao serverless no repositorio `fcg-notifications-api`.

## Status da Fase 3

| Etapa | Status | Faltante |
| --- | --- | --- |
| Leitura dos requisitos | Concluido | 0% |
| Branch de trabalho | Concluido | 0% |
| Orquestracao Fase 3 | Concluido | 0% |
| Instrumentacao Users/Catalog | Concluido | 0% |
| Notifications serverless | Concluido | 0% |
| Validacao e entrega | Pendente | 100% |

## Documentacao de entrega

- Arquitetura: `docs/arquitetura.md`
- Eventos: `docs/eventos.md`
- Status da Fase 3: `docs/fase3-status.md`
- Notifications serverless: `docs/serverless-notifications.md`
- Relatorio Fase 3: `RELATORIO_ENTREGA_FASE3.txt`
