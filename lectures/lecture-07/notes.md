# Лекція 7: Хмарні бази даних — Конспект

## 1. RDBMS vs NoSQL

**RDBMS (Relational)**: таблиці, SQL, схема, ACID. PostgreSQL, MySQL, Oracle.
**NoSQL**: схема гнучка, горизонтальне масштабування, різні моделі даних.

| Тип NoSQL | Модель | Приклад | AWS |
|-----------|--------|---------|-----|
| Key-Value | key → value | Redis | ElastiCache, DynamoDB |
| Document | JSON documents | MongoDB | DocumentDB |
| Wide Column | column families | Cassandra | Keyspaces |
| Graph | nodes, edges | Neo4j | Neptune |
| Time Series | time-stamped | InfluxDB | Timestream |

---

## 2. ACID vs BASE

**ACID** (Relational):
- **A**tomicity: транзакція — все або нічого
- **C**onsistency: дані завжди в дійсному стані
- **I**solation: паралельні транзакції не впливають одна на одну
- **D**urability: після COMMIT дані збережені

**BASE** (NoSQL):
- **B**asically **A**vailable: відповідь завжди, навіть якщо дані неточні
- **S**oft state: стан може змінюватися без зовнішнього input
- **E**ventually consistent: з часом всі вузли будуть консистентними

---

## 3. Database як Bottleneck

**Read-heavy**: Read Replicas (RDS), Read Replicas DynamoDB (DAX)
**Write-heavy**: Sharding, DynamoDB on-demand
**Connection bottleneck**: RDS Proxy (connection pooling)
**Query bottleneck**: ElastiCache (Redis/Memcached) перед БД

---

## 4. Scaling Databases

**Vertical Scaling (Scale Up)**: більший інстанс (r6g.4xlarge). Обмежено розміром найбільшого сервера.

**Horizontal Scaling (Scale Out)**:
- Read Replicas: копії для читання
- Sharding: партиціонування даних між серверами
- CQRS: розділення read та write моделей

---

## 5. Amazon RDS

Managed relational database. Підтримує: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora.

**Переваги**: automated backups, patch management, Multi-AZ failover, read replicas.

**Multi-AZ**: standby в іншій AZ, автоматичний failover (~60–120 сек). Синхронна реплікація.

**Read Replicas**: асинхронна реплікація. Для read-heavy workloads. Можна промотувати до primary.

---

## 6. Amazon Aurora

MySQL/PostgreSQL сумісна, але кастомна архітектура AWS:
- 6 копій даних у 3 AZ
- Storage автоматично збільшується до 128 TB
- До 15 Read Replicas
- Failover за 30 секунд (Aurora Serverless v2 — миттєво)
- ~5x швидша за MySQL, ~3x за PostgreSQL (за заявою AWS)

---

## 7. Amazon DynamoDB

NoSQL key-value та document база даних, повністю managed.
- Sub-millisecond latency
- On-demand або provisioned capacity
- DynamoDB Streams → Lambda (event-driven)
- Global Tables: multi-region active-active
- DAX: in-memory cache для DynamoDB

---

## 8. Anti-Patterns

- Використовувати RDS там, де потрібен DynamoDB (і навпаки)
- SELECT * без WHERE на великих таблицях
- Немає індексів на frequently queried columns
- Зберігати binary/blob дані в БД (є S3)
- Підключатися до БД напряму з Lambda (connection exhaustion → RDS Proxy)
