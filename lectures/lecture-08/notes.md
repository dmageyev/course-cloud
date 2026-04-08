# Лекція 8: Масштабування та відмовостійкість — Конспект

## 1. Single Point of Failure (SPOF)

**SPOF** — компонент системи, відмова якого призводить до повної зупинки сервісу.

Типові SPOF:
- Один EC2 без Auto Scaling
- Один RDS без Multi-AZ
- Один NAT Gateway (один AZ)
- Один Load Balancer
- Один DNS resolver

**Усунення SPOF**: дублювання у кількох AZ, Auto Scaling, Multi-AZ для БД.

---

## 2. Load Balancing Patterns

**Application Load Balancer (ALB)**: Layer 7 (HTTP/HTTPS). Routing по URL, заголовках, методі. Підтримує WebSocket. Для мікросервісів та containers.

**Network Load Balancer (NLB)**: Layer 4 (TCP/UDP). Мільйони запитів/секунду, ultra-low latency. Для gaming, IoT, VoIP.

**Gateway Load Balancer (GWLB)**: Layer 3. Для transparent inline network appliances (firewall, IDS/IPS).

**Алгоритми балансування**:
- Round Robin (default)
- Least Outstanding Requests
- Sticky Sessions (IP Hash)

---

## 3. Health Checks

**ALB Health Check**: HTTP GET до `/health` endpoint кожні N секунд. Якщо N consecutive failures → інстанс виводиться з ротації.

```
GET /health HTTP/1.1
Host: 10.0.1.42

HTTP/1.1 200 OK
{"status":"healthy","db":"connected","cache":"ok"}
```

**Best Practices**:
- Окремий `/health` endpoint (не `GET /`)
- Перевіряй залежності (DB, cache connectivity)
- Швидкий response (< 1 сек)

---

## 4. Auto Scaling

**Auto Scaling Group (ASG)**: управляє групою EC2 інстансів. Зберігає desired capacity, замінює нездорові інстанси.

**Scaling Policies**:
- **Target Tracking**: підтримує метрику на рівні (CPU = 60%)
- **Step Scaling**: крок залежить від відхилення
- **Scheduled**: за розкладом (ранок/вечір)

**Cooldown period** (300с): пауза між scaling подіями.

**Lifecycle Hooks**: дозволяє виконати дії до termination або після launch (drain connections, register in service mesh).

---

## 5. Disaster Recovery Strategies

| Стратегія | RTO | RPO | Вартість |
|-----------|-----|-----|----------|
| Backup & Restore | Год | Год | Мінімальна |
| Pilot Light | ~10 хв | Хв | Низька |
| Warm Standby | Хв | Сек | Середня |
| Multi-site Active/Active | ~0 | ~0 | Висока |

**Pilot Light**: мінімальна інфраструктура завжди запущена (БД sync, AMI готовий). При DR — розгорнути повну систему.

**Warm Standby**: зменшена копія production. При DR — scale up.

---

## 6. Blue/Green Deployment

```
           Router (Route 53 / ALB)
              │
    ┌─────────┴─────────┐
    │                   │
Blue (v1.0) 100%    Green (v2.0) 0%
    │                   │
  Stable              New version
                    (тестується)

После тестування:
  Blue 0%         Green 100%
  (standby/terminate)
```

**Переваги**: миттєвий rollback (перемикнути трафік назад), нема downtime.

---

## 7. Circuit Breaker Pattern

Захищає від каскадних відмов. Якщо downstream сервіс відповідає з помилками → circuit "відкривається", запити fallback одразу без очікування timeout.

```
CLOSED → (N failures) → OPEN → (timeout) → HALF-OPEN → (success) → CLOSED
```
