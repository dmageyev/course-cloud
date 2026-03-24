# Лекція 1: Хмарні обчислення як архітектурна парадигма — Слайди

---

## Slide 1: Назва

# Лекція 1: Хмарні обчислення як архітектурна парадигма

---

## Slide 2: Проблема on-premises

```
Традиційний підхід:
  Бізнес-запит → Procurement → Поставка → Налаштування → Deploy
        └─────────────── 3–6 місяців ──────────────────┘

Хмарний підхід:
  Бізнес-запит → API call → Running instance
        └──────────── 60 секунд ────────────┘
```

---

## Slide 3: Визначення NIST

П'ять характеристик хмари:
1. On-demand self-service
2. Broad network access
3. Resource pooling
4. Rapid elasticity
5. Measured service

---

## Slide 4: IaaS / PaaS / SaaS

| Модель | Ви управляєте | AWS приклад |
|--------|--------------|-------------|
| IaaS | ОС + App | EC2 |
| PaaS | App | RDS, Beanstalk |
| SaaS | Дані | SES, Cognito |

---

## Slide 5: Public / Private / Hybrid / Multi-cloud

- Public: AWS, GCP, Azure
- Private: VMware, OpenStack
- Hybrid: AWS Direct Connect + on-premises
- Multi-cloud: AWS + GCP разом

---

## Slide 6: Cloud ≠ AWS

AWS — ~31% ринку. Є ще:
- GCP (Google): ML/AI лідер
- Azure (Microsoft): enterprise
- Alibaba: Азія

→ Vendor-agnostic через Terraform, Kubernetes

---

## Slide 7: API-first підхід

```bash
# Будь-яка дія = API call
aws ec2 run-instances \
  --image-id ami-0abcdef \
  --instance-type t3.micro \
  --count 1
```

→ Інфраструктура як код = IaC

---

## Slide 8: AWS Free Tier

- t2.micro/t3.micro: 750 год/місяць
- S3: 5 GB сховище
- Lambda: 1M викликів
- RDS: 750 год db.t2.micro

→ Достатньо для навчання та особистих проектів

---

## Slide 9: Ключові висновки

1. Хмара = API over compute resources
2. IaaS/PaaS/SaaS — різний рівень абстракції
3. Автоматизація — обов'язкова умова
4. AWS — найбільший, але vendor-agnostic підхід важливий
