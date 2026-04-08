# Лекція 7: Хмарні бази даних — Слайди

---

## Slide 1: Назва (Title)

# Лекція 7: Хмарні бази даних  
**Курс:** Хмарні технології інформаційних систем  
Бакалавр ІТ · 3 курс · 2 години

---

## Slide 2: Agenda

1. Де живуть дані?
2. RDBMS vs NoSQL
3. Типи NoSQL
4. ACID — детально
5. BASE та Eventually Consistent
6. CAP теорема
7. DB як Bottleneck
8. Стратегії масштабування
9. Anti-patterns
10. AWS Database Services

---

## Slide 3: Де живуть дані?

| Тип | Що зберігає | Приклад |
|-----|-------------|---------|
| File System | Файли | S3, EFS |
| RDBMS | Таблиці (рядки) | PostgreSQL, MySQL |
| NoSQL | Документи, KV, graphs | DynamoDB, Redis |
| Cache | Тимчасові дані | Redis, Memcached |
| Data Warehouse | Аналітика | Redshift, BigQuery |

---

## Slide 4: RDBMS — огляд

- **Реляційна модель**: дані в таблицях з фіксованою схемою
- **SQL** — стандартна мова запитів
- **Зв'язки**: JOIN між таблицями
- **Транзакції з ACID-гарантіями**
- Приклади: PostgreSQL, MySQL, Oracle, SQL Server, Amazon RDS

---

## Slide 5: NoSQL — огляд

- "Not Only SQL" — нереляційні бази
- **Гнучка або відсутня схема** (schema-less)
- Розроблені для **горизонтального масштабування**
- Зазвичай жертвують частиною ACID на користь масштабованості
- Приклади: MongoDB, Cassandra, Redis, DynamoDB

---

## Slide 6: RDBMS vs NoSQL

| Характеристика | RDBMS | NoSQL |
|----------------|-------|-------|
| Схема | Фіксована | Гнучка / відсутня |
| Мова запитів | SQL | API / DSL |
| Масштабування | Вертикальне | Горизонтальне |
| Гарантії | ACID | BASE (зазвичай) |
| Зв'язки | JOIN | Денормалізація |
| Use-cases | Транзакції, фінанси | Scale, гнучкість |

---

## Slide 7: NoSQL — Key-Value

- **Структура**: ключ → значення (як хеш-таблиця)
- **Сильна сторона**: екстремально швидкі reads/writes (O(1))
- **Слабка сторона**: немає складних запитів
- **Приклади**: Redis, Amazon ElastiCache, DynamoDB (частково)
- **Use-cases**: сесії, кеш, ліки, черги

---

## Slide 8: NoSQL — Document

- **Структура**: документ (JSON/BSON) з вкладеними полями
- **Сильна сторона**: гнучка схема, вкладені дані разом
- **Слабка сторона**: складні JOIN-и не підтримуються
- **Приклади**: MongoDB, Amazon DocumentDB, Firestore
- **Use-cases**: каталоги продуктів, профілі, CMS

---

## Slide 9: NoSQL — Wide-Column

- **Структура**: рядки з динамічними стовпцями, розбиті на вузли
- **Сильна сторона**: пише мільйони записів/сек, TimesSeries
- **Слабка сторона**: запити тільки по ключу / обмежені умови
- **Приклади**: Cassandra, Amazon Keyspaces, HBase
- **Use-cases**: IoT, логи, метрики, time series

---

## Slide 10: NoSQL — Graph

- **Структура**: вузли (entities) + ребра (relationships) + властивості
- **Сильна сторона**: traversal по зв'язках — природній і швидкий
- **Слабка сторона**: не підходить для агрегацій, нішевий
- **Приклади**: Neo4j, Amazon Neptune
- **Use-cases**: соціальні мережі, рекомендації, fraud detection

---

## Slide 11: Вибір БД — дерево рішень

```
Потрібні JOIN і транзакції?
  ├─ ТАК → RDBMS (PostgreSQL, RDS)
  └─ НІ  → Який доступ?
             ├─ По ключу / кеш       → Key-Value (Redis, ElastiCache)
             ├─ Вкладені документи   → Document (MongoDB, DocumentDB)
             ├─ Великі потоки записів → Wide-Column (Cassandra, Keyspaces)
             └─ Складні зв'язки      → Graph (Neo4j, Neptune)
```

---

## Slide 12: ACID — огляд

**Транзакція** — це набір операцій, що виконуються як єдине ціле.

| Буква | Властивість | Суть |
|-------|-------------|------|
| **A** | Atomicity | Все або нічого |
| **C** | Consistency | Дані завжди валідні |
| **I** | Isolation | Транзакції не заважають одна одній |
| **D** | Durability | Після COMMIT — назавжди |

---

## Slide 13: A — Atomicity

> Транзакція = неподільна одиниця. Якщо будь-який крок завершився з помилкою — відкочується весь блок.

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 500 WHERE id = 1;
  UPDATE accounts SET balance = balance + 500 WHERE id = 2;
  -- якщо тут збій → обидва UPDATE скасовуються
COMMIT;
```

---

## Slide 14: C — Consistency

> Дані завжди задовольняють усі правила (constraints, triggers, cascades).

```sql
-- Правило: balance >= 0
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
-- balance стане -200 → порушення CHECK(balance >= 0)
-- → транзакція відхиляється, дані залишаються валідними
```

---

## Slide 15: I — Isolation

> Паралельні транзакції не "бачать" незафіксованих змін одна одної.

**Рівні ізоляції (від слабого до сильного):**

| Рівень | Dirty Read | Non-Repeatable | Phantom |
|--------|-----------|----------------|---------|
| Read Uncommitted | ✅ | ✅ | ✅ |
| Read Committed | ❌ | ✅ | ✅ |
| Repeatable Read | ❌ | ❌ | ✅ |
| Serializable | ❌ | ❌ | ❌ |

---

## Slide 16: D — Durability

> Після отримання підтвердження COMMIT дані збережені навіть після збою сервера.

```
Як це реалізується:
  • WAL (Write-Ahead Log) — журнал транзакцій
  • Запис на диск до підтвердження клієнту
  • Репліка: синхронна (durability) або асинхронна (performance)
```

---

## Slide 17: ACID — приклад: переказ грошей

```
Крок 1: BEGIN TRANSACTION
Крок 2: SELECT balance FROM A  → 1000
Крок 3: balance - 500 → 500   (А: 1000 → 500)
Крок 4: balance + 500 → 800   (Б: 300 → 800)
Крок 5: перевірка constraints → OK
Крок 6: COMMIT

Якщо збій між кроком 3 і 4:
  → ROLLBACK → A залишається 1000, Б залишається 300
```

---

## Slide 18: BASE — огляд

| Буква | Розшифровка | Суть |
|-------|-------------|------|
| **B** | Basically Available | Система відповідає завжди (можливо з частковими даними) |
| **S** | Soft state | Стан може змінюватись навіть без нових записів |
| **E** | Eventually consistent | З часом усі репліки синхронізуються |

> "Ми гарантуємо відповідь, але не гарантуємо що вона найсвіжіша"

---

## Slide 19: Eventually Consistent — приклад

```
Користувач змінює аватар у соцмережі:

t=0  → запис у Region US-East  ✅
t=1  → користувач у EU бачить старий аватар  (стара репліка)
t=3  → репліка EU синхронізувалась           (eventually consistent)
t=3+ → усі бачать новий аватар               ✅

Прийнятно? Так — для соцмережі.
Прийнятно для банківського переказу? НІ.
```

---

## Slide 20: ACID vs BASE

| | ACID | BASE |
|-|------|------|
| Консистентність | Сильна (завжди) | Слабка (з часом) |
| Доступність | Може блокувати | Завжди відповідає |
| Масштабування | Складне | Природне |
| Приклади | PostgreSQL, MySQL, Oracle | Cassandra, DynamoDB, MongoDB |
| Use-cases | Фінанси, медицина, ERP | Соцмережі, IoT, каталоги |

---

## Slide 21: CAP теорема

**Теорема Брюера:** розподілена система не може одночасно гарантувати всі три:

```
    Consistency (C)
         △
        / \
       /   \
      /     \
     ▽───────▽
Availability   Partition
    (A)       tolerance (P)
```

> У разі мережевого розділення (P) — завжди обов'язкове — система обирає між C або A.

---

## Slide 22: CAP — приклади БД

| БД | CAP клас | Пояснення |
|----|---------|-----------|
| PostgreSQL | **CP** | Консистентність важливіша за доступність |
| MySQL (sync repl) | **CP** | Блокує запис при відмові репліки |
| Cassandra | **AP** | Завжди відповідає, може бути stale |
| DynamoDB | **AP** | (за замовч.) Eventually consistent reads |
| HBase | **CP** | Сильна консистентність, ZooKeeper |
| CouchDB | **AP** | Multi-master, eventual consistency |

---

## Slide 23: DB як Bottleneck

> База даних — найчастіший вузьке місце у веб-застосунках.

**Типові симптоми:**
- Час відповіді API > 500 мс
- CPU на DB-сервері > 80%
- Черга запитів зростає
- Connection pool вичерпаний

**Причини:**
- Read-heavy: багато читань без кешу
- Write-heavy: багато записів на один вузол
- Slow queries: повні сканування таблиць
- Connection exhaustion: немає pool

---

## Slide 24: Read-heavy Bottleneck

```
Проблема: 95% трафіку — читання
          Primary DB перевантажений

Рішення:
  ┌─────────────────────────────┐
  │  Read Replicas              │ ← Читання
  │  (MySQL, PostgreSQL, Aurora)│
  │  + Cache (Redis/Memcached)  │ ← Кешований результат
  └─────────────────────────────┘
     Primary ← тільки записи
```

---

## Slide 25: Write-heavy Bottleneck

```
Проблема: мільйони записів/сек
          один вузол не справляється

Рішення:
  • Sharding (горизонтальне партиціювання)
  • NoSQL (Cassandra, DynamoDB) — оптимізовані для writes
  • Async writes (queue → batch insert)
  • Write-ahead buffer (Kafka → DB)
```

---

## Slide 26: Connection Bottleneck

```
Проблема: PostgreSQL підтримує ~100–500 з'єднань
          App servers відкривають нові → вичерпання

Симптоми:
  "too many connections"  /  "connection refused"

Рішення:
  • Connection Pool: PgBouncer, RDS Proxy
  • Pool sizing: (CPU cores × 2) + disk spindles
  • Retry + backoff
```

---

## Slide 27: Slow Queries

```sql
-- Проблема: Full Table Scan
SELECT * FROM orders WHERE customer_email = 'user@example.com';
-- 10M рядків → 2.3 секунди

-- Рішення: Індекс
CREATE INDEX idx_orders_email ON orders(customer_email);
-- → 1 мс

-- Виявлення: EXPLAIN ANALYZE
EXPLAIN ANALYZE SELECT ...;
-- → Seq Scan vs Index Scan
```

---

## Slide 28: Стратегії масштабування БД

| Стратегія | Суть | Коли |
|-----------|------|------|
| **Vertical** | Більше CPU/RAM | Перший крок, простий |
| **Read Replicas** | Реплікація читань | Read-heavy |
| **Sharding** | Розбиття по вузлах | Write-heavy, великі дані |
| **Caching** | Кеш перед БД | Read-heavy, повторні запити |
| **CQRS** | Окремі моделі читання/запису | Складні системи |
| **Functional decomp.** | Окрема БД для кожного сервісу | Мікросервіси |

---

## Slide 29: Vertical Scaling (Scale Up)

```
t1: db.t3.medium  (2 vCPU, 4 GB RAM)
         ↓
t2: db.r6g.4xlarge (16 vCPU, 128 GB RAM)

Переваги:
  ✅ Просто (1 клік в AWS Console)
  ✅ Не треба міняти код
  
Недоліки:
  ❌ Дорого
  ❌ Є верхня межа
  ❌ Single point of failure
  ❌ Downtime під час зміни (якщо не Aurora)
```

---

## Slide 30: Read Replicas

```
                 ┌─ Read Replica 1 (AZ-a)  → App reads
Primary ──async──┤─ Read Replica 2 (AZ-b)  → Reporting
(writes)         └─ Read Replica 3 (AZ-c)  → Analytics

Особливості:
  • Реплікація асинхронна → можливий replication lag
  • Не підходить для read-after-write consistency
  • AWS Aurora: до 15 Read Replicas, failover на репліку
```

---

## Slide 31: Sharding

```
Shard Key: user_id % 4

user_id 0,4,8...  → Shard 0 (DB node 0)
user_id 1,5,9...  → Shard 1 (DB node 1)
user_id 2,6,10... → Shard 2 (DB node 2)
user_id 3,7,11... → Shard 3 (DB node 3)

Проблеми:
  ❌ Cross-shard JOINs складні
  ❌ Нерівномірний розподіл (hot shard)
  ❌ Re-sharding при масштабуванні
```

---

## Slide 32: Caching

```
Cache-aside (найпоширеніший):
  1. App → Cache? → HIT  → повернути
                 → MISS → DB → зберегти в Cache → повернути

Write-through:
  App → Cache → DB (синхронно)

Write-behind (write-back):
  App → Cache → (async) DB

TTL: час життя запису → автовидалення застарілих даних
```

---

## Slide 33: Connection Pooling

```
БЕЗ ПУЛУ:                    З ПУЛОМ (RDS Proxy):
App req 1 → [open conn] →    App req 1 ──┐
App req 2 → [open conn] →    App req 2 ──┤→ Pool → DB
App req 3 → [open conn] →    App req 3 ──┘
...                           (5 connections замість 1000)

AWS RDS Proxy:
  • Serverless connection pooler
  • Failover без розриву з'єднання
  • IAM authentication
```

---

## Slide 34: Anti-patterns — огляд

> Anti-pattern — поширене "рішення" яке насправді створює нові проблеми.

**Топ-5 anti-patterns баз даних:**

1. 🐌 N+1 Query
2. 🔍 Відсутність індексів
3. 🧟 God Table
4. 🎯 One DB for Everything
5. 🚪 No Connection Pooling

---

## Slide 35: Anti-pattern: N+1 Query

```python
# ПОГАНО (N+1):
orders = db.query("SELECT * FROM orders")     # 1 запит
for order in orders:
    user = db.query(f"SELECT * FROM users WHERE id={order.user_id}")
    # N запитів → якщо 1000 замовлень → 1001 запит!

# ДОБРЕ (JOIN або eager loading):
orders = db.query("""
    SELECT o.*, u.name FROM orders o
    JOIN users u ON u.id = o.user_id
""")  # 1 запит
```

---

## Slide 36: Anti-pattern: Відсутність індексів

```sql
-- Таблиця: 50M рядків

-- БЕЗ ІНДЕКСУ → Seq Scan → 8 секунд
SELECT * FROM events WHERE user_id = 42;

-- З ІНДЕКСОМ → Index Scan → 2 мс
CREATE INDEX idx_events_user_id ON events(user_id);

Як виявити:
  • EXPLAIN ANALYZE → шукаємо Seq Scan на великих таблицях
  • pg_stat_statements → топ-10 повільних запитів
  • AWS RDS Performance Insights
```

---

## Slide 37: Anti-pattern: God Table

```sql
-- ПОГАНО: одна таблиця для всього
CREATE TABLE entities (
  id INT, type VARCHAR,
  field1 VARCHAR, field2 VARCHAR, ..., field50 VARCHAR,
  data JSON  -- "все що не влізло"
);

Проблеми:
  ❌ NULL у 80% стовпців
  ❌ Неможливо додати constraints
  ❌ Неможливо нормально індексувати
  
Рішення: нормалізація, окремі таблиці по типу сутності
```

---

## Slide 38: Anti-pattern: One DB for Everything

```
ПОГАНО:
  OLTP + Analytics + Cache → одна PostgreSQL
  → Аналітичні запити блокують транзакційні

ДОБРЕ:
  OLTP      → RDS PostgreSQL / Aurora
  Analytics → Redshift / Athena (окремо)
  Cache     → ElastiCache (Redis)
  Search    → OpenSearch
  Files     → S3
```

---

## Slide 39: Anti-pattern: No Connection Pooling

```
Сценарій: Lambda функція + RDS PostgreSQL

БЕЗ ПУЛУ:
  1000 concurrent Lambdas → 1000 connections → DB crash
  "FATAL: connection limit exceeded"

З ПУЛОМ (RDS Proxy):
  1000 Lambdas → RDS Proxy → 20 connections → DB OK
  
Правило: завжди використовуй connection pool між
          serverless/stateless шаром та RDBMS
```

---

## Slide 40: Amazon RDS

**Managed Relational Database Service**

| Движок | Версії |
|--------|--------|
| PostgreSQL | 13, 14, 15, 16 |
| MySQL | 5.7, 8.0 |
| MariaDB | 10.x |
| Oracle | SE2, EE |
| SQL Server | SE, EE |

**Ключові функції:**
- Multi-AZ: синхронна реплікація + автоfailover (60–120 сек)
- Read Replicas: до 5 реплік
- Автоматичні бекапи: до 35 днів
- RDS Proxy: connection pooling

---

## Slide 41: Amazon Aurora

**MySQL/PostgreSQL-сумісна, хмарна СУБД від AWS**

```
  6 копій даних у 3 AZ автоматично
  (2 копії на AZ)
  
  Primary ── write ──→ Shared Storage
  Replica 1 ─ read ──→ (up to 15 replicas)
  Replica 2 ─ read ──→
  
  Failover: ~30 секунд (vs 60–120 у RDS)
  ~5x швидше MySQL, ~3x швидше PostgreSQL
  Aurora Serverless v2: auto-scale за навантаженням
```

---

## Slide 42: Amazon DynamoDB

**Serverless NoSQL — Key-Value + Document**

| Характеристика | Значення |
|----------------|---------|
| Latency | Sub-millisecond |
| Capacity modes | On-demand / Provisioned |
| Indexes | GSI (Global), LSI (Local) |
| Streams | Real-time change capture |
| DAX | In-memory cache, мікросекунди |
| Global Tables | Multi-region active-active |

> Ідеально для: сесій, кошика, IoT, gaming leaderboards

---

## Slide 43: Amazon ElastiCache

**Managed Redis / Memcached**

```
  App → ElastiCache (Redis) → Cache HIT  → відповідь (мікросекунди)
                            → Cache MISS → RDS → зберегти в cache

Redis:
  ✅ Структури даних (lists, sets, sorted sets)
  ✅ Pub/Sub
  ✅ Persistence (RDB + AOF)
  ✅ Cluster mode (sharding)
  
Memcached:
  ✅ Простий KV кеш
  ✅ Multi-threaded
  ❌ Немає persistence / replication
```

---

## Slide 44: AWS DB Services — Overview

| Сервіс | Тип | Use-case |
|--------|-----|---------|
| RDS | RDBMS managed | Стандартні SQL застосунки |
| Aurora | RDBMS high-perf | Великі OLTP навантаження |
| DynamoDB | NoSQL KV/Doc | Scale, serverless, low latency |
| ElastiCache | Cache (Redis/MC) | Кешування, сесії, черги |
| Redshift | Data Warehouse | Аналітика (OLAP) |
| DocumentDB | Document NoSQL | MongoDB-compatible |
| Neptune | Graph | Графи зв'язків |
| Keyspaces | Wide-Column | Cassandra-compatible |

---

## Slide 45: Ключові висновки

1. **RDBMS** = ACID + SQL + вертикальне масштабування
2. **NoSQL** = BASE + гнучкість + горизонтальне масштабування
3. **CAP**: консистентність або доступність — обери одне
4. **Read-heavy** → Read Replicas + Cache
5. **Write-heavy** → Sharding або NoSQL
6. **Connection bottleneck** → Connection Pool (RDS Proxy)
7. **Уникай**: N+1, God Table, One DB for Everything
8. **AWS**: RDS/Aurora для SQL, DynamoDB для NoSQL, ElastiCache для кешу

> "Use the right tool for the right job"
