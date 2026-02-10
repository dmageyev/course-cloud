# Хмарні технології інформаційних систем (Cloud & DevSecOps)

## Опис курсу

**Рівень:** бакалавр ІТ  
**Обсяг:** 20 год лекцій  
**Платформа:** AWS (як референс)

Даний курс призначений для формування у студентів **архітектурного та інженерного розуміння хмарних інформаційних систем**, базових практичних навичок роботи з **AWS, Git, DevSecOps** та підготовки до **Junior Cloud / DevOps ролей**.

Курс поєднує теоретичні основи побудови хмарних систем з практичним DevSecOps підходом. Amazon Web Services (AWS) використовується як референсна платформа для демонстрації архітектурних принципів та індустріальних практик.

## Структура курсу

Курс складається з наступних компонентів:

- 🧠 **10 лекцій** (по 2 години, всього 20 годин)
- 🧪 **2 практичні заняття**
- 🔬 **4 лабораторні роботи**
- 🏠 **4 домашні завдання** (Git / Cloud / DevSecOps)
- 🎓 **Фінальний проєкт** (рекомендовано)

### Лекційний курс (20 годин / 10 лекцій)

#### Лекція 1. Хмарні обчислення як архітектурна парадигма (2 год)

**Теорія:**
- Еволюція ІТ-інфраструктури
- Визначення Cloud (NIST)
- IaaS / PaaS / SaaS
- Public / Private / Hybrid / Multi-cloud
- Cloud ≠ AWS
- API-first, automation-first підхід

**AWS-приклади:**
- AWS як реалізація архітектурних принципів
- Free Tier, регіони

---

#### Лекція 2. Глобальна архітектура хмар та відповідальність (2 год)

**Теорія:**
- Regions, AZ, Edge
- Latency, data residency
- SLA, SLO, availability
- Shared Responsibility Model
- RTO / RPO
- Design for failure

**AWS-приклади:**
- Multi-AZ
- Реальні outage кейси

---

#### Лекція 3. Безпека хмарних ІС: DevSecOps mindset (2 год)

**Теорія:**
- Threat model у хмарі
- Misconfiguration як головний ризик
- IAM як архітектурний компонент
- Least privilege
- Zero Trust
- Dev / Staging / Prod

**AWS-приклади:**
- IAM users, roles, policies
- Security Groups vs NACL
- MFA

---

#### Лекція 4. Мережева архітектура хмар (2 год)

**Теорія:**
- VPC як логічна ізоляція
- CIDR, IP-addressing
- Public vs Private subnet
- Routing, NAT
- Ingress / Egress
- Bastion host
- 2-tier / 3-tier patterns

**AWS-приклади:**
- VPC, Subnets
- Route Tables
- IGW, NAT Gateway

---

#### Лекція 5. Обчислювальні ресурси та Linux у хмарі (2 год)

**Теорія:**
- Гіпервізори
- VM vs Containers vs Serverless
- Linux-first mindset
- systemd, services, ports
- Immutable vs mutable infra
- Stateless systems

**AWS-приклади:**
- EC2
- Auto Scaling
- AMI

---

#### Лекція 6. Зберігання даних та життєвий цикл (2 год)

**Теорія:**
- Object / Block / File storage
- Data lifecycle
- Backup vs Archive
- CAP theorem
- GDPR, data residency

**AWS-приклади:**
- S3 (Lifecycle, versioning)
- EBS
- EFS

---

#### Лекція 7. Хмарні бази даних (2 год)

**Теорія:**
- RDBMS vs NoSQL
- ACID vs BASE
- DB як bottleneck
- Scaling databases
- Anti-patterns

**AWS-приклади:**
- RDS
- DynamoDB
- Aurora
- Multi-AZ

---

#### Лекція 8. Масштабування та відмовостійкість (2 год)

**Теорія:**
- Single Point of Failure
- Load balancing patterns
- Health checks
- Auto Scaling
- DR strategies
- Blue/Green deployment (concept)

**AWS-приклади:**
- ALB
- ASG

---

#### Лекція 9. Cloud-native та контейнеризація (2 год)

**Теорія:**
- Cloud-native principles
- Microservices
- Containers
- Docker (concept)
- Kubernetes (idea, no practice)
- Event-driven systems

**AWS-приклади:**
- ECS / EKS (overview)
- Lambda

---

#### Лекція 10. DevOps, IaC та економіка хмари (2 год)

**Теорія:**
- DevOps & DevSecOps
- CI/CD pipelines (diagram)
- Infrastructure as Code
- Terraform — industry standard
- FinOps basics

**AWS-приклади:**
- CloudWatch
- Cost Explorer
- Well-Architected Tool

---

### Практичні роботи (2 заняття)

#### 🧪 Практичне заняття 1: Git + Cloud workflow

**Зміст:**
- GitHub / GitLab
- Repo structure для cloud-проєкту
- README як технічна документація
- Cloud architecture diagram (draw.io)

**🎯 Результат:** студент веде cloud-проєкт у Git

---

#### 🧪 Практичне заняття 2: DevSecOps: мислення і пайплайн

**Зміст:**
- CI/CD pipeline (conceptual)
- Secrets management
- Env separation
- Security scanning (idea)

**🎯 Результат:** студент розуміє DevSecOps lifecycle

---

### Лабораторні роботи (4 заняття)

#### 🔬 Лабораторна робота 1: IAM + Secure AWS account

**Завдання:**
- IAM user, group
- MFA
- Least privilege policy
- Git: commit policy

---

#### 🔬 Лабораторна робота 2: Networking + EC2

**Завдання:**
- VPC
- Public subnet
- EC2 + Linux
- Security Groups
- SSH

---

#### 🔬 Лабораторна робота 3: Storage + Database

**Завдання:**
- S3 bucket
- Static hosting
- RDS (MySQL/Postgres)
- Backup

---

#### 🔬 Лабораторна робота 4: Highly Available Web Architecture

**Завдання:**
- 2×EC2
- ALB
- Auto Scaling
- Monitoring (CloudWatch)

---

### Домашні завдання (4 завдання)

#### 🏠 Домашнє завдання 1: Git

**Завдання:**
- Fork репозиторію
- README + architecture diagram
- Pull Request

---

#### 🏠 Домашнє завдання 2: Cloud Architecture

**Завдання:**
- Запропонувати cloud-архітектуру для кейсу
- Обґрунтувати вибір сервісів

---

#### 🏠 Домашнє завдання 3: DevSecOps mindset

**Завдання:**
- Знайти cloud security incident
- Проаналізувати помилки

---

#### 🏠 Домашнє завдання 4: FinOps

**Завдання:**
- Оцінити вартість архітектури
- Запропонувати оптимізацію

---

### 🎓 Фінальний проєкт (рекомендовано)

**Тема:** Спроєктувати хмарну інформаційну систему та захистити архітектуру

**Оцінюється:**
- Архітектурне мислення
- Безпека
- Масштабування
- Вартість
- Git-документація

## Мета курсу

Сформувати у студента **архітектурне та інженерне розуміння хмарних ІС**, базові практичні навички роботи з **AWS, Git, DevSecOps**, підготувати до **Junior Cloud / DevOps ролей**.

## Методологія викладання

Курс побудований на принципах:
- **Архітектурного мислення**: розуміння принципів проектування систем
- **DevSecOps практик**: безпека як інтегральна частина розробки
- **Індустріальних стандартів**: використання AWS, Git, Infrastructure as Code
- **Практичної спрямованості**: підготовка до реальних робочих завдань
- **API-first, automation-first**: сучасний підхід до хмарних технологій

## Цільова аудиторія

Курс призначений для студентів, які:
- Мають базові знання з інформаційних систем та мереж
- Знайомі з основами Linux та командного рядка
- Розуміють принципи програмування
- Прагнуть розвиватися в напрямку Cloud/DevOps інженерії

## Очікувані результати навчання

Після завершення курсу студенти зможуть:
- Проектувати архітектуру хмарних інформаційних систем
- Застосовувати принципи DevSecOps у розробці
- Працювати з AWS сервісами (EC2, VPC, S3, RDS, ALB)
- Використовувати Git для документування та версіонування
- Аналізувати аспекти безпеки та оптимізації вартості
- Створювати високодоступні та масштабовані рішення
- Розуміти принципи Infrastructure as Code

## Структура репозиторію

```
course-cloud/
├── lectures/          # Матеріали 10 лекцій
│   ├── lecture-01/    # Хмарні обчислення як архітектурна парадигма
│   ├── lecture-02/    # Глобальна архітектура хмар та відповідальність
│   ├── lecture-03/    # Безпека хмарних ІС: DevSecOps mindset
│   ├── lecture-04/    # Мережева архітектура хмар
│   ├── lecture-05/    # Обчислювальні ресурси та Linux у хмарі
│   ├── lecture-06/    # Зберігання даних та життєвий цикл
│   ├── lecture-07/    # Хмарні бази даних
│   ├── lecture-08/    # Масштабування та відмовостійкість
│   ├── lecture-09/    # Cloud-native та контейнеризація
│   └── lecture-10/    # DevOps, IaC та економіка хмари
├── practicals/        # 2 практичні заняття
│   ├── practical-01/  # Git + Cloud workflow
│   └── practical-02/  # DevSecOps: мислення і пайплайн
├── labs/              # 4 лабораторні роботи
│   ├── lab-01/        # IAM + Secure AWS account
│   ├── lab-02/        # Networking + EC2
│   ├── lab-03/        # Storage + Database
│   └── lab-04/        # Highly Available Web Architecture
├── homework/          # 4 домашні завдання
│   ├── hw-01/         # Git
│   ├── hw-02/         # Cloud Architecture
│   ├── hw-03/         # DevSecOps mindset
│   └── hw-04/         # FinOps
├── final-project/     # Фінальний проєкт
└── TODO.md            # Інструкції для генерації матеріалів
```

## Ліцензія

Матеріали курсу призначені для освітніх цілей.