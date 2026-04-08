# PLAN.md — Лекція 7: Хмарні бази даних

**Курс:** Хмарні технології інформаційних систем  
**Аудиторія:** Бакалавр ІТ · 3 курс  
**Тривалість:** 2 академічні години (90 хв)

---

## Мета лекції

Студенти повинні вміти:
- Порівнювати RDBMS та NoSQL і обирати відповідний тип для задачі
- Пояснювати гарантії ACID та модель BASE
- Ідентифікувати database bottleneck-и та способи їх усунення
- Описувати горизонтальне та вертикальне масштабування БД
- Розпізнавати типові anti-patterns роботи з БД
- Орієнтуватися в AWS-сервісах: RDS, Aurora, DynamoDB, ElastiCache

---

## Структура лекції (45 слайдів)

### Блок 1 — Вступ (слайди 1–3) · ~5 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 1 | Титульний | Назва, курс, аудиторія |
| 2 | Agenda | Огляд тем лекції |
| 3 | Де живуть дані? | Короткий огляд типів сховищ: файли, RDBMS, NoSQL, кеш |

### Блок 2 — RDBMS vs NoSQL (слайди 4–11) · ~20 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 4 | RDBMS — огляд | Реляційна модель, схема, SQL, приклади (PostgreSQL, MySQL, Oracle) |
| 5 | NoSQL — огляд | Нереляційні моделі, гнучка схема, горизонтальне масштабування |
| 6 | RDBMS vs NoSQL | Порівняльна таблиця: схема, мова, масштабування, ACID, use-cases |
| 7 | NoSQL: Key-Value | Структура, приклади (Redis, DynamoDB), use-cases |
| 8 | NoSQL: Document | Структура (JSON/BSON), приклади (MongoDB, DocumentDB), use-cases |
| 9 | NoSQL: Wide-Column | Структура, приклади (Cassandra, Keyspaces), use-cases |
| 10 | NoSQL: Graph | Структура (вузли/ребра), приклади (Neo4j, Neptune), use-cases |
| 11 | Вибір БД | Дерево рішень: коли RDBMS, коли NoSQL, коли змішано |

### Блок 3 — ACID vs BASE (слайди 12–23) · ~25 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 12 | ACID — огляд | Поняття транзакції, 4 властивості |
| 13 | Atomicity | Все або нічого, rollback на прикладі |
| 14 | Consistency | Дані завжди задовольняють правила БД |
| 15 | Isolation | Паралельні транзакції не впливають одна на одну |
| 16 | Durability | Після COMMIT дані збережені назавжди |
| 17 | ACID: банківська транзакція | Живий приклад: переказ грошей |
| 18 | Рівні ізоляції | Read Uncommitted → Serializable, аномалії |
| 19 | BASE — огляд | Basically Available, Soft state, Eventually consistent |
| 20 | Eventually Consistent | Приклад: оновлення балансу в розподіленій системі |
| 21 | ACID vs BASE | Порівняльна таблиця, сценарії вибору |
| 22 | CAP теорема | Consistency / Availability / Partition tolerance — не все одночасно |
| 23 | CAP — приклади | Класифікація реальних БД (PostgreSQL=CP, Cassandra=AP тощо) |

### Блок 4 — DB як Bottleneck та Масштабування (слайди 24–34) · ~25 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 24 | DB як Bottleneck | Чому БД є найчастішим вузьким місцем у системі |
| 25 | Read-heavy | Симптоми, рішення: Read Replicas, кешування |
| 26 | Write-heavy | Симптоми, рішення: Sharding, write-optimized NoSQL |
| 27 | Connection bottleneck | Надмірна кількість з'єднань, connection pool exhaustion |
| 28 | Slow queries | EXPLAIN, індекси, N+1, full table scan |
| 29 | Стратегії масштабування | Огляд: Vertical vs Horizontal vs Functional decomposition |
| 30 | Vertical Scaling | Scale Up: більше CPU/RAM, межі, вартість, downtime |
| 31 | Read Replicas | Реплікація читання, lag, use-cases |
| 32 | Sharding | Горизонтальне партиціювання, shard key вибір, cross-shard queries |
| 33 | Caching | Cache-aside, write-through, TTL, Redis/Memcached |
| 34 | Connection Pooling | PgBouncer, RDS Proxy, pool sizing |

### Блок 5 — Anti-patterns (слайди 35–40) · ~10 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 35 | Anti-patterns — огляд | Поширені помилки проектування та використання БД |
| 36 | N+1 Query | Опис, приклад коду, вирішення (JOIN / eager loading) |
| 37 | Відсутність індексів | Full table scan, вартість, як виявити |
| 38 | God Table | Одна таблиця для всього, проблеми, нормалізація |
| 39 | One DB for Everything | OLTP + OLAP + cache в одному місці, рішення |
| 40 | No Connection Pooling | Відкриття нового з'єднання на кожен запит, наслідки |

### Блок 6 — AWS Database Services (слайди 41–44) · ~10 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 41 | Amazon RDS | Managed RDBMS: движки, Multi-AZ, Read Replicas, автобекапи |
| 42 | Amazon Aurora | MySQL/PG-сумісна, 6 копій, serverless, ~5x MySQL |
| 43 | Amazon DynamoDB | Serverless NoSQL, DAX, Global Tables, on-demand capacity |
| 44 | Amazon ElastiCache | Managed Redis/Memcached, cache patterns, TTL |

### Блок 7 — Підсумок (слайд 45) · ~5 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 45 | Ключові висновки | Чеклист знань, запитання для самоперевірки |

---

## Ключові поняття

| Термін | Визначення |
|--------|-----------|
| RDBMS | Relational Database Management System — реляційна СУБД |
| NoSQL | Not Only SQL — нереляційні БД |
| ACID | Atomicity, Consistency, Isolation, Durability |
| BASE | Basically Available, Soft state, Eventually consistent |
| CAP | Consistency, Availability, Partition tolerance |
| Sharding | Горизонтальне розбиття даних між вузлами |
| Read Replica | Копія БД тільки для читання |
| Connection Pool | Пул відкритих з'єднань для повторного використання |
| N+1 Problem | Виконання N+1 запитів замість одного |
| DAX | DynamoDB Accelerator — in-memory кеш для DynamoDB |

---

## Рекомендована підготовка

- Пригадати основи SQL: SELECT, JOIN, транзакції
- Переглянути: [AWS Database Services](https://aws.amazon.com/products/databases/)
- Прочитати: [CAP theorem explained](https://www.ibm.com/cloud/learn/cap-theorem)

---

## Посилання

- AWS RDS: <https://docs.aws.amazon.com/rds/>
- AWS Aurora: <https://docs.aws.amazon.com/aurora/>
- DynamoDB: <https://docs.aws.amazon.com/dynamodb/>
- ElastiCache: <https://docs.aws.amazon.com/elasticache/>
- Martin Fowler — NoSQL Distilled (книга)
- "Designing Data-Intensive Applications" — Martin Kleppmann
