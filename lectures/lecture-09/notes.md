# Лекція 9: Cloud-native та контейнеризація — Конспект

## 1. Cloud-native принципи

**Cloud-native** — підхід до побудови та запуску застосунків, що повністю використовує переваги хмарної моделі.

**CNCF визначення**: контейнеризовані, dynamically orchestrated, мікросервісна архітектура.

**Ключові принципи**:
- **12-Factor App**: методологія для cloud-native застосунків
- **Microservices**: маленькі, незалежні сервіси
- **Containers**: стандартна одиниця деплою
- **Dynamic Orchestration**: Kubernetes, ECS
- **DevOps automation**: CI/CD pipelines

---

## 2. Microservices

**Монолітна архітектура**: весь код в одному застосунку. Простіше розробляти спочатку, важко масштабувати та розгортати.

**Мікросервіси**: кожен сервіс:
- Виконує одну задачу добре (SRP)
- Незалежно деплоїться
- Має власну БД (database per service)
- Комунікує через API (REST, gRPC) або події (SQS, SNS)

**Переваги**: незалежне масштабування, технологічна різноманітність, менші команди.
**Виклики**: distributed tracing, service discovery, eventual consistency.

---

## 3. Docker та Containers

**Docker**: платформа для контейнеризації застосунків.

```dockerfile
FROM node:20-alpine       # base image
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
USER node
CMD ["node", "server.js"]
```

**Шари (layers)**: кожна інструкція = layer. Кешуються. Змінюй часто — ставь внизу.

**Container Registry**: Amazon ECR (Elastic Container Registry) — зберігання Docker images.

---

## 4. Kubernetes: концепція

**Kubernetes (K8s)**: оркестратор контейнерів. Автоматизує deployment, scaling, self-healing.

**Ключові об'єкти**:
- **Pod**: 1+ контейнерів, мінімальна одиниця деплою
- **Deployment**: керує репліками Pods, rolling updates
- **Service**: стабільна мережева адреса для Pods
- **Ingress**: HTTP routing до Services

**AWS EKS**: managed Kubernetes. Control plane = AWS відповідальність. Worker nodes = ваша відповідальність (або Fargate).

---

## 5. AWS ECS

**ECS (Elastic Container Service)**: власний оркестратор AWS. Простіше, ніж Kubernetes.

**Launch types**:
- **EC2**: контейнери на EC2 instances (ви управляєте servers)
- **Fargate**: serverless containers (AWS управляє servers)

**ECS Fargate**: ви описуєте CPU/RAM → AWS запускає. Немає EC2 management.

---

## 6. AWS Lambda (Serverless)

- Функція як сервіс (FaaS)
- Оплата: кількість викликів + duration (GB-секунди)
- Trigger: API Gateway, S3 event, SQS, DynamoDB Streams, EventBridge
- Cold start: 100–500ms (provisioned concurrency усуває)
- Timeout: max 15 хвилин
- Memory: 128MB – 10GB

---

## 7. Event-driven Architecture

```
Producer → Queue/Topic → Consumer

S3 upload → S3 Event → SQS Queue → Lambda → DB
EC2 state → EventBridge → SNS → Email/SMS
API call → API Gateway → Lambda → Response
```

**Переваги**: loose coupling, async processing, retry logic, dead letter queues.
