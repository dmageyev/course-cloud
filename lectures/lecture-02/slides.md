# Лекція 2: Глобальна архітектура хмар та відповідальність — Слайди

---

## Slide 1: Назва

# Лекція 2: Глобальна архітектура хмар та відповідальність

---

## Slide 2: Region → AZ → Edge

```
Region: eu-central-1 (Frankfurt)
  ├── AZ: eu-central-1a  (datacenter)
  ├── AZ: eu-central-1b  (datacenter)
  └── AZ: eu-central-1c  (datacenter)

Edge Locations: 400+ пунктів по світу (CloudFront CDN)
```

---

## Slide 3: Як обрати регіон?

1. Compliance → GDPR: eu-central-1, eu-west-1
2. Latency → близькість до користувачів
3. Feature availability → нові сервіси спочатку в us-east-1
4. Cost → ціни відрізняються ~20%

---

## Slide 4: SLA / SLO / SLI

| Термін | Опис | Приклад |
|--------|------|---------|
| SLA | Юридичний контракт | AWS S3 = 99.9% |
| SLO | Внутрішня ціль | 99.95% |
| SLI | Вимірювана метрика | % успішних запитів |

Error Budget = 100% - SLO

---

## Slide 5: Availability у числах

```
99%     → 87.6 год downtime/рік
99.9%   →  8.7 год downtime/рік
99.99%  → 52.6 хв downtime/рік
99.999% →  5.3 хв downtime/рік
```

---

## Slide 6: Shared Responsibility Model

```
AWS відповідає:          ВИ відповідаєте:
  Physical security         OS patches
  Hardware                  App configuration
  Hypervisor                Data encryption
  Managed service OS        IAM policies
```

---

## Slide 7: RTO та RPO

- **RPO** = скільки даних можна втратити (час останнього backup)
- **RTO** = час відновлення після збою

DR стратегії (дешево → дорого):
Backup/Restore → Pilot Light → Warm Standby → Multi-site Active

---

## Slide 8: Design for Failure

- Multi-AZ deployment
- Eliminate SPOF (Single Point of Failure)
- Health checks + auto-recovery
- Loose coupling (SQS між сервісами)
- Graceful degradation

---

## Slide 9: Real-world AWS Outage

- us-east-1 2017: S3 outage — постраждали сотні сервісів
- Урок: не залежи від одного регіону для критичних систем
- Multi-region → складніше, але більш resilient

---

## Slide 10: Ключові висновки

1. Region → AZ → Edge: вибір впливає на latency та compliance
2. SLA ≠ SLO: перший — контракт, другий — ваша ціль
3. Shared Responsibility: AWS = security OF, ви = security IN
4. RPO/RTO визначають архітектуру DR
5. Design for failure = стандарт production систем
