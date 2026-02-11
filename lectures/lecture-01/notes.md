# Лекція 1: Хмарні обчислення як архітектурна парадигма

## Загальна інформація
- **Тривалість**: 2 години (90 хвилин)
- **Рівень**: Бакалаври 3 курс, ІТ напрямок
- **Мета**: Ознайомити студентів з хмарними обчисленнями як фундаментальною архітектурною парадигмою сучасних інформаційних систем

## Очікувані результати навчання

Після вивчення цієї лекції студенти зможуть:
1. Пояснити еволюцію ІТ-інфраструктури до хмарних обчислень
2. Визначити основні моделі обслуговування та розгортання
3. Розрізняти хмарні платформи та розуміти принципи vendor-agnostic підходу
4. Застосовувати API-first та automation-first мислення

---

## 1. Еволюція ІТ-інфраструктури (15 хвилин)

### 1.1 Традиційна інфраструктура (1960-1990-ті)

**Mainframe Era (Епоха мейнфреймів)**
- Централізовані обчислення
- Висока вартість обладнання
- Складність масштабування
- Термінали без власної обчислювальної потужності

**Client-Server Era (Епоха клієнт-сервер, 1980-1990-ті)**
- Розподілені обчислення
- Персональні комп'ютери та сервери
- Локальні мережі (LAN)
- Власні дата-центри

### 1.2 Перехід до сучасної інфраструктури (2000-ні)

**Віртуалізація**
- VMware, Xen, KVM
- Ефективніше використання ресурсів
- Ізоляція робочих навантажень
- Основа для хмарних обчислень

**Data Center Consolidation (Консолідація дата-центрів)**
- Зменшення кількості фізичних серверів
- Економія на енергії та охолодженні
- Централізоване управління

### 1.3 Виклики традиційного підходу

1. **Капітальні витрати (CapEx)**
   - Велика початкова інвестиція
   - Амортизація обладнання
   - Ризик простою обладнання

2. **Масштабування**
   - Тривалий час закупівлі та встановлення
   - Складність прогнозування навантаження
   - Надлишкові потужності або їх нестача

3. **Утримання інфраструктури**
   - Необхідність персоналу
   - Фізична безпека
   - Електроживлення, охолодження, мережа

4. **Відновлення після катастроф**
   - Складність та висока вартість
   - Географічна прив'язка

---

## 2. Визначення Cloud Computing за NIST (20 хвилин)

### 2.1 Офіційне визначення NIST

**National Institute of Standards and Technology (NIST) Definition of Cloud Computing**:

> "Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction."

**Переклад**: Хмарні обчислення - це модель для забезпечення повсюдного, зручного, on-demand мережевого доступу до спільного пулу конфігурованих обчислювальних ресурсів (наприклад, мереж, серверів, сховищ, додатків та сервісів), які можуть бути швидко надані та звільнені з мінімальними зусиллями управління або взаємодії з постачальником послуг.

### 2.2 П'ять основних характеристик

#### 2.2.1 On-demand self-service (Самообслуговування на вимогу)
- Користувач може самостійно отримати ресурси
- Без взаємодії з персоналом провайдера
- Автоматизація через веб-портал або API
- **Приклад**: Запуск EC2 instance через AWS Console або CLI

#### 2.2.2 Broad network access (Широкий мережевий доступ)
- Доступ через стандартні мережеві механізми
- З різних пристроїв (ноутбук, телефон, планшет)
- Через інтернет або приватні мережі
- **Приклад**: Доступ до AWS з браузера, мобільного додатку, CLI

#### 2.2.3 Resource pooling (Об'єднання ресурсів)
- Мультитенантність (multi-tenancy)
- Динамічне призначення ресурсів
- Абстрагування від фізичного розташування
- **Приклад**: EC2 instances працюють на спільному обладнанні AWS

#### 2.2.4 Rapid elasticity (Швидка еластичність)
- Автоматичне масштабування вгору/вниз
- Швидке надання нових ресурсів
- Звільнення непотрібних ресурсів
- **Приклад**: Auto Scaling Groups в AWS

#### 2.2.5 Measured service (Вимірюваність сервісу)
- Моніторинг використання ресурсів
- Оплата за фактичне використання (pay-as-you-go)
- Прозорість витрат
- **Приклад**: CloudWatch metrics, AWS Cost Explorer

---

## 3. Моделі обслуговування (Service Models): IaaS / PaaS / SaaS (20 хвилин)

### 3.1 Infrastructure as a Service (IaaS)

**Визначення**: Надання віртуалізованих обчислювальних ресурсів через інтернет.

**Що надається**:
- Віртуальні машини
- Сховища даних
- Мережа
- Операційні системи (вибір користувача)

**Відповідальність користувача**:
- ОС та патчі
- Middleware, runtime
- Додатки та дані
- Налаштування мережі

**Відповідальність провайдера**:
- Фізичне обладнання
- Віртуалізація
- Мережева інфраструктура
- Дата-центри

**Приклади**:
- AWS: EC2, S3, VPC
- Azure: Virtual Machines, Blob Storage
- Google Cloud: Compute Engine, Cloud Storage

**Переваги**:
- Максимальна гнучкість
- Повний контроль над ОС
- Можливість міграції legacy додатків

**Недоліки**:
- Потребує більше управління
- Відповідальність за безпеку ОС
- Більше операційного overhead

### 3.2 Platform as a Service (PaaS)

**Визначення**: Надання платформи для розробки, тестування та розгортання додатків.

**Що надається**:
- Runtime середовище
- Middleware
- Бази даних
- Інструменти розробки
- ОС (керована провайдером)

**Відповідальність користувача**:
- Код додатку
- Конфігурація додатку
- Дані

**Відповідальність провайдера**:
- ОС та патчі
- Runtime та frameworks
- Масштабування платформи
- Інфраструктура

**Приклади**:
- AWS: Elastic Beanstalk, Lambda, RDS
- Azure: App Service, Azure Functions
- Google Cloud: App Engine, Cloud Functions
- Heroku, Cloud Foundry

**Переваги**:
- Фокус на коді, а не інфраструктурі
- Автоматичне масштабування
- Вбудовані сервіси (DB, cache, queue)

**Недоліки**:
- Менша гнучкість
- Обмеження платформи
- Potential vendor lock-in

### 3.3 Software as a Service (SaaS)

**Визначення**: Надання готових додатків через інтернет.

**Що надається**:
- Повністю готовий додаток
- Інтерфейс користувача
- Все необхідне для роботи

**Відповідальність користувача**:
- Використання додатку
- Дані (в рамках додатку)
- Конфігурація (обмежена)

**Відповідальність провайдера**:
- Весь стек: від інфраструктури до додатку
- Оновлення та патчі
- Безпека
- Доступність

**Приклади**:
- Gmail, Google Workspace
- Microsoft 365
- Salesforce
- Dropbox, Slack, Zoom

**Переваги**:
- Немає управління інфраструктурою
- Швидкий старт
- Автоматичні оновлення

**Недоліки**:
- Мінімальна кастомізація
- Залежність від провайдера
- Обмежена інтеграція

### 3.4 Порівняльна таблиця відповідальності

```
Компонент         | On-Premises | IaaS | PaaS | SaaS
------------------|-------------|------|------|------
Додатки           | Ви          | Ви   | Ви   | Провайдер
Дані              | Ви          | Ви   | Ви   | Провайдер
Runtime           | Ви          | Ви   | Пров.| Провайдер
Middleware        | Ви          | Ви   | Пров.| Провайдер
ОС                | Ви          | Ви   | Пров.| Провайдер
Віртуалізація     | Ви          | Пров.| Пров.| Провайдер
Сервери           | Ви          | Пров.| Пров.| Провайдер
Сховище           | Ви          | Пров.| Пров.| Провайдер
Мережа            | Ви          | Пров.| Пров.| Провайдер
```

---

## 4. Моделі розгортання (Deployment Models) (15 хвилин)

### 4.1 Public Cloud (Публічна хмара)

**Визначення**: Хмарні сервіси, доступні для загального користування через інтернет.

**Характеристики**:
- Спільна інфраструктура (multi-tenancy)
- Оплата за використання
- Немає початкових інвестицій
- Провайдер керує всією інфраструктурою

**Приклади**: AWS, Azure, Google Cloud Platform

**Переваги**:
- Низькі початкові витрати
- Швидке масштабування
- Не потрібно управління обладнанням
- Глобальна доступність

**Недоліки**:
- Менший контроль
- Можливі регуляторні обмеження
- Залежність від інтернету

**Коли використовувати**:
- Стартапи та малий бізнес
- Змінне навантаження
- Тестування та розробка
- Глобальні додатки

### 4.2 Private Cloud (Приватна хмара)

**Визначення**: Хмарна інфраструктура, яка використовується однією організацією.

**Характеристики**:
- Виділена інфраструктура
- Може бути on-premises або у провайдера
- Повний контроль
- Власне управління або керована третьою стороною

**Приклади**: 
- VMware vCloud
- OpenStack
- AWS Outposts (hybrid)

**Переваги**:
- Повний контроль
- Відповідність регуляторним вимогам
- Кращий контроль безпеки
- Передбачувані витрати

**Недоліки**:
- Високі початкові витрати
- Потреба в персоналі
- Складність масштабування
- Відповідальність за обладнання

**Коли використовувати**:
- Регульовані галузі (банки, медицина)
- Строгі вимоги до безпеки
- Специфічні потреби в продуктивності
- Legacy системи з обмеженнями

### 4.3 Hybrid Cloud (Гібридна хмара)

**Визначення**: Комбінація приватної та публічної хмар з оркестрацією між ними.

**Характеристики**:
- Дані та додатки можуть переміщуватися між хмарами
- Єдине управління
- Гнучкість розміщення
- Інтеграція on-premises та cloud

**Технології зв'язування**:
- VPN connections
- Direct Connect / Express Route
- API integration
- Data synchronization

**Приклади**:
- Azure Arc
- AWS Outposts
- Google Anthos
- VMware Cloud

**Переваги**:
- Гнучкість розміщення
- Поступова міграція
- Burst to cloud (CloudBursting)
- Оптимізація витрат

**Недоліки**:
- Складність управління
- Потреба в інтеграції
- Можливі проблеми з мережею
- Складність безпеки

**Коли використовувати**:
- Поступова міграція в хмару
- Різні вимоги до розміщення даних
- Пікові навантаження
- Disaster recovery

### 4.4 Multi-Cloud (Мультихмара)

**Визначення**: Використання сервісів від кількох хмарних провайдерів.

**Характеристики**:
- Сервіси від AWS, Azure, GCP одночасно
- Уникнення vendor lock-in
- Best-of-breed підхід
- Складна архітектура

**Стратегії**:
1. **Distributed**: Різні додатки на різних провайдерах
2. **Redundant**: Дублювання для надійності
3. **Specialized**: Використання унікальних сервісів

**Переваги**:
- Уникнення залежності від одного провайдера
- Використання найкращих сервісів
- Географічне покриття
- Переговорна позиція з провайдерами

**Недоліки**:
- Найвища складність
- Різні інструменти та API
- Складність навчання команди
- Потреба в абстракції

**Коли використовувати**:
- Критичні системи з вимогами до надійності
- Глобальні компанії з різними регіональними вимогами
- Уникнення vendor lock-in
- Регуляторні вимоги в різних країнах

---

## 5. Cloud ≠ AWS: Vendor-Agnostic підхід (10 хвилин)

### 5.1 Поширена помилка

**Хибне уявлення**: "Cloud" = "AWS"

**Реальність**: AWS - один з багатьох хмарних провайдерів

### 5.2 Чому це важливо?

1. **Технологічна грамотність**
   - Розуміння концепцій vs. конкретних реалізацій
   - Portable знання

2. **Архітектурне мислення**
   - Проектування vendor-agnostic рішень
   - Уникнення lock-in з самого початку

3. **Кар'єрна гнучкість**
   - Можливість працювати з різними платформами
   - Адаптивність до змін на ринку

### 5.3 Основні хмарні провайдери

#### Amazon Web Services (AWS)
- **Запуск**: 2006
- **Частка ринку**: ~32% (2023)
- **Сильні сторони**: Найширший набір сервісів, найбільша екосистема
- **Основні сервіси**: EC2, S3, Lambda, RDS

#### Microsoft Azure
- **Запуск**: 2010
- **Частка ринку**: ~23% (2023)
- **Сильні сторони**: Інтеграція з Microsoft продуктами, hybrid cloud
- **Основні сервіси**: Virtual Machines, Blob Storage, Azure Functions

#### Google Cloud Platform (GCP)
- **Запуск**: 2011
- **Частка ринку**: ~10% (2023)
- **Сильні сторони**: Big Data, ML/AI, Kubernetes
- **Основні сервіси**: Compute Engine, Cloud Storage, Cloud Functions

#### Інші провайдери
- **Alibaba Cloud**: Домінування в Азії
- **Oracle Cloud**: Фокус на enterprise та бази даних
- **IBM Cloud**: Hybrid, enterprise
- **DigitalOcean**: Простота, developers

### 5.4 Vendor-Agnostic архітектура

**Принципи**:

1. **Використання стандартів**
   - Kubernetes замість ECS
   - PostgreSQL замість Aurora (якщо можливо)
   - OpenTelemetry для спостережуваності

2. **Абстракція специфічних сервісів**
   - Wrapper для cloud storage
   - Абстракція черг повідомлень
   - Database abstraction layers

3. **Infrastructure as Code (IaC)**
   - Terraform (мульти-провайдер)
   - Pulumi
   - Crossplane

4. **Containerization**
   - Docker для портабельності
   - Kubernetes для оркестрації
   - Cloud-agnostic deployment

**Приклад концептуального маппінгу**:

```
Концепція              | AWS          | Azure              | GCP
-----------------------|--------------|--------------------|-----------------
VM                     | EC2          | Virtual Machines   | Compute Engine
Object Storage         | S3           | Blob Storage       | Cloud Storage
Serverless Functions   | Lambda       | Azure Functions    | Cloud Functions
Managed Kubernetes     | EKS          | AKS                | GKE
Managed DB (SQL)       | RDS          | Azure SQL Database | Cloud SQL
NoSQL DB               | DynamoDB     | Cosmos DB          | Firestore
Load Balancer          | ELB/ALB      | Load Balancer      | Cloud Load Balancing
```

### 5.5 Коли використовувати специфічні сервіси?

**За vendor-agnostic**:
- Довгострокові проекти
- Можливість зміни провайдера
- Multi-cloud стратегія
- Уникнення lock-in

**За специфічні сервіси**:
- Унікальні можливості (напр., SageMaker для ML)
- Значне покращення productivity
- Cost optimization
- Performance requirements

---

## 6. API-First та Automation-First підхід (15 хвилин)

### 6.1 API-First підхід

**Визначення**: Всі операції з хмарою виконуються через API, веб-консоль - лише UI над API.

#### Переваги API-First

1. **Автоматизація**
   - Scripting операцій
   - CI/CD інтеграція
   - Infrastructure as Code

2. **Повторюваність**
   - Однакові операції завжди виконуються однаково
   - Документування через код

3. **Версіонування**
   - Відстеження змін
   - Rollback можливості

4. **Інтеграція**
   - З іншими системами
   - Custom dashboards
   - Monitoring та alerting

#### Способи взаємодії з API

**1. Web Console (UI)**
- Візуальний інтерфейс
- Для learning та exploration
- Обмежена автоматизація

**2. CLI (Command Line Interface)**
- AWS CLI, Azure CLI, gcloud
- Scripting
- Interactive використання

```bash
# AWS CLI приклад
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t2.micro \
  --key-name MyKeyPair
```

**3. SDKs (Software Development Kits)**
- Python (boto3), JavaScript, Java, .NET, etc.
- Інтеграція в додатки
- Type-safe операції

```python
# boto3 (AWS SDK for Python) приклад
import boto3

ec2 = boto3.resource('ec2')
instance = ec2.create_instances(
    ImageId='ami-0abcdef1234567890',
    InstanceType='t2.micro',
    KeyName='MyKeyPair',
    MinCount=1,
    MaxCount=1
)
```

**4. Infrastructure as Code (IaC)**
- Terraform, CloudFormation, Pulumi
- Декларативний підхід
- State management

```hcl
# Terraform приклад
resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

### 6.2 Automation-First підхід

**Принцип**: Якщо робиш щось більше одного разу - автоматизуй.

#### Чому автоматизація важлива?

1. **Швидкість**
   - Миттєве виконання повторюваних задач
   - Паралельне виконання

2. **Надійність**
   - Відсутність людських помилок
   - Консистентність

3. **Масштабованість**
   - Обробка сотень/тисяч ресурсів
   - Однакова складність для 1 та 1000 ресурсів

4. **Документація**
   - Код як документація
   - Self-documenting infrastructure

5. **Compliance**
   - Автоматична перевірка політик
   - Audit trail

#### Рівні автоматизації

**Рівень 1: Manual (Ручний)**
- Всі дії через веб-консоль
- Високий ризик помилок
- Не масштабується

**Рівень 2: Scripted (Скриптовий)**
- Bash/Python скрипти
- CLI команди
- Базова автоматизація

**Рівень 3: Infrastructure as Code**
- Terraform, CloudFormation
- Декларативний підхід
- Version control

**Рівень 4: GitOps**
- Git як single source of truth
- Automated reconciliation
- Pull request workflow

**Рівень 5: Self-Healing**
- Автоматичне виправлення проблем
- Chaos engineering
- Advanced automation

#### Best Practices автоматизації

1. **Version Control Всього**
   - Infrastructure code в Git
   - Конфігурації в репозиторіях
   - Branching та PR workflow

2. **Idempotency**
   - Повторне виконання не змінює результат
   - Безпечні re-runs

3. **Testing**
   - Unit тести для IaC
   - Integration тести
   - Compliance тести

4. **Модульність**
   - Reusable модулі
   - DRY principle
   - Composition

5. **Documentation**
   - README для кожного проекту
   - Inline коментарі
   - Architecture diagrams

---

## 7. AWS як реалізація архітектурних принципів (20 хвилин)

### 7.1 Чому AWS як приклад?

1. **Лідер ринку**: Найбільша частка ринку
2. **Піонер**: Перший major public cloud провайдер
3. **Найширший набір сервісів**: 200+ сервісів
4. **Well-Architected Framework**: Документовані best practices
5. **Навчальні ресурси**: Величезна екосистема матеріалів

**Важливо**: AWS - це приклад, не єдине рішення!

### 7.2 Глобальна інфраструктура AWS

#### Regions (Регіони)

**Визначення**: Географічна область з кількома ізольованими дата-центрами.

**Характеристики**:
- Повністю ізольовані один від одного
- Власні копії сервісів
- Відповідність data residency requirements
- Різні ціни в різних регіонах

**Приклади**:
- us-east-1 (N. Virginia)
- eu-west-1 (Ireland)
- ap-southeast-1 (Singapore)
- eu-central-1 (Frankfurt)

**Вибір регіону залежить від**:
1. **Latency**: Близькість до користувачів
2. **Compliance**: Законодавчі вимоги
3. **Availability**: Доступність сервісів
4. **Cost**: Різні ціни

#### Availability Zones (Зони доступності)

**Визначення**: Один або більше дата-центрів в межах регіону.

**Характеристики**:
- Фізично розділені (різні будівлі)
- З'єднані високошвидкісними приватними мережами
- Незалежне електроживлення та мережа
- Для high availability

**Типовий регіон**: 3+ Availability Zones

**Best Practice**: Розгортати в multiple AZ для відмовостійкості

```
Region: us-east-1
├── AZ: us-east-1a
├── AZ: us-east-1b
├── AZ: us-east-1c
└── AZ: us-east-1d
```

#### Edge Locations

**Визначення**: Точки присутності для CloudFront (CDN).

**Характеристики**:
- 400+ локацій по всьому світу
- Кешування контенту близько до користувачів
- Зменшення latency

### 7.3 AWS Free Tier

**Мета**: Безкоштовне ознайомлення з AWS та тестування.

#### Типи Free Tier

**1. 12 Months Free**
- Починається з дати реєстрації
- **EC2**: 750 годин/місяць t2.micro або t3.micro
- **S3**: 5 GB storage
- **RDS**: 750 годин/місяць db.t2.micro
- **CloudFront**: 50 GB data transfer

**2. Always Free**
- Без обмеження часу
- **Lambda**: 1 млн запитів/місяць
- **DynamoDB**: 25 GB storage
- **SNS**: 1000 публікацій/місяць
- **CloudWatch**: 10 custom metrics

**3. Trials**
- Короткострокові пробні періоди
- Для специфічних сервісів

#### Важливо про Free Tier

**Обережно**:
- Легко перевищити ліміти
- Настроїть Billing Alerts!
- AWS не завжди зупиняє resources автоматично
- Видаляйте непотрібні ресурси

**Захист від несподіваних витрат**:
```bash
# Створити бюджет
aws budgets create-budget \
  --budget file://budget.json \
  --notifications-with-subscribers file://notifications.json
```

### 7.4 Основні AWS сервіси (приклади)

#### Compute

**EC2 (Elastic Compute Cloud)**
- Віртуальні машини в хмарі
- Різні типи instances для різних навантажень
- Pay per second/hour

**Lambda**
- Serverless функції
- Оплата за виконання
- Event-driven

**ECS/EKS**
- Container orchestration
- ECS: власне рішення AWS
- EKS: managed Kubernetes

#### Storage

**S3 (Simple Storage Service)**
- Object storage
- 11 9's durability
- Різні storage classes

**EBS (Elastic Block Store)**
- Block storage для EC2
- Persistent storage

**EFS (Elastic File System)**
- Managed NFS
- Shared file system

#### Database

**RDS (Relational Database Service)**
- Managed SQL databases
- MySQL, PostgreSQL, Oracle, SQL Server
- Automated backups, patching

**DynamoDB**
- NoSQL database
- Serverless
- Single-digit millisecond latency

#### Networking

**VPC (Virtual Private Cloud)**
- Ізольована мережа
- Subnets, routing, security groups

**Route 53**
- DNS service
- Domain registration
- Health checking

**CloudFront**
- CDN
- Global content delivery

### 7.5 AWS Well-Architected Framework

**6 Pillars (Стовпів)**:

1. **Operational Excellence**
   - Автоматизація
   - Моніторинг
   - Continuous improvement

2. **Security**
   - Identity and access management
   - Detective controls
   - Infrastructure protection

3. **Reliability**
   - Recover from failures
   - Scale horizontally
   - Automatic recovery

4. **Performance Efficiency**
   - Use advanced technologies
   - Go global in minutes
   - Serverless architectures

5. **Cost Optimization**
   - Pay only for what you use
   - Measure efficiency
   - Stop spending on data centers

6. **Sustainability**
   - Minimize environmental impact
   - Shared responsibility model

---

## 8. Практичні поради та Best Practices (5 хвилин)

### 8.1 Початок роботи з хмарою

1. **Навчання**
   - Безкоштовні курси провайдерів
   - Документація
   - Hands-on practice

2. **Безпека перш за все**
   - MFA для всіх акаунтів
   - Least privilege principle
   - Never commit credentials to Git

3. **Контроль витрат**
   - Billing alerts
   - Видалення unused resources
   - Використання Free Tier

4. **Version Control**
   - Infrastructure as Code
   - Git для всіх конфігурацій
   - Documentation

### 8.2 Типові помилки початківців

1. **Залишені ресурси**
   - Забуті EC2 instances
   - Невикористані volumes
   - Orphaned snapshots

2. **Відсутність моніторингу**
   - Немає alerting
   - Відсутність logging
   - Сліпі плями

3. **Слабка безпека**
   - Публічні credentials
   - Відкриті security groups
   - No MFA

4. **Відсутність disaster recovery**
   - No backups
   - Single point of failure
   - No tested recovery plan

---

## 9. Висновки та підсумки (5 хвилин)

### Ключові takeaways

1. **Хмарні обчислення** - це архітектурна парадигма, а не просто "чужі комп'ютери"

2. **5 характеристик NIST** - фундамент розуміння cloud computing

3. **IaaS / PaaS / SaaS** - різні рівні абстракції та відповідальності

4. **Deployment models** - вибір залежить від вимог бізнесу, security, compliance

5. **Cloud ≠ AWS** - vendor-agnostic мислення для гнучкості

6. **API-first + Automation-first** - основа сучасних cloud операцій

7. **AWS** - приклад реалізації, а не єдине рішення

### Наступні кроки

1. **Практика**: Створити AWS Free Tier акаунт
2. **Експериментувати**: Запустити першу EC2 instance, S3 bucket
3. **Вивчати**: AWS CLI, основи Terraform
4. **Читати**: AWS Well-Architected Framework

---

## Питання для самоперевірки

1. Які 5 основних характеристик cloud computing за NIST?
2. В чому різниця між IaaS, PaaS та SaaS? Наведіть приклади.
3. Які переваги та недоліки public vs. private cloud?
4. Що таке multi-cloud та коли його використовувати?
5. Чому важливо мислити vendor-agnostic?
6. Що таке API-first підхід і чому він важливий?
7. Які три типи AWS Free Tier існують?
8. Що таке Availability Zone і як це відноситься до high availability?
9. Назвіть принципи AWS Well-Architected Framework.
10. Які перші кроки для безпечної роботи з cloud?

---

## Додаткові ресурси

Дивіться файл `references.md` для повного списку літератури та корисних посилань.

---

**Версія**: 1.0  
**Останнє оновлення**: 2026-02-11  
**Автор**: Курс "Cloud Computing"
