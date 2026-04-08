# Лекція 2: Глобальна архітектура хмар та відповідальність — Конспект

## 1. Регіони, Availability Zones, Edge

**Region**: незалежна географічна зона з кількома AZ (наприклад, `eu-central-1` — Frankfurt).
**Availability Zone (AZ)**: один або кілька датацентрів у межах регіону, фізично ізольовані, пов'язані low-latency fiber. Наприклад: `eu-central-1a`, `eu-central-1b`.
**Edge Locations**: точки присутності CloudFront CDN (~400+). Кешування та низька latency для кінцевих користувачів.

**Принцип вибору регіону**:
1. Compliance (GDPR, дані ЄС → eu-* регіони)
2. Latency (близькість до користувачів)
3. Availability сервісів (нові сервіси спочатку в us-east-1)
4. Вартість (ціни відрізняються між регіонами)

---

## 2. Latency та Data Residency

**Latency** — час відповіді. Фізично: 1ms ≈ 200km. CDN зменшує latency для статичного контенту.

**Data Residency**: де фізично зберігаються дані. Критично для GDPR (дані ЄС = в ЄС). AWS гарантує: без явного дозволу дані не переміщуються між регіонами.

---

## 3. SLA, SLO, Availability

**SLA (Service Level Agreement)**: юридичний контракт. AWS S3 = 99.9% monthly uptime.
**SLO (Service Level Objective)**: внутрішня ціль команди (наприклад, 99.95%).
**SLI (Service Level Indicator)**: метрика (наприклад, % successful requests).

**Availability розрахунок**:
```
99.9%   → ~8.7 год downtime/рік
99.99%  → ~52 хв downtime/рік
99.999% → ~5.2 хв downtime/рік (five nines)
```

**Error Budget**: 100% − SLO. При 99.9% SLO → 0.1% = 8.7 год/рік на помилки та деплойменти.

---

## 4. Shared Responsibility Model

| Відповідальність | AWS | Клієнт |
|-----------------|-----|--------|
| Фізичний захист | ✅ | — |
| Hardware, мережа | ✅ | — |
| Гіпервізор | ✅ | — |
| ОС, патчі | — | ✅ (EC2) |
| Дані, шифрування | — | ✅ |
| IAM, доступ | — | ✅ |

**"Security OF the cloud"** — AWS відповідає за інфраструктуру.
**"Security IN the cloud"** — ви відповідаєте за налаштування та дані.

---

## 5. RTO та RPO

**RPO (Recovery Point Objective)**: скільки даних можна втратити (максимальний вік backup). RPO = 1h → backup щогодини.

**RTO (Recovery Time Objective)**: час на відновлення системи після збою. RTO = 30min → система має бути up за 30 хвилин.

| Стратегія DR | RTO | RPO | Вартість |
|-------------|-----|-----|----------|
| Backup & Restore | Години | Години | Низька |
| Pilot Light | ~10 хв | Хвилини | Середня |
| Warm Standby | Хвилини | Секунди | Висока |
| Multi-site Active | ~0 | ~0 | Дуже висока |

---

## 6. Design for Failure

Принципи проектування відмовостійких систем:
- **Multi-AZ**: поширювати ресурси між AZ
- **Eliminate SPOF**: будь-який елемент, що може відмовити = проблема
- **Assume failure**: код повинен обробляти збої gracefully
- **Loose coupling**: сервіси не залежать жорстко один від одного
- **Health checks**: ALB, Route 53 health checks → автоматичний failover
