# Лекція 9: Cloud-native та контейнеризація — Слайди

---

## Slide 1: Назва

# Лекція 9: Cloud-native та контейнеризація

---

## Slide 2: Cloud-native принципи

```
✅ Microservices
✅ Containers (стандартна одиниця деплою)
✅ Dynamic Orchestration (K8s, ECS)
✅ DevOps + CI/CD
✅ 12-Factor App
✅ Observability (logs, metrics, traces)
```

---

## Slide 3: Монолітна vs Мікросервісна

| | Моноліт | Мікросервіси |
|--|---------|--------------|
| Деплой | Всі разом | Незалежно |
| Масштаб | Вертикальний | Горизонтальний (per service) |
| БД | Спільна | Database per service |
| Команда | Одна велика | Маленькі (pizza team) |
| Складність | ОС | Мережева |

---

## Slide 4: Docker Dockerfile

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
USER node
CMD ["node", "server.js"]
```

Image → Container → ECR → ECS/EKS

---

## Slide 5: Kubernetes об'єкти

| Об'єкт | Що це |
|--------|-------|
| Pod | 1+ контейнерів |
| Deployment | Реплікація + rolling update |
| Service | Стабільна мережева адреса |
| Ingress | HTTP routing |
| ConfigMap | Конфігурація (не секрети) |
| Secret | Чутливі дані |

---

## Slide 6: EKS vs ECS

| | ECS | EKS |
|--|-----|-----|
| Оркестратор | AWS власний | Kubernetes |
| Складність | Нижча | Вища |
| Vendor lock | Так | Ні (portable) |
| Ecosystem | AWS tools | CNCF ecosystem |
| Fargate | ✅ | ✅ |

---

## Slide 7: AWS Lambda

```
Trigger      →   Lambda    →  Result
S3 upload        (функція)    DB write
API Gateway      max 15min    S3 write
SQS message      128MB-10GB   API call
EventBridge      pay/ms
```

Cold start: 100–500ms. Provisioned Concurrency = завжди warm.

---

## Slide 8: Event-driven Architecture

```
S3 upload
    ↓
S3 Event Notification
    ↓
SQS Queue (buffer + retry)
    ↓
Lambda (consumer)
    ↓
RDS / DynamoDB
```

Loose coupling, async, DLQ для failed messages

---

## Slide 9: 12-Factor App (ключові)

1. Codebase: один repo
2. Dependencies: explicit (requirements.txt, package.json)
3. Config: через environment variables
4. Stateless processes: не зберігай стан на диску
5. Logs: stdout/stderr (не файли)
6. Dev/Prod parity: мінімальна різниця між середовищами

---

## Slide 10: Ключові висновки

1. Cloud-native = containers + orchestration + DevOps
2. Мікросервіси: незалежний деплой, власна БД
3. Docker: стандарт пакування застосунків
4. ECS (простіше) vs EKS (portable Kubernetes)
5. Lambda: коли функція, а не сервер
