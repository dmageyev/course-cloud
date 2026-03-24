# Лекція 7: Хмарні бази даних — Слайди

---

## Slide 1: Назва

# Лекція 7: Хмарні бази даних

---

## Slide 2: RDBMS vs NoSQL

| | RDBMS | NoSQL |
|--|-------|-------|
| Схема | Фіксована | Гнучка |
| Мова | SQL | Varied API |
| Scaling | Vertical | Horizontal |
| ACID | ✅ | Частково |
| Use case | Транзакції | Scale, flexible |

---

## Slide 3: NoSQL типи

| Тип | Приклад | AWS |
|-----|---------|-----|
| Key-Value | Redis | ElastiCache |
| Document | MongoDB | DocumentDB |
| Wide Column | Cassandra | Keyspaces |
| Graph | Neo4j | Neptune |

---

## Slide 4: ACID

```
A - Atomicity: транзакція = все або нічого
C - Consistency: дані завжди валідні
I - Isolation: паралельні tx незалежні
D - Durability: після COMMIT = збережено
```

---

## Slide 5: BASE (NoSQL)

```
B - Basically Available
A - (немає) — Soft state
S - Soft state: стан може змінюватись
E - Eventually consistent
```

→ "З часом все буде консистентно"

---

## Slide 6: Database як Bottleneck

```
Read-heavy:  → Read Replicas, ElastiCache
Write-heavy: → Sharding, DynamoDB
Connections: → RDS Proxy (connection pool)
Query slow:  → Indexes, Query optimization
```

---

## Slide 7: Amazon RDS Multi-AZ

```
Primary (AZ-a)  ←sync→  Standby (AZ-b)
       │
     Failover (60–120с)
       ↓
Standby стає Primary
```

---

## Slide 8: Amazon Aurora

- MySQL/PostgreSQL compatible
- 6 копій у 3 AZ автоматично
- До 15 Read Replicas
- Failover ~30 сек
- ~5x швидше MySQL

---

## Slide 9: Amazon DynamoDB

- NoSQL key-value + document
- Sub-ms latency
- On-demand capacity (no planning)
- Global Tables: multi-region active-active
- DAX: in-memory cache

---

## Slide 10: Ключові висновки

1. RDBMS = ACID, SQL, vertical scale
2. NoSQL = BASE, flexible, horizontal scale
3. Multi-AZ = HA, Read Replicas = performance
4. Aurora = managed MySQL/PostgreSQL на стероїдах
5. DynamoDB = serverless NoSQL для scale
