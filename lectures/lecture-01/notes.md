# Лекція 1: Хмарні обчислення як архітектурна парадигма — Конспект

## 1. Еволюція ІТ-інфраструктури

**On-premises** (до 2000-х): власні сервери, довгий procurement, велика капітальна інвестиція (CapEx).

**Colocation**: власне обладнання в чужому датацентрі. Менше OPEX, але ті ж проблеми з procurement.

**Хмарні обчислення**: оренда ресурсів через API, оплата за використання (OPEX), необмежена масштабованість.

---

## 2. Визначення NIST (SP 800-145)

Хмарні обчислення — модель для зручного, on-demand мережевого доступу до конфігурованого пулу обчислювальних ресурсів (мережі, сервери, сховище, застосунки, сервіси), що може бути швидко виділено та звільнено з мінімальними зусиллями управління.

**П'ять характеристик**:
1. On-demand self-service
2. Broad network access
3. Resource pooling
4. Rapid elasticity
5. Measured service

---

## 3. Моделі обслуговування

| Модель | Ви управляєте | Провайдер управляє | Приклад AWS |
|--------|--------------|---------------------|-------------|
| IaaS | ОС, runtime, додатки | Hardware, virtualization | EC2, VPC |
| PaaS | Додатки, дані | ОС, runtime, hardware | Elastic Beanstalk, RDS |
| SaaS | Тільки дані | Все | Gmail, Salesforce |

---

## 4. Моделі розгортання

- **Public Cloud**: AWS, GCP, Azure — ресурси у провайдера
- **Private Cloud**: VMware, OpenStack — у власному датацентрі
- **Hybrid Cloud**: комбінація public + private з інтеграцією
- **Multi-cloud**: кілька public провайдерів паралельно

---

## 5. Cloud ≠ AWS

AWS — найбільший провайдер (~31% ринку 2024), але не єдиний:
- **GCP** (Google): AI/ML, BigQuery, Kubernetes (створили Kubernetes)
- **Azure** (Microsoft): enterprise, AD інтеграція
- **Alibaba Cloud**: лідер в Азії
- **Oracle Cloud**: бази даних

Vendor-agnostic підхід: використовуй абстракції (Terraform, Kubernetes), де можливо.

---

## 6. API-first, Automation-first

Хмара — це API. Кожна дія (запуск VM, створення БД) доступна через HTTP API.

**Наслідки**:
- Інфраструктура як код (IaC)
- CI/CD для infrastructure
- GitOps підхід
- Everything-as-a-service

---

## 7. AWS як реалізація принципів

- **Регіони**: 33+ регіони у світі (eu-central-1, us-east-1...)
- **Availability Zones**: 2–6 AZ на регіон, незалежні датацентри
- **Free Tier**: 12 місяців безкоштовного навчання (t2.micro, 5GB S3...)
- **Global Infrastructure**: CloudFront, Route 53
