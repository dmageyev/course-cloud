# Лекція 1: Хмарні обчислення як архітектурна парадигма

**Курс**: Cloud Computing  
**Рівень**: Бакалаври 3 курс, ІТ напрямок  
**Тривалість**: 90 хвилин

---

## Огляд лекції

- Еволюція ІТ-інфраструктури
- Визначення Cloud Computing (NIST)
- Моделі обслуговування: IaaS / PaaS / SaaS
- Моделі розгортання: Public / Private / Hybrid / Multi-cloud
- Cloud ≠ AWS: Vendor-Agnostic підхід
- API-First та Automation-First принципи
- AWS як приклад реалізації

---

## 1. Еволюція ІТ-інфраструктури

---

### Mainframe Era (1960-1980-ті)

**Централізовані обчислення**

- 🖥️ Величезні комп'ютери (кімнатного розміру)
- 💰 Надзвичайно висока вартість
- 🔒 Обмежений доступ
- 📊 Термінали без власної потужності
- ⏱️ Пакетна обробка даних

**Проблеми**: Висока вартість, складність масштабування, централізація

---

### Client-Server Era (1980-2000-ні)

**Розподілені обчислення**

- 💻 Персональні комп'ютери + сервери
- 🌐 Локальні мережі (LAN)
- 🏢 Власні дата-центри
- 📈 Більша гнучкість
- 💵 Нижча вартість входу

**Проблеми**: Утримання інфраструктури, капітальні витрати, складність масштабування

---

### Віртуалізація (2000-ні)

**Революція в ефективності**

- 🔄 VMware, Xen, KVM
- 📦 Множинні ОС на одному сервері
- 🎯 Ефективніше використання ресурсів
- 🔐 Ізоляція робочих навантажень
- 🌱 Основа для хмарних обчислень

**Результат**: Можливість динамічного виділення ресурсів

---

### Виклики традиційного підходу

**Капітальні витрати (CapEx)**
- 💰 Велика початкова інвестиція
- 📉 Амортизація обладнання
- ⚠️ Ризик застарівання

**Масштабування**
- ⏳ Тривалий час закупівлі (тижні/місяці)
- 🔮 Складність прогнозування
- ⚖️ Надлишкові потужності або їх нестача

**Утримання**
- 👥 Необхідність персоналу
- 🔒 Фізична безпека
- ⚡ Електроживлення, охолодження

---

## 2. Cloud Computing: Визначення NIST

---

### Офіційне визначення

> **Cloud computing** is a model for enabling ubiquitous, convenient, **on-demand network access** to a **shared pool of configurable computing resources** that can be **rapidly provisioned and released** with minimal management effort.

**— National Institute of Standards and Technology (NIST)**

---

### П'ять основних характеристик

1. **On-demand self-service** (Самообслуговування)
2. **Broad network access** (Широкий мережевий доступ)
3. **Resource pooling** (Об'єднання ресурсів)
4. **Rapid elasticity** (Швидка еластичність)
5. **Measured service** (Вимірюваність)

**Усі п'ять обов'язкові для "справжньої" хмари!**

---

### 1. On-demand Self-Service

**Самообслуговування на вимогу**

- 🖱️ Користувач сам отримує ресурси
- 🤖 Автоматизація через веб-портал або API
- ⚡ Без взаємодії з персоналом провайдера
- 🚀 Миттєвий старт

**Приклад AWS**: Запуск EC2 instance за 2 хвилини через консоль або за секунди через CLI

```bash
aws ec2 run-instances --image-id ami-xxx --instance-type t2.micro
```

---

### 2. Broad Network Access

**Широкий мережевий доступ**

- 🌐 Доступ через стандартні протоколи (HTTP/HTTPS)
- 📱 З різних пристроїв (ноутбук, телефон, планшет)
- 🔓 Через інтернет або приватні мережі
- 🌍 Звідки завгодно

**Приклад**: Доступ до AWS Console з браузера, мобільного додатку, CLI

---

### 3. Resource Pooling

**Об'єднання ресурсів**

- 👥 Мультитенантність (multi-tenancy)
- 🔄 Динамічне призначення ресурсів
- 🗺️ Абстрагування від фізичного розташування
- 📊 Ефективне використання

**Концепція**: Ваша EC2 instance працює на спільному обладнанні з іншими, але ви цього не помічаєте

---

### 4. Rapid Elasticity

**Швидка еластичність**

- 📈 Масштабування вгору (scale up)
- 📉 Масштабування вниз (scale down)
- 🤖 Автоматичне або ручне
- ⚡ За лічені хвилини

**Приклад AWS**: Auto Scaling Groups автоматично додають EC2 instances при високому навантаженні

```
Низьке навантаження: 2 instances
Високе навантаження: 20 instances
Повернення до норми: 2 instances
```

---

### 5. Measured Service

**Вимірюваність сервісу**

- 📊 Моніторинг використання ресурсів
- 💵 Оплата за фактичне використання (pay-as-you-go)
- 📈 Прозорість витрат
- 🔍 Детальна аналітика

**Приклад AWS**: 
- CloudWatch metrics для моніторингу
- Cost Explorer для аналізу витрат
- Billing dashboard з розбивкою по сервісах

---

## 3. Моделі обслуговування

### IaaS / PaaS / SaaS

---

### Піраміда моделей обслуговування

```
        +------------------+
        |   SaaS           |  Додатки як сервіс
        +------------------+
        |   PaaS           |  Платформа як сервіс
        +------------------+
        |   IaaS           |  Інфраструктура як сервіс
        +------------------+
        | On-Premises      |  Власна інфраструктура
        +------------------+

      Менше контролю ↑      ↑ Менше управління
      Більше контролю ↓     ↓ Більше управління
```

---

### IaaS: Infrastructure as a Service

**Що це**: Оренда віртуалізованих обчислювальних ресурсів

**Що надається**:
- ☁️ Віртуальні машини
- 💾 Сховища даних
- 🌐 Мережа
- 🖥️ Операційні системи (на вибір)

**Ваша відповідальність**:
- ⚙️ ОС та патчі
- 📦 Middleware, runtime
- 💻 Додатки та дані

---

### IaaS: Приклади та використання

**Приклади**:
- **AWS**: EC2, S3, VPC
- **Azure**: Virtual Machines, Blob Storage
- **GCP**: Compute Engine, Cloud Storage

**Коли використовувати**:
- ✅ Потрібен повний контроль над ОС
- ✅ Міграція legacy додатків
- ✅ Специфічні вимоги до конфігурації
- ✅ Lift-and-shift міграція

**Переваги**: Максимальна гнучкість  
**Недоліки**: Більше управління

---

### PaaS: Platform as a Service

**Що це**: Платформа для розробки та розгортання додатків

**Що надається**:
- 🔧 Runtime середовище
- 📚 Middleware
- 🗄️ Бази даних
- 🛠️ Інструменти розробки
- 🖥️ ОС (керована провайдером)

**Ваша відповідальність**:
- 💻 Код додатку
- ⚙️ Конфігурація додатку
- 📊 Дані

---

### PaaS: Приклади та використання

**Приклади**:
- **AWS**: Elastic Beanstalk, Lambda, RDS
- **Azure**: App Service, Azure Functions
- **GCP**: App Engine, Cloud Functions
- **Інше**: Heroku, Cloud Foundry

**Коли використовувати**:
- ✅ Фокус на коді, не на інфраструктурі
- ✅ Потрібне швидке розгортання
- ✅ Стандартні веб-додатки
- ✅ Автоматичне масштабування

**Переваги**: Швидка розробка  
**Недоліки**: Менша гнучкість

---

### SaaS: Software as a Service

**Що це**: Готові додатки через інтернет

**Що надається**:
- 📱 Повністю готовий додаток
- 🖥️ Інтерфейс користувача
- 🔒 Безпека та оновлення
- 📊 Все необхідне

**Ваша відповідальність**:
- 👤 Використання додатку
- 📄 Ваші дані
- ⚙️ Конфігурація (обмежена)

---

### SaaS: Приклади

**Популярні SaaS додатки**:
- 📧 **Email**: Gmail, Outlook.com
- 💼 **Productivity**: Microsoft 365, Google Workspace
- 💰 **CRM**: Salesforce, HubSpot
- 💬 **Communication**: Slack, Zoom, Teams
- 📁 **Storage**: Dropbox, Google Drive

**Переваги**: Немає управління інфраструктурою  
**Недоліки**: Мінімальна кастомізація

---

### Порівняння відповідальності

| Компонент      | On-Prem | IaaS | PaaS | SaaS |
|----------------|---------|------|------|------|
| Додатки        | 👤 Ви   | 👤 Ви | 👤 Ви | ☁️ Провайдер |
| Дані           | 👤 Ви   | 👤 Ви | 👤 Ви | ☁️ Провайдер |
| Runtime        | 👤 Ви   | 👤 Ви | ☁️ Провайдер | ☁️ Провайдер |
| Middleware     | 👤 Ви   | 👤 Ви | ☁️ Провайдер | ☁️ Провайдер |
| ОС             | 👤 Ви   | 👤 Ви | ☁️ Провайдер | ☁️ Провайдер |
| Віртуалізація  | 👤 Ви   | ☁️ Провайдер | ☁️ Провайдер | ☁️ Провайдер |
| Сервери        | 👤 Ви   | ☁️ Провайдер | ☁️ Провайдер | ☁️ Провайдер |
| Мережа         | 👤 Ви   | ☁️ Провайдер | ☁️ Провайдер | ☁️ Провайдер |

---

## 4. Моделі розгортання

---

### Типи хмар

```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   Public    │  │   Private   │  │   Hybrid    │  │ Multi-Cloud │
│    Cloud    │  │    Cloud    │  │    Cloud    │  │             │
└─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘
     AWS              On-Prem       AWS + On-Prem   AWS + Azure
     Azure            VMware        Azure + On-Prem     + GCP
     GCP              OpenStack
```

---

### Public Cloud (Публічна хмара)

**Що це**: Хмара доступна для загального користування

**Характеристики**:
- 🌐 Спільна інфраструктура (multi-tenancy)
- 💵 Оплата за використання
- 🚀 Немає початкових інвестицій
- ☁️ Провайдер керує всім

**Приклади**: AWS, Azure, GCP, DigitalOcean

**Переваги**:
- ✅ Низькі початкові витрати
- ✅ Швидке масштабування
- ✅ Глобальна доступність

**Недоліки**:
- ❌ Менший контроль
- ❌ Regulatory concerns

---

### Private Cloud (Приватна хмара)

**Що це**: Виділена інфраструктура для однієї організації

**Характеристики**:
- 🔒 Виділена інфраструктура
- 🏢 On-premises або hosted
- 👥 Повний контроль
- ⚙️ Власне управління

**Приклади**: VMware vCloud, OpenStack, AWS Outposts

**Переваги**:
- ✅ Повний контроль
- ✅ Compliance (відповідність вимогам)
- ✅ Кращий контроль безпеки

**Недоліки**:
- ❌ Високі початкові витрати
- ❌ Потреба в персоналі

---

### Hybrid Cloud (Гібридна хмара)

**Що це**: Комбінація приватної та публічної хмар

**Характеристики**:
- 🔄 Дані переміщуються між хмарами
- 🎯 Єдине управління
- ⚖️ Гнучкість розміщення
- 🔗 Інтеграція on-prem та cloud

**Use Cases**:
- 💾 Sensitive data → Private
- 🌐 Public apps → Public
- 📈 Burst to cloud (CloudBursting)
- 🔄 Disaster recovery

**Технології**: VPN, Direct Connect, Azure Arc, AWS Outposts

---

### Multi-Cloud (Мультихмара)

**Що це**: Використання сервісів від кількох провайдерів

**Характеристики**:
- ☁️☁️☁️ AWS + Azure + GCP одночасно
- 🔓 Уникнення vendor lock-in
- 🎯 Best-of-breed підхід
- 🌍 Географічне покриття

**Стратегії**:
- 📦 Distributed: різні додатки
- 🔄 Redundant: дублювання
- 🎯 Specialized: унікальні сервіси

**Переваги**: Гнучкість, надійність  
**Недоліки**: Складність управління

---

### Порівняння моделей розгортання

|                | Public | Private | Hybrid | Multi-Cloud |
|----------------|--------|---------|--------|-------------|
| **Контроль**   | Низький | Високий | Середній | Середній |
| **Безпека**    | Стандарт | Макс | Висока | Висока |
| **Вартість**   | Низька | Висока | Середня | Висока |
| **Складність** | Низька | Середня | Висока | Найвища |
| **Гнучкість**  | Висока | Середня | Висока | Найвища |

**Вибір залежить**: від вимог бізнесу, security, compliance, бюджету

---

## 5. Cloud ≠ AWS

### Vendor-Agnostic підхід

---

### Поширена помилка

❌ **Хибне уявлення**: 

```
"Cloud" = "AWS"
```

✅ **Реальність**:

```
Cloud = Архітектурна парадигма
AWS = Один з багатьох провайдерів
```

**Це як сказати: "Автомобіль = Tesla"**

---

### Чому це важливо?

**1. Технологічна грамотність**
- 📚 Розуміння концепцій vs. реалізацій
- 🔄 Portable знання
- 🎓 Deeper understanding

**2. Архітектурне мислення**
- 🏗️ Vendor-agnostic дизайн
- 🔓 Уникнення lock-in
- 🌍 Flexibility

**3. Кар'єрна гнучкість**
- 💼 Робота з різними платформами
- 📈 Адаптивність до ринку
- 🌟 Ширші можливості

---

### Основні хмарні провайдери

| Провайдер | Запуск | Частка ринку | Сильні сторони |
|-----------|--------|--------------|----------------|
| **AWS** | 2006 | ~32% | Найширший набір сервісів |
| **Azure** | 2010 | ~23% | Microsoft інтеграція, hybrid |
| **GCP** | 2011 | ~10% | Big Data, ML/AI, Kubernetes |
| **Alibaba** | 2009 | ~4% | Домінування в Азії |
| **Oracle** | 2016 | ~2% | Enterprise, databases |

**Інші**: IBM Cloud, DigitalOcean, Linode, Vultr...

---

### Vendor-Agnostic архітектура

**Принципи**:

**1. Використання стандартів**
- 🐳 Kubernetes замість ECS/AKS/GKE
- 🗄️ PostgreSQL замість Aurora/CosmosDB
- 📊 OpenTelemetry для спостережуваності

**2. Абстракція специфічних сервісів**
- 📦 Wrapper для cloud storage
- 📬 Абстракція черг повідомлень
- 🗄️ Database abstraction layers

**3. Infrastructure as Code**
- 🔧 Terraform (мульти-провайдер)
- 🚀 Pulumi, Crossplane

---

### Концептуальний маппінг сервісів

| Концепція | AWS | Azure | GCP |
|-----------|-----|-------|-----|
| VM | EC2 | Virtual Machines | Compute Engine |
| Object Storage | S3 | Blob Storage | Cloud Storage |
| Serverless | Lambda | Functions | Cloud Functions |
| Kubernetes | EKS | AKS | GKE |
| SQL DB | RDS | SQL Database | Cloud SQL |
| NoSQL | DynamoDB | Cosmos DB | Firestore |
| Load Balancer | ALB | Load Balancer | Cloud LB |

**Концепції однакові, реалізації різні!**

---

### Коли використовувати специфічні сервіси?

**За vendor-agnostic** ✅:
- Довгострокові проекти
- Можливість зміни провайдера
- Multi-cloud стратегія

**За специфічні сервіси** ✅:
- Унікальні можливості (SageMaker для ML)
- Значне покращення productivity
- Cost optimization
- Performance requirements

**Баланс між гнучкістю та ефективністю!**

---

## 6. API-First & Automation-First

---

### API-First підхід

**Принцип**: Всі операції виконуються через API

```
Web Console → API
CLI → API
SDK → API
Terraform → API
```

**Web Console - це лише UI над API!**

---

### Чому API-First?

**Переваги**:

✅ **Автоматизація**
- Scripting операцій
- CI/CD інтеграція
- Infrastructure as Code

✅ **Повторюваність**
- Однакові результати
- Документування через код

✅ **Версіонування**
- Git для інфраструктури
- Rollback можливості

✅ **Інтеграція**
- З іншими системами
- Custom dashboards

---

### Способи взаємодії з Cloud

**1. Web Console (UI)** 🖱️
- Візуальний інтерфейс
- Для learning та exploration
- Не для production!

**2. CLI (Command Line)** 💻
```bash
aws ec2 run-instances --image-id ami-xxx
az vm create --resource-group myRG
gcloud compute instances create my-instance
```

**3. SDK (Software Development Kit)** 🔧
```python
import boto3
ec2 = boto3.resource('ec2')
instance = ec2.create_instances(...)
```

**4. Infrastructure as Code** 📄
```hcl
resource "aws_instance" "web" {
  ami = "ami-xxx"
  instance_type = "t2.micro"
}
```

---

### Automation-First підхід

**Принцип**: Якщо робиш більше 1 разу → автоматизуй!

```
Manual → Script → IaC → GitOps → Self-Healing
  🐌      🏃       🚀     ✈️      🤖
```

---

### Чому автоматизація?

**1. Швидкість** ⚡
- Миттєве виконання
- Паралельне виконання

**2. Надійність** ✅
- Без людських помилок
- Консистентність

**3. Масштабованість** 📈
- 1 ресурс = 1000 ресурсів
- Однакова складність

**4. Документація** 📚
- Код як документація
- Self-documenting

**5. Compliance** 🔒
- Автоматичні перевірки
- Audit trail

---

### Рівні автоматизації

```
Рівень 5: Self-Healing        🤖 Повна автоматизація
            ↑
Рівень 4: GitOps               📊 Git як джерело істини
            ↑
Рівень 3: Infrastructure       🏗️ Декларативний код
          as Code              
            ↑
Рівень 2: Scripts              📝 CLI автоматизація
            ↑
Рівень 1: Manual               🖱️ Веб-консоль
```

**Мета: рухатися вгору!**

---

### Best Practices автоматизації

**1. Version Control усього** 📚
```
infrastructure/
├── terraform/
├── ansible/
└── scripts/
```

**2. Idempotency** 🔄
- Повторне виконання = той самий результат
- Безпечні re-runs

**3. Testing** ✅
- Unit тести для IaC
- Integration тести
- Compliance тести

**4. Модульність** 📦
- Reusable модулі
- DRY principle

---

## 7. AWS як приклад

---

### Чому AWS як приклад?

**1. Лідер ринку** 📊
- Найбільша частка (~32%)
- Найстаріший провайдер (2006)

**2. Найширший набір** 🎯
- 200+ сервісів
- Практично для всього

**3. Well-Architected** 📖
- Документовані best practices
- Proven patterns

**4. Навчальні ресурси** 🎓
- Величезна екосистема
- Community

**⚠️ AWS - це ПРИКЛАД, не єдине рішення!**

---

### Глобальна інфраструктура AWS

```
        🌍 AWS Global Infrastructure
                    │
        ┌───────────┴───────────┐
        │                       │
    Regions                Edge Locations
    (30+)                     (400+)
        │                       │
  Availability Zones         CloudFront
    (90+)                      CDN
```

---

### Regions (Регіони)

**Що це**: Географічна область з множинними дата-центрами

**Приклади**:
- 🇺🇸 us-east-1 (N. Virginia)
- 🇮🇪 eu-west-1 (Ireland)
- 🇸🇬 ap-southeast-1 (Singapore)
- 🇩🇪 eu-central-1 (Frankfurt)

**Вибір регіону**:
- 📍 **Latency**: близькість до користувачів
- 📋 **Compliance**: законодавчі вимоги
- 🎯 **Availability**: доступність сервісів
- 💰 **Cost**: різні ціни в різних регіонах

---

### Availability Zones (AZ)

**Що це**: Ізольовані дата-центри в межах регіону

```
Region: eu-west-1 (Ireland)
├── AZ: eu-west-1a  🏢
├── AZ: eu-west-1b  🏢
└── AZ: eu-west-1c  🏢
     │
     └─ High-speed private network
```

**Характеристики**:
- Фізично розділені (різні будівлі)
- Незалежне електроживлення
- Швидкий зв'язок між AZ

**Best Practice**: Deploy в множинні AZ для high availability

---

### AWS Free Tier 🎁

**Мета**: Безкоштовне ознайомлення з AWS

**Типи**:

**1. 12 Months Free** (перший рік)
- EC2: 750 год/міс (t2.micro)
- S3: 5 GB storage
- RDS: 750 год/міс (db.t2.micro)

**2. Always Free** (завжди)
- Lambda: 1M запитів/міс
- DynamoDB: 25 GB storage
- SNS: 1000 публікацій/міс

**3. Trials** (короткі періоди)
- Специфічні сервіси

---

### ⚠️ Важливо про Free Tier

**Обережно**:
- 💸 Легко перевищити ліміти
- 📊 **Обов'язково**: Billing Alerts!
- 🔍 Regularly перевіряйте витрати
- 🗑️ Видаляйте unused resources

**Setup Billing Alert**:
```bash
aws budgets create-budget \
  --budget-name "Monthly-Budget" \
  --budget-limit Amount=10,Unit=USD
```

**Не залишайте EC2 instances running!**

---

### Основні AWS сервіси

**Compute** 💻
- **EC2**: Virtual machines
- **Lambda**: Serverless functions
- **ECS/EKS**: Containers

**Storage** 💾
- **S3**: Object storage
- **EBS**: Block storage
- **EFS**: File storage

**Database** 🗄️
- **RDS**: Managed SQL
- **DynamoDB**: NoSQL

**Networking** 🌐
- **VPC**: Virtual network
- **Route 53**: DNS
- **CloudFront**: CDN

---

### AWS Well-Architected Framework

**6 Pillars (Стовпів)**:

1. **⚙️ Operational Excellence**
   - Автоматизація, моніторинг

2. **🔒 Security**
   - IAM, encryption, detective controls

3. **✅ Reliability**
   - High availability, disaster recovery

4. **⚡ Performance Efficiency**
   - Right-sizing, caching

5. **💰 Cost Optimization**
   - Pay for what you use

6. **🌱 Sustainability**
   - Environmental impact

---

## 8. Практичні поради

---

### Початок роботи з Cloud

**1. Навчання** 📚
- Free tier акаунт
- Online курси (AWS Skill Builder, Azure Learn)
- Hands-on practice
- Документація

**2. Безпека** 🔒
- **MFA на всіх акаунтах!**
- Never use root account
- Least privilege principle
- **Never commit credentials!**

**3. Контроль витрат** 💰
- Billing alerts
- Budget limits
- Видаляйте unused resources
- Use tags для tracking

---

### Типові помилки початківців

❌ **Залишені ресурси**
- Забуті EC2 instances → $$$
- Невикористані volumes
- Orphaned snapshots

❌ **Слабка безпека**
- Публічні credentials в Git
- Відкриті security groups (0.0.0.0/0)
- No MFA

❌ **Відсутність моніторингу**
- Немає alerting
- Blindspot в системі

❌ **No disaster recovery plan**
- No backups
- Single AZ deployment

---

### Best Practices checklist

✅ **Security First**
- [ ] MFA enabled
- [ ] IAM roles замість ключів
- [ ] Encryption at rest та in transit
- [ ] Regular security audits

✅ **Cost Management**
- [ ] Billing alerts configured
- [ ] Tags на всіх ресурсах
- [ ] Regular cleanup
- [ ] Right-sizing instances

✅ **Reliability**
- [ ] Multi-AZ deployment
- [ ] Automated backups
- [ ] Disaster recovery plan tested

---

## 9. Висновки

---

### Ключові висновки

**1. Cloud = Архітектурна парадигма** ☁️
- Не просто "чужі комп'ютери"
- 5 характеристик NIST

**2. Моделі обслуговування** 🎯
- IaaS → PaaS → SaaS
- Різні рівні абстракції

**3. Моделі розгортання** 🌍
- Public / Private / Hybrid / Multi-cloud
- Вибір залежить від вимог

**4. Vendor-Agnostic мислення** 🔓
- Cloud ≠ AWS
- Концепції > Реалізації

---

### Ключові висновки (продовження)

**5. API-First + Automation-First** 🤖
- Основа сучасних операцій
- Infrastructure as Code

**6. AWS як приклад** 📚
- Не єдине рішення
- Демонстрація принципів

**7. Практика** 💪
- Free Tier для навчання
- Безпека і контроль витрат
- Continuous learning

---

### Наступні кроки

**1. Створіть AWS акаунт** (Free Tier) 🎁

**2. Перші експерименти** 🧪
- Запустіть EC2 instance
- Створіть S3 bucket
- Познайомтесь з Console

**3. Освоюйте CLI** 💻
- AWS CLI встановлення
- Базові команди
- Scripting

**4. Вивчайте IaC** 🏗️
- Terraform basics
- CloudFormation
- Version control

---

### Питання для самоперевірки

1. Які 5 характеристик cloud за NIST?
2. В чому різниця IaaS vs PaaS vs SaaS?
3. Коли використовувати hybrid cloud?
4. Чому важливо vendor-agnostic мислення?
5. Що таке API-first підхід?
6. Навіщо автоматизація в cloud?
7. Що таке Availability Zone?
8. Які типи AWS Free Tier?
9. Назвіть 6 pillars Well-Architected Framework
10. Які перші кроки для безпеки в cloud?

---

### Ресурси для подальшого вивчення

📚 **Офіційна документація**:
- AWS Documentation
- Azure Documentation
- GCP Documentation

🎓 **Безкоштовні курси**:
- AWS Skill Builder
- Microsoft Learn
- Google Cloud Skills Boost

📖 **Книги**:
- Cloud Native Patterns
- Architecting the Cloud

🔗 **Детальніше**: Дивіться `references.md`

---

## Дякую за увагу!

**Питання?** 🙋

---

**Наступна лекція**: Compute сервіси та віртуалізація

**Домашнє завдання**: Створити AWS Free Tier акаунт та запустити першу EC2 instance

**Контакти**: [інформація курсу]

---

**Версія**: 1.0  
**Дата**: 2026-02-11
