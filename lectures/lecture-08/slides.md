# Лекція 8: Масштабування та відмовостійкість — Слайди

---

## Slide 1: Назва

# Лекція 8: Масштабування та відмовостійкість

---

## Slide 2: Single Point of Failure

```
❌ SPOF архітектура:        ✅ Resilient архітектура:
  Internet                    Internet
     │                           │
   EC2 (один!)               ALB (Multi-AZ)
     │                        ╱         ╲
   RDS (один!)             EC2(AZ-a)  EC2(AZ-b)
                               ╲         ╱
                            RDS Multi-AZ
```

---

## Slide 3: Load Balancer Types

| LB | Layer | Use Case |
|----|-------|----------|
| ALB | 7 (HTTP) | Web, mікросервіси |
| NLB | 4 (TCP) | Gaming, IoT, ultra-low latency |
| GWLB | 3 | Firewall, IDS/IPS |

---

## Slide 4: ALB Routing Rules

```
GET /api/*        → Target Group: api-servers
GET /static/*     → Target Group: cdn-servers
Host: admin.*     → Target Group: admin-servers
```

ALB вміє: path-based, host-based, header-based routing

---

## Slide 5: Health Checks

```
ALB → GET /health → EC2
              ↓
         200 OK {"status":"ok"}  → In rotation
         500 Error               → Out (3 failures)
         Timeout                 → Out
```

Перевіряй залежності: DB, cache — не тільки "process alive"

---

## Slide 6: Auto Scaling Group

```
CloudWatch: CPU > 70%
    → Scale Out (+2 instances, cooldown 300s)

CloudWatch: CPU < 20%
    → Scale In (-1 instance, cooldown 300s)

ASG: min=2, desired=4, max=10
```

---

## Slide 7: DR Стратегії

| Стратегія | RTO | Вартість |
|-----------|-----|----------|
| Backup & Restore | Год | 💰 |
| Pilot Light | ~10хв | 💰💰 |
| Warm Standby | Хвилини | 💰💰💰 |
| Multi-site Active | ~0 | 💰💰💰💰 |

---

## Slide 8: Blue/Green Deployment

```
Route 53 / ALB Weighted:
  Blue  (v1): 100% → 0%
  Green (v2):   0% → 100%

Rollback: 30 секунд (перемикнути ваги)
```

---

## Slide 9: Circuit Breaker

```
CLOSED         → запити проходять
(N помилок)
    ↓
OPEN           → fail fast (не чекаємо timeout)
(timeout)
    ↓
HALF-OPEN      → пробний запит
(success)
    ↓
CLOSED         → нормальна робота
```

---

## Slide 10: Ключові висновки

1. Eliminate SPOF: Multi-AZ для всього критичного
2. ALB: Layer 7, health checks, path routing
3. ASG: автоматичне масштабування + самовідновлення
4. DR: RPO/RTO визначають стратегію та вартість
5. Blue/Green: zero-downtime deploys
