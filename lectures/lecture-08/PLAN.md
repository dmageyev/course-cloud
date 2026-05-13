# PLAN.md — Лекція 8: Масштабування та відмовостійкість

**Курс:** Хмарні технології інформаційних систем
**Аудиторія:** Бакалавр ІТ · 3 курс
**Тривалість:** 2 академічні години (90 хв)

---

## Мета лекції

Студенти повинні вміти:
- Ідентифікувати та усувати Single Points of Failure
- Застосовувати паттерни балансування навантаження
- Налаштовувати health checks для моніторингу здоров'я сервісів
- Проектувати Auto Scaling стратегії
- Розробляти DR (Disaster Recovery) плани
- Розуміти Blue/Green deployment та безпечне розгортання

---

## Структура лекції (40 слайдів)

### Блок 1 — Вступ (слайди 1–3) · ~5 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 1 | Титульний | Назва, курс, аудиторія |
| 2 | Agenda | Огляд тем лекції |
| 3 | Чому важливо? | Реальні інциденти: GitHub 2018, AWS S3 2017, статистика downtime |

### Блок 2 — Single Point of Failure (слайди 4–8) · ~15 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 4 | SPOF — визначення | Що таке Single Point of Failure, наслідки |
| 5 | Типові SPOF | Сервер, БД, Load Balancer, DNS, мережа, зона доступності |
| 6 | Виявлення SPOF | Аналіз архітектури: питання для перевірки, fault tree analysis |
| 7 | Усунення SPOF | Redundancy, реплікація, multi-AZ/region, резервування |
| 8 | Приклад: від SPOF до HA | Еволюція архітектури: Single → Multi-AZ → Multi-Region |

### Блок 3 — Load Balancing (слайди 9–15) · ~20 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 9 | Load Balancing — навіщо? | Розподіл навантаження, відмовостійкість, масштабування |
| 10 | Типи Load Balancers | L4 (TCP/UDP), L7 (HTTP/HTTPS), порівняння |
| 11 | Алгоритми балансування | Round Robin, Least Connections, IP Hash, Weighted |
| 12 | Session Affinity | Sticky sessions, проблеми, коли потрібні |
| 13 | AWS Application LB | Функції: path-based routing, host-based, target groups |
| 14 | AWS Network LB | Ultra-low latency, мільйони req/sec, static IP |
| 15 | Паттерни розгортання | Single LB, Multi-AZ LB, Global Load Balancing |

### Блок 4 — Health Checks (слайди 16–20) · ~12 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 16 | Health Checks — що це? | Перевірка доступності та працездатності сервісів |
| 17 | Типи Health Checks | Shallow (ping), Deep (DB connection), Dependency checks |
| 18 | Параметри HC | Interval, timeout, healthy/unhealthy thresholds |
| 19 | Health Check endpoints | GET /health, GET /ready, GET /live (Kubernetes style) |
| 20 | AWS Health Checks | ELB HC, ASG HC, Route53 HC, CloudWatch integration |

### Блок 5 — Auto Scaling (слайди 21–28) · ~20 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 21 | Auto Scaling — концепція | Автоматичне масштабування під навантаження |
| 22 | Вертикальне vs Горизонтальне | Scale Up/Down vs Scale Out/In, порівняння |
| 23 | Метрики масштабування | CPU, Memory, Network, Custom (queue depth, latency) |
| 24 | Scaling Policies | Target Tracking, Step Scaling, Scheduled, Predictive |
| 25 | Cooldown Period | Чому важливий, уникнення flapping |
| 26 | AWS Auto Scaling Groups | Launch Template, Min/Max/Desired, AZ balancing |
| 27 | Приклад ASG політики | CPU > 70% → +2 instances, CPU < 30% → -1 instance |
| 28 | Best Practices ASG | Stateless додатки, graceful shutdown, warm-up time |

### Блок 6 — Disaster Recovery (слайди 29–34) · ~15 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 29 | DR — визначення | Відновлення після катастрофи, RTO, RPO |
| 30 | RTO vs RPO | Recovery Time Objective, Recovery Point Objective, приклади |
| 31 | DR стратегії — огляд | Backup & Restore, Pilot Light, Warm Standby, Multi-Site |
| 32 | Backup & Restore | Найдешевше, RTO/RPO = години, snapshot, S3 backup |
| 33 | Pilot Light / Warm Standby | Мінімальна/часткова інфра в standby, RTO = хвилини |
| 34 | Multi-Site (Hot Standby) | Активна репліка, RTO/RPO ≈ 0, найдорожче |

### Блок 7 — Blue/Green Deployment (слайди 35–38) · ~10 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 35 | Blue/Green — концепція | Два ідентичні середовища, швидкий rollback |
| 36 | Процес Blue/Green | Deploy to Green → Test → Switch traffic → Keep Blue standby |
| 37 | Переваги та недоліки | Zero downtime, instant rollback vs подвійна інфра |
| 38 | AWS реалізація | Route53 weighted routing, ALB target groups, ECS/EKS |

### Блок 8 — Підсумок (слайди 39–40) · ~3 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 39 | Ключові висновки | Чеклист знань, принципи проектування |
| 40 | Питання та ресурси | Q&A, посилання для поглибленого вивчення |

---

## Ключові поняття

| Термін | Визначення |
|--------|-----------|
| SPOF | Single Point of Failure — компонент, відмова якого зупиняє всю систему |
| HA | High Availability — висока доступність системи |
| Load Balancer | Розподільник навантаження між серверами |
| Health Check | Перевірка стану здоров'я сервісу |
| Auto Scaling | Автоматичне масштабування ресурсів |
| Scale Out/In | Горизонтальне масштабування (додавання/видалення інстансів) |
| Scale Up/Down | Вертикальне масштабування (збільшення/зменшення потужності) |
| DR | Disaster Recovery — відновлення після катастрофи |
| RTO | Recovery Time Objective — максимальний допустимий час відновлення |
| RPO | Recovery Point Objective — максимальна допустима втрата даних |
| Blue/Green | Стратегія розгортання з двома паралельними середовищами |
| ALB | Application Load Balancer — L7 балансувальник AWS |
| ASG | Auto Scaling Group — група автомасштабування AWS |

---

## Рекомендована підготовка

- Пригадати основи мережевих протоколів (TCP, HTTP)
- Переглянути: [AWS Well-Architected Framework — Reliability Pillar](https://aws.amazon.com/architecture/well-architected/)
- Прочитати: [The Twelve-Factor App — Processes & Disposability](https://12factor.net/)

---

## Посилання

- AWS Elastic Load Balancing: <https://docs.aws.amazon.com/elasticloadbalancing/>
- AWS Auto Scaling: <https://docs.aws.amazon.com/autoscaling/>
- AWS Well-Architected — Reliability: <https://wa.aws.amazon.com/wat.pillar.reliability.en.html>
- "Site Reliability Engineering" (книга від Google)
- "Release It!" by Michael T. Nygard
- Netflix Tech Blog: <https://netflixtechblog.com/>
