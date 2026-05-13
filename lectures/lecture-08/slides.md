# Лекція 8: Масштабування та відмовостійкість — Слайди

---

## Slide 1: Назва (Title)

# Лекція 8: Масштабування та відмовостійкість
**Курс:** Хмарні технології інформаційних систем
Бакалавр ІТ · 3 курс · 2 години

---

## Slide 2: Agenda

1. Чому важливо?
2. Single Point of Failure (SPOF)
3. Виявлення та усунення SPOF
4. Load Balancing patterns
5. Health Checks
6. Auto Scaling стратегії
7. Disaster Recovery (DR)
8. Blue/Green Deployment
9. AWS приклади: ALB, ASG
10. Підсумки

---

## Slide 3: Чому важливо?

**Реальні інциденти:**

| Компанія | Рік | Причина | Downtime | Втрати |
|----------|-----|---------|----------|--------|
| GitHub | 2018 | Network partition | 24 год | Репутація |
| AWS S3 | 2017 | Human error | 4 год | ~$150M |
| Facebook | 2021 | BGP misconfiguration | 6 год | ~$100M |

**Статистика:**
- 1 година downtime для e-commerce = $100K–$5M втрат
- 99.9% uptime = 8.76 год downtime/рік
- 99.99% uptime = 52 хв downtime/рік

---

## Slide 4: SPOF — визначення

**Single Point of Failure (SPOF)** — компонент системи, відмова якого призводить до повної зупинки системи.

```
┌──────────────────┐
│   Load Balancer  │ ← SPOF!
└────────┬─────────┘
         │
    ┌────┴────┐
    │         │
┌───▼──┐  ┌──▼───┐
│ App1 │  │ App2 │
└──────┘  └──────┘
```

**Наслідки:**
- ❌ Повна недоступність сервісу
- ❌ Втрата доходів
- ❌ Погана репутація
- ❌ Порушення SLA

---

## Slide 5: Типові SPOF

| Компонент | Приклад SPOF | Наслідок |
|-----------|--------------|----------|
| **Сервер** | Один web-сервер | App недоступний |
| **База даних** | Single DB instance | Неможливість читання/запису |
| **Load Balancer** | Один LB | Трафік не розподіляється |
| **DNS** | Один DNS provider | Домен не резолвиться |
| **Мережа** | Один ISP | Повна ізоляція |
| **AZ** | Всі ресурси в одній AZ | Відмова при AZ outage |
| **Region** | Один AWS регіон | Відмова при regional outage |

---

## Slide 6: Виявлення SPOF

**Питання для аналізу архітектури:**

1. ✓ Що станеться, якщо цей компонент відмовить?
2. ✓ Чи є резервна копія цього компонента?
3. ✓ Чи може система працювати без нього?
4. ✓ Чи розподілені компоненти між AZ/регіонами?
5. ✓ Чи є автоматичне failover?

**Інструменти:**
- Fault Tree Analysis (FTA)
- Architecture Review
- Chaos Engineering (Netflix Chaos Monkey)

---

## Slide 7: Усунення SPOF

**Стратегії:**

```
1. Redundancy (надлишковість)
   Single server → Multiple servers

2. Реплікація
   Master DB → Master + Replicas

3. Multi-AZ
   One AZ → Multiple AZs

4. Multi-Region
   One Region → Multiple Regions

5. Automated Failover
   Manual switch → Auto failover
```

**Принцип:** Кожен критичний компонент має мати N+1 або N+2 резервування.

---

## Slide 8: Приклад: від SPOF до HA

**Еволюція архітектури:**

```
КРОК 1: Single Server (SPOF)
[Users] → [Single EC2] → [Single RDS]
  ❌ Будь-яка відмова = повний downtime

КРОК 2: Multi-AZ
[Users] → [ALB] → [EC2-AZa, EC2-AZb] → [RDS Multi-AZ]
  ✓ Failover при відмові AZ

КРОК 3: Multi-Region
[Users] → [Route53]
           ├→ [Region US: ALB → EC2s → RDS]
           └→ [Region EU: ALB → EC2s → RDS replica]
  ✓ DR при відмові регіону
```

---

## Slide 9: Load Balancing — навіщо?

**Три основні цілі:**

1. **Розподіл навантаження**
   - Рівномірний розподіл трафіку між серверами
   - Уникнення перевантаження одного сервера

2. **Відмовостійкість**
   - Автоматичне виключення unhealthy серверів
   - Продовження роботи при відмові частини серверів

3. **Масштабування**
   - Додавання нових серверів без downtime
   - Elastic capacity під навантаження

---

## Slide 10: Типи Load Balancers

| Тип | Рівень OSI | Протокол | Use Case |
|-----|------------|----------|----------|
| **L4 (Network)** | Transport (4) | TCP, UDP | High throughput, низька затримка, статичний IP |
| **L7 (Application)** | Application (7) | HTTP, HTTPS, WebSocket | Routing по URL/headers, SSL termination |

**Layer 4:**
```
Client → NLB (IP:port) → Backend (IP:port)
         • Дуже швидкий
         • Не бачить HTTP
```

**Layer 7:**
```
Client → ALB (HTTP headers) → Backend
         • URL-based routing
         • Host-based routing
         • Cookie-based affinity
```

---

## Slide 11: Алгоритми балансування

```
1. Round Robin
   Request 1 → Server A
   Request 2 → Server B
   Request 3 → Server C
   Request 4 → Server A ...

2. Least Connections
   → Сервер з найменшою кількістю активних з'єднань

3. IP Hash
   hash(client_ip) % num_servers → Server X
   (той самий клієнт → той самий сервер)

4. Weighted Round Robin
   Server A (weight=3): 60% трафіку
   Server B (weight=2): 40% трафіку
```

---

## Slide 12: Session Affinity (Sticky Sessions)

**Проблема:** Додаток зберігає сесію локально на сервері.

```
Without Sticky:
Request 1 (login) → Server A (session створено)
Request 2 (profile) → Server B (немає сесії!) ❌

With Sticky:
Request 1 (login) → Server A (cookie встановлено)
Request 2 (profile) → Server A (та сама сесія) ✓
```

**Недоліки:**
- ❌ Нерівномірний розподіл навантаження
- ❌ Ускладнює Auto Scaling

**Краще рішення:** Зовнішнє сховище сесій (Redis, DynamoDB)

---

## Slide 13: AWS Application Load Balancer (ALB)

**Основні можливості:**

```yaml
Target Groups:
  - web-servers: [EC2-1, EC2-2, EC2-3]
  - api-servers: [EC2-4, EC2-5]

Rules:
  - Path /api/* → api-servers
  - Path /* → web-servers
  - Host api.example.com → api-servers
  - Host www.example.com → web-servers
```

**Features:**
- ✓ HTTP/2, WebSockets, gRPC
- ✓ SSL/TLS termination
- ✓ Sticky sessions (cookie-based)
- ✓ WAF integration
- ✓ Lambda targets

---

## Slide 14: AWS Network Load Balancer (NLB)

**Характеристики:**

- **Ultra-low latency** (~100 microseconds)
- **Millions req/sec**
- **Static IP** (один IP на AZ)
- **TCP, UDP, TLS**
- **Preserve source IP**

**Use cases:**
- Gaming (UDP)
- IoT (MQTT)
- Extreme performance
- Non-HTTP protocols
- Need static IP for whitelisting

```
[Client] → [NLB static IP] → [Target EC2 бачить real client IP]
```

---

## Slide 15: Паттерни розгортання LB

```
1. Single AZ (не рекомендується)
   [LB in AZ-a] → [EC2s in AZ-a]
   ❌ SPOF на рівні AZ

2. Multi-AZ (стандарт)
   [LB in AZ-a, AZ-b] → [EC2s in AZ-a, AZ-b]
   ✓ Failover між AZ

3. Global Load Balancing
   [Route53 Geo-routing]
     ├→ US-EAST: [ALB] → [EC2s]
     └→ EU-WEST: [ALB] → [EC2s]
   ✓ Latency-based routing
```

---

## Slide 16: Health Checks — що це?

**Health Check** — автоматична перевірка працездатності сервісу.

```
Load Balancer перевіряє кожні 30 сек:
  GET /health → 200 OK?
    ✓ Healthy → route traffic
    ❌ Unhealthy → stop traffic
```

**Навіщо?**
- Автоматично виключає проблемні інстанси
- Запобігає помилкам "connection refused"
- Працює разом з Auto Scaling

---

## Slide 17: Типи Health Checks

```
1. Shallow (поверхневий)
   GET /health → 200 OK
   • Просто перевіряє, що процес живий
   • Швидко, але не інформативно

2. Deep (глибокий)
   GET /health → перевіряє:
     ✓ DB connection
     ✓ Redis connection
     ✓ Disk space > 10%
   • Більш надійно
   • Повільніше

3. Dependency Checks
   GET /ready → перевіряє всі залежності
   • Kubernetes-style: /live vs /ready
```

**Рекомендація:** Shallow для production traffic, Deep для monitoring.

---

## Slide 18: Параметри Health Checks

```yaml
HealthCheck:
  Path: /health
  Protocol: HTTP
  Port: 80
  Interval: 30 sec           # Як часто перевіряти
  Timeout: 5 sec             # Скільки чекати відповіді
  HealthyThreshold: 2        # 2 успішні → healthy
  UnhealthyThreshold: 3      # 3 невдалі → unhealthy
```

**Важливо:**
- `Timeout < Interval`
- Не робити HC занадто частими (навантаження)
- UnhealthyThreshold > 1 (уникати false positives)

---

## Slide 19: Health Check Endpoints

**Стандартні ендпоінти:**

```python
# Liveness (чи працює процес?)
@app.get("/health")
def health():
    return {"status": "ok"}

# Readiness (чи готовий приймати трафік?)
@app.get("/ready")
def ready():
    if db.is_connected() and cache.is_connected():
        return {"status": "ready"}
    else:
        return {"status": "not ready"}, 503

# Detailed (для debugging)
@app.get("/health/detailed")
def health_detailed():
    return {
        "db": check_db(),
        "cache": check_cache(),
        "disk": check_disk()
    }
```

---

## Slide 20: AWS Health Checks

```
1. ELB Health Checks
   ALB/NLB → перевіряє targets кожні 30 сек
   Unhealthy → stop routing traffic

2. ASG Health Checks
   Types:
     • EC2 (instance running?)
     • ELB (traffic healthy?)
   Unhealthy → terminate & replace instance

3. Route53 Health Checks
   Перевіряє endpoint доступність
   Unhealthy → failover to secondary

4. CloudWatch + SNS
   Custom metrics → Alarm → notify team
```

---

## Slide 21: Auto Scaling — концепція

**Auto Scaling** — автоматичне масштабування кількості ресурсів під навантаження.

```
High Load:
  9:00 AM  ████████████  → 10 instances

Low Load:
  3:00 AM  ██            → 2 instances
```

**Переваги:**
- ✓ Оптимізація вартості (платите за використання)
- ✓ Автоматична реакція на зміни навантаження
- ✓ Висока доступність

**Умова:** Додаток має бути **stateless**.

---

## Slide 22: Вертикальне vs Горизонтальне масштабування

| | Vertical (Scale Up/Down) | Horizontal (Scale Out/In) |
|---|--------------------------|---------------------------|
| **Що** | Збільшення CPU/RAM інстанса | Додавання/видалення інстансів |
| **Межі** | Є ліміт (максимальний EC2) | Практично необмежене |
| **Downtime** | Зазвичай потрібен (reboot) | Без downtime |
| **Складність** | Просто | Потрібен LB, stateless design |
| **Вартість** | Дорожче (нелінійна ціна) | Дешевше (лінійна) |
| **AWS** | Зміна instance type | Auto Scaling Group |

**Рекомендація:** Horizontal scaling для web/API, Vertical для legacy/stateful систем.

---

## Slide 23: Метрики масштабування

```
1. CPU Utilization
   CPU > 70% → scale out
   CPU < 30% → scale in

2. Memory Utilization
   Memory > 80% → scale out

3. Network I/O
   NetworkIn/Out > threshold

4. Custom Metrics (найкраще!)
   • Queue depth (SQS messages)
   • Request latency (p99 > 500ms)
   • Active connections
   • Business metrics (orders/sec)
```

**Важливо:** CPU — простий, але не завжди правильний індикатор. Custom metrics точніші.

---

## Slide 24: Scaling Policies

```yaml
1. Target Tracking
   "Тримай CPU на рівні 50%"
   → ASG сам додає/видаляє інстанси

2. Step Scaling
   CPU 50-60%  → +1 instance
   CPU 60-80%  → +2 instances
   CPU > 80%   → +3 instances

3. Scheduled Scaling
   8:00 AM (перед робочим днем) → scale to 10
   6:00 PM (після роботи)        → scale to 2

4. Predictive Scaling
   ML-based прогноз на основі історії
```

**Best:** Target Tracking для більшості сценаріїв.

---

## Slide 25: Cooldown Period

**Проблема:** Flapping (постійне scale out/in)

```
Without Cooldown:
10:00 → CPU 80% → +1 instance
10:01 → CPU 85% → +1 instance (ще не встиг запуститися попередній!)
10:02 → CPU 90% → +1 instance
...
10:05 → CPU 20% (всі запустилися) → remove all! ❌
```

**Рішення:** Cooldown Period

```
Cooldown = 300 sec (5 хв)

10:00 → CPU 80% → +1 instance → WAIT 5 min
10:05 → check again
```

**Scale-out:** Короткий cooldown (scale швидко)
**Scale-in:** Довгий cooldown (scale обережно)

---

## Slide 26: AWS Auto Scaling Groups (ASG)

```yaml
AutoScalingGroup:
  LaunchTemplate: my-app-template  # AMI, instance type, userdata
  MinSize: 2                        # Мінімум
  MaxSize: 10                       # Максимум
  DesiredCapacity: 4                # Поточна ціль

  VPCZoneIdentifier:
    - subnet-az-a
    - subnet-az-b
    - subnet-az-c

  TargetGroups:
    - my-alb-target-group

  HealthCheckType: ELB
  HealthCheckGracePeriod: 300
```

**Features:**
- ✓ Автоматичний баланс між AZ
- ✓ Заміна unhealthy instances
- ✓ Integration з ELB

---

## Slide 27: Приклад ASG політики

```yaml
ScalingPolicies:
  - Name: scale-out
    AdjustmentType: ChangeInCapacity
    ScalingAdjustment: +2
    Trigger:
      MetricName: CPUUtilization
      Threshold: 70
      ComparisonOperator: GreaterThanThreshold
      Period: 60 sec
      EvaluationPeriods: 2  # 2 periods підряд
    Cooldown: 180

  - Name: scale-in
    AdjustmentType: ChangeInCapacity
    ScalingAdjustment: -1
    Trigger:
      MetricName: CPUUtilization
      Threshold: 30
      ComparisonOperator: LessThanThreshold
      Period: 300 sec
      EvaluationPeriods: 3
    Cooldown: 600  # Довший для scale-in
```

---

## Slide 28: Best Practices ASG

```
✓ Stateless додатки
  Зберігай сесії в Redis/DynamoDB, не локально

✓ Graceful shutdown
  SIGTERM → close connections → exit
  HealthCheckGracePeriod для warm-up

✓ Fast startup
  Use AMI з преінстальованим софтом
  Minimize userdata script

✓ Тестуй scaling
  Load testing → verify scale out/in працює

✓ Monitors & Alarms
  CloudWatch metrics + SNS notifications

✗ Avoid manual capacity changes
  Use Desired Capacity тільки для override
```

---

## Slide 29: Disaster Recovery (DR) — визначення

**Disaster Recovery** — процес відновлення IT-інфраструктури після катастрофи.

**Типи катастроф:**
- 🔥 Пожежа в data center
- 💧 Повінь
- 🌪️ Стихійне лихо
- 💻 Кібератака (ransomware)
- 🧑‍💻 Human error (rm -rf /)
- 🏢 Відмова всього регіону (AWS outage)

**Мета DR:** Мінімізувати RTO та RPO.

---

## Slide 30: RTO vs RPO

```
         Normal Operation
              │
              ▼
         🔥 DISASTER 🔥
              │
              ├─────────────┬────────────────┐
              │             │                │
            Disaster      Recovery        Fully
            Occurs        Starts         Operational

              ◄────RPO────►
                            ◄─────RTO─────►
```

**RPO (Recovery Point Objective):**
- Максимально допустима **втрата даних** (години/хвилини)
- "Скільки даних можемо втратити?"

**RTO (Recovery Time Objective):**
- Максимально допустимий **час відновлення**
- "Скільки часу можемо бути offline?"

---

## Slide 31: DR стратегії — огляд

| Стратегія | RTO | RPO | Вартість | Use Case |
|-----------|-----|-----|----------|----------|
| **Backup & Restore** | Години–дні | Години | $ | Некритичні системи |
| **Pilot Light** | Хвилини–години | Хвилини | $$ | Середня критичність |
| **Warm Standby** | Хвилини | Секунди–хвилини | $$$ | Важливі системи |
| **Multi-Site (Hot)** | Секунди | ≈0 | $$$$ | Mission-critical |

---

## Slide 32: Backup & Restore

**Концепція:** Регулярні бекапи → Відновлення при disaster.

```
Normal Operation:
  [Production in Region A]
     ↓ nightly backup
  [S3 snapshots in Region B]

Disaster:
  [Region A unavailable ❌]
     ↓ restore (manual)
  [Rebuild in Region B] → 4-12 hours
```

**AWS інструменти:**
- EBS snapshots → cross-region copy
- RDS automated backups → multi-region
- S3 Cross-Region Replication

**RTO:** 4–24 години | **RPO:** 1–24 години | **Вартість:** Найдешевше

---

## Slide 33: Pilot Light / Warm Standby

**Pilot Light:** Мінімальна критична інфра постійно running.

```
Region A (Primary):       Region B (Pilot Light):
  [ALB + EC2s + RDS]        [RDS replica (read-only)]
                            [AMI ready, but EC2s off]

Disaster → Launch EC2s from AMI → Promote RDS replica → 30-60 min
```

**Warm Standby:** Часткова інфра running, але зменшений capacity.

```
Region A (Primary):       Region B (Warm Standby):
  [10 EC2 instances]        [2 EC2 instances running]
  [db.r5.2xlarge]           [db.r5.large]

Disaster → Scale up Region B → 10-30 min
```

**RTO:** 10–60 хв | **RPO:** Секунди–хвилини | **Вартість:** Середня

---

## Slide 34: Multi-Site (Hot Standby)

**Концепція:** Два повністю активні середовища, трафік розподілений.

```
         [Route53]
         /        \
        /          \
   Region A      Region B
   (Active)      (Active)
   [Full stack]  [Full stack]

   Real-time data replication
```

**Реалізація:**
- Route53 weighted routing (50%/50%)
- Aurora Global Database (replication lag < 1 sec)
- DynamoDB Global Tables

**RTO:** Секунди (автоматичний failover)
**RPO:** Практично 0 (real-time replication)
**Вартість:** Подвійна інфраструктура (найдорожче)

---

## Slide 35: Blue/Green Deployment — концепція

**Два ідентичні середовища:**
- **Blue:** Поточна production версія
- **Green:** Нова версія

```
BEFORE:
  [Users] → [Route53] → [Blue Environment v1.0] ← 100% traffic
                        [Green Environment v2.0] ← 0% traffic

AFTER (switch):
  [Users] → [Route53] → [Blue Environment v1.0] ← 0% traffic
                        [Green Environment v2.0] ← 100% traffic
```

**Переваги:**
- ✓ Zero-downtime deployment
- ✓ Instant rollback (просто перемкнути назад)
- ✓ Можливість тестувати Green перед switch

---

## Slide 36: Процес Blue/Green

```
1. Deploy нову версію в Green
   Blue (v1.0) ← 100% traffic
   Green (v2.0) ← 0% traffic (testing only)

2. Тестування Green
   Smoke tests, integration tests
   Internal team testing

3. Поступовий перехід (canary)
   Blue ← 90% traffic
   Green ← 10% traffic
   → Monitor metrics

4. Повний перехід
   Blue ← 0%
   Green ← 100%

5. Тримати Blue standby (2-24 год)
   На випадок rollback

6. Деактивувати Blue (або він стає новим standby)
```

---

## Slide 37: Переваги та недоліки Blue/Green

**Переваги:**
- ✅ Zero downtime
- ✅ Instant rollback (секунди)
- ✅ Production-like testing (Green = копія Blue)
- ✅ Зменшує ризик deployment

**Недоліки:**
- ❌ Подвійна інфраструктура (дорого)
- ❌ Database schema migrations складні (треба backward compatible)
- ❌ Stateful applications (сесії, кеш)

**Коли використовувати:**
- Mission-critical додатки
- Frequent deployments
- Коли потрібен instant rollback

---

## Slide 38: AWS реалізація Blue/Green

```yaml
Option 1: Route53 Weighted Routing
  Route53:
    blue.example.com → Weight: 0
    green.example.com → Weight: 100

Option 2: ALB Target Groups
  ALB:
    Rules:
      - Default → Blue Target Group (weight: 0)
      - Default → Green Target Group (weight: 100)

Option 3: ECS/EKS
  ECS Service:
    TaskDefinition: v1 (Blue)  → DesiredCount: 0
    TaskDefinition: v2 (Green) → DesiredCount: 10

Option 4: Elastic Beanstalk
  eb deploy --staged
  eb swap blue-env green-env
```

**Рекомендація:** ALB Target Groups (найпростіше в AWS).

---

## Slide 39: Ключові висновки

**Принципи надійних систем:**

```
1. ✓ Eliminate SPOF
   Кожен компонент має redundancy

2. ✓ Multi-AZ by default
   Мінімум 2 AZ для production

3. ✓ Load Balancing
   Розподіл навантаження + failover

4. ✓ Health Checks
   Автоматичне виявлення проблем

5. ✓ Auto Scaling
   Elastic capacity під навантаження

6. ✓ Plan for DR
   Визначити RTO/RPO → вибрати стратегію

7. ✓ Automate Everything
   Ручні процеси = human errors
```

---

## Slide 40: Питання та ресурси

**Питання для самоперевірки:**
1. Як виявити SPOF у вашій архітектурі?
2. Коли використовувати ALB vs NLB?
3. Яка різниця між /health та /ready endpoints?
4. Як вибрати між Pilot Light та Warm Standby?
5. Які умови потрібні для успішного Blue/Green deployment?

**Ресурси:**
- AWS Well-Architected Framework
- AWS Architecture Center
- "Site Reliability Engineering" (Google)
- Netflix Tech Blog
- Martin Fowler — Microservices patterns

**Наступна лекція:** Моніторинг та Observability
