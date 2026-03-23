# TODO: Генерація навчальних матеріалів за допомогою ШІ

## Загальні інструкції

Цей документ містить інструкції для генерації навчальних матеріалів курсу "Хмарні технології інформаційних систем (Cloud & DevSecOps)" за допомогою систем штучного інтелекту.

## Концепція курсу

**Рівень:** Бакалавр ІТ  
**Фокус:** DevSecOps-підхід до хмарних технологій  
**Мета:** Підготовка до Junior Cloud / DevOps ролей  
**Стиль:** Практично-орієнтований з архітектурним мисленням

## Вимоги до генерації

### Загальні принципи
- **Мова**: Українська (технічна, з використанням індустріальних термінів)
- **Формат**: Markdown для конспектів, reveal.js для слайдів
- **Підхід**: 50% теорія + 50% практика/AWS приклади
- **Стиль**: Практичний, індустріальний (не суто академічний)
- **DevSecOps mindset**: Безпека інтегрована на всіх етапах
- **Структура**: Від концепцій до практичного застосування

### Ключові акценти
- **API-first, automation-first** підхід
- **Security by design**, не afterthought
- **Cost awareness** (FinOps thinking)
- **Production-ready practices**
- **Real-world scenarios** та case studies
- **Linux-first mindset**

### Компоненти для кожної лекції

Для кожної лекції необхідно створити:

1. **Конспект лекції** (`lecture-XX/notes.md`)
   - Теоретичні концепції (50%)
   - Практичні AWS приклади (50%)
   - Діаграми (Mermaid)
   - Code snippets (де доречно)
   - Real-world scenarios
   - Security considerations
   - Best practices та anti-patterns
   - Обсяг: 3000-4000 слів

2. **Слайди** (`lecture-XX/slides.md`)
   - 25-35 слайдів
   - Візуальні схеми та діаграми
   - Code examples
   - Мінімум тексту, максимум візуалізації
   - Clear takeaways

3. **Список ресурсів** (`lecture-XX/references.md`)
   - AWS офіційна документація
   - Industry blogs (AWS, HashiCorp, тощо)
   - Best practice guides
   - Relevant certifications

## План генерації лекцій

### Лекція 1: Хмарні обчислення як архітектурна парадигма

**Промпт для ШІ:**
```
Створи матеріали лекції українською мовою для курсу DevSecOps/Cloud на тему "Хмарні обчислення як архітектурна парадигма".

Цільова аудиторія: студенти бакалавріату ІТ, підготовка до Junior Cloud/DevOps ролей.

Структура (2 години):

ТЕОРІЯ (50%):
1. Еволюція ІТ-інфраструктури
   - Від physical servers до cloud
   - Key milestones та drivers змін
2. Визначення Cloud computing (NIST)
   - 5 характеристик
   - Чому це важливо
3. Моделі обслуговування
   - IaaS, PaaS, SaaS
   - Responsibility matrix
4. Моделі розгортання
   - Public, Private, Hybrid, Multi-cloud
   - Trade-offs кожної
5. Cloud ≠ AWS
   - Vendor-agnostic thinking
   - Portable skills
6. API-first, automation-first підхід
   - Infrastructure as Code mentality
   - Everything is programmable

ПРАКТИКА (50%):
1. AWS як референсна реалізація
   - Regions, AZs концептуально
   - Free Tier та як почати
2. AWS Console vs CLI vs IaC
   - Порівняння підходів
   - Коли що використовувати
3. Приклад: створення першого ресурсу різними способами

ДІАГРАМИ (Mermaid):
- Еволюція інфраструктури (timeline)
- IaaS/PaaS/SaaS responsibility model
- Cloud deployment models порівняння

КЛЮЧОВІ TAKEAWAYS:
- Cloud - це архітектурний підхід, не тільки AWS
- API-first thinking критично важливий
- Розуміння shared responsibility model
- Automation over manual work

SECURITY NOTES:
- Root account management з першого дня
- MFA importance
- Least privilege principle intro

Обсяг: 3500-4000 слів
Стиль: практичний, готуй до реального використання
```

---

### Лекція 2: Глобальна архітектура хмар та відповідальність

**Промпт для ШІ:**
```
Створи матеріали лекції українською мовою на тему "Глобальна архітектура хмар та відповідальність".

ТЕОРІЯ (50%):
1. Regions, Availability Zones, Edge Locations
   - Фізична vs логічна структура
   - Latency та географія
2. Data residency та compliance
   - GDPR, локальні вимоги
   - Вибір region
3. SLA, SLO, SLI
   - Визначення та різниця
   - Як читати AWS SLA
   - Availability math (99.9% vs 99.99%)
4. Shared Responsibility Model
   - Що забезпечує AWS
   - Що забезпечує клієнт
   - Security IN the cloud vs OF the cloud
5. RTO/RPO
   - Recovery objectives
   - Backup vs DR
6. Design for Failure
   - Murphy's Law у хмарі
   - Chaos Engineering intro

ПРАКТИКА (50%):
1. AWS Multi-AZ architecture
   - Приклади сервісів
   - Cost vs availability trade-offs
2. Реальні outage кейси
   - AWS us-east-1 incidents
   - Lessons learned
   - Post-mortems аналіз
3. Availability розрахунки
   - Single AZ vs Multi-AZ
   - SLA math

ДІАГРАМИ:
- AWS global infrastructure
- Multi-AZ architecture pattern
- Shared responsibility model visual

ПРАКТИЧНІ СЦЕНАРІЇ:
- Вибір region для EU startup
- Проектування для 99.95% availability
- DR strategy для критичного застосунку

Обсяг: 3500-4000 слів
```

---

### Лекція 3: Безпека хмарних ІС: DevSecOps mindset

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Безпека хмарних ІС: DevSecOps mindset".

ТЕОРІЯ (50%):
1. Threat model у хмарі
   - Cloud-specific загрози
   - OWASP Cloud Top 10
2. Misconfiguration як #1 ризик
   - S3 buckets публічні
   - Security groups wide open
   - Реальні breaches через misconfiguration
3. IAM як архітектурний компонент
   - Authentication vs Authorization
   - Identity-centric security
4. Least Privilege principle
   - Практичне застосування
   - Policy design
5. Zero Trust model
   - Never trust, always verify
   - Micro-segmentation
6. Environment separation
   - Dev / Staging / Prod
   - Account vs VPC separation
   - Blast radius limitation

ПРАКТИКА (50%):
1. IAM deep dive
   - Users vs Roles
   - Groups
   - Policies (managed vs inline)
   - Policy evaluation logic
2. Security Groups vs NACLs
   - Stateful vs stateless
   - Layered security
3. MFA setup
   - Root account
   - IAM users
   - Hardware tokens vs apps
4. Hands-on: створення secure IAM policy
5. AWS CloudTrail intro
   - Logging всіх API calls

ДІАГРАМИ:
- Threat model для web app у хмарі
- IAM components та їх relationships
- Zero Trust architecture
- Security Groups vs NACLs

РЕАЛЬНІ КЕЙСИ:
- Capital One breach 2019
- Як misconfiguration призвів до витоку
- Lessons learned

DevSecOps PRACTICES:
- Security as code
- Policy as code
- Automated security scanning
- Shift left security

Обсяг: 4000-4500 слів
```

---

### Лекція 4: Мережева архітектура хмар

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Мережева архітектура хмар".

ТЕОРІЯ (50%):
1. VPC як Software-Defined Network
   - Логічна ізоляція
   - Tenant isolation
2. CIDR та IP addressing
   - RFC 1918
   - Subnet calculations
   - /24, /16 тощо
3. Public vs Private subnets
   - Internet Gateway
   - NAT Gateway vs NAT Instance
   - Egress only
4. Routing у VPC
   - Route tables
   - Main vs custom
   - Route priority
5. Ingress / Egress patterns
   - Inbound vs outbound traffic
   - Bastion host
   - VPN vs Direct Connect (концепція)
6. Multi-tier architecture
   - 2-tier (web + db)
   - 3-tier (web + app + db)
   - DMZ concept

ПРАКТИКА (50%):
1. VPC design hands-on
   - CIDR planning
   - Subnet creation
   - Route table setup
2. Internet Gateway vs NAT Gateway
   - Коли що використовувати
   - Cost considerations
3. Security layering
   - Security Groups на кожному рівні
   - NACLs для subnet-level control
4. Bastion host setup
   - Jump box pattern
   - SSH key management

ДІАГРАМИ:
- VPC components
- Public/Private subnet architecture
- 3-tier architecture з proper security
- Routing flow diagrams

ПРАКТИЧНИЙ КЕЙС:
Спроектувати VPC для web application:
- 2 public subnets (web tier)
- 2 private subnets (app tier)
- 2 private subnets (db tier)
- Bastion host для admin access
- NAT Gateway для private resources

Обсяг: 3500-4000 слів
```

---

### Лекція 5: Обчислювальні ресурси та Linux у хмарі

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Обчислювальні ресурси та Linux у хмарі".

ТЕОРІЯ (50%):
1. Hypervisors (Type 1 vs Type 2)
   - Nitro System (AWS)
   - Performance implications
2. VM vs Containers vs Serverless
   - Abstraction levels
   - Trade-offs matrix
   - Коли що використовувати
3. Linux-first mindset
   - Чому Linux у production
   - Amazon Linux 2 vs Ubuntu
   - Package management (yum/apt)
4. systemd та service management
   - systemctl
   - Daemon processes
   - Service auto-start
5. Ports та networking
   - Common ports (80, 443, 22, 3306)
   - netstat, ss
   - Firewall concepts (iptables intro)
6. Immutable vs Mutable infrastructure
   - Cattle vs Pets
   - Configuration management era
   - Modern immutable approach
7. Stateless systems
   - 12-factor app principles
   - Ephemeral compute

ПРАКТИКА (50%):
1. EC2 instance types
   - t3, m5, c5, r5 families
   - Sizing decisions
   - Cost optimization
2. User Data scripts
   - Bootstrap при launch
   - Automation examples
3. AMI (Amazon Machine Image)
   - Golden images
   - AMI creation process
   - Versioning
4. Auto Scaling basics
   - Launch configurations
   - Scaling policies
   - Health checks
5. Linux commands essential
   - ssh, scp
   - top, htop, ps
   - systemctl
   - journalctl
   - Basic troubleshooting

ДІАГРАМИ:
- VM vs Containers vs Serverless порівняння
- EC2 instance types decision tree
- Immutable infrastructure flow
- Auto Scaling flow

HANDS-ON SCENARIO:
1. Launch EC2 з User Data
2. Install nginx via script
3. Create AMI
4. Launch new instance from AMI

Обсяг: 4000-4500 слів
```

---

### Лекція 6: Зберігання даних та життєвий цикл

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Зберігання даних та життєвий цикл".

ТЕОРІЯ (50%):
1. Типи storage
   - Object (S3)
   - Block (EBS)
   - File (EFS)
   - Коли що використовувати
2. Object storage deep dive
   - Key-value model
   - Metadata
   - Eventual consistency (S3 now strongly consistent)
3. Data lifecycle management
   - Hot, warm, cold data
   - Archival strategies
4. Backup vs Archive
   - RPO/RTO implications
   - Cost vs recovery time
5. CAP theorem application
   - Consistency vs Availability
   - Partition tolerance
6. Compliance та data residency
   - GDPR requirements
   - Data locality
   - Encryption at rest

ПРАКТИКА (50%):
1. S3 в деталях
   - Buckets та objects
   - Versioning
   - Lifecycle policies
   - Storage classes (Standard, IA, Glacier)
   - S3 Select
2. EBS volumes
   - Types (gp3, io2, st1, sc1)
   - Snapshots
   - EBS vs instance store
3. EFS для shared storage
   - NFS protocol
   - Multi-AZ mount
   - Performance modes
4. Практичний приклад:
   - S3 lifecycle policy
   - Transition to Glacier після 90 днів
   - Expiration після 7 років

ДІАГРАМИ:
- Object vs Block vs File порівняння
- Data lifecycle tiers
- S3 storage classes decision tree
- EBS types performance matrix

SECURITY:
- S3 bucket policies
- Encryption at rest (SSE-S3, SSE-KMS)
- Encryption in transit
- Public access block

COST OPTIMIZATION:
- Storage class selection
- Lifecycle policies для автоматизації
- S3 Intelligent-Tiering

Обсяг: 4000-4500 слів
```

---

### Лекція 7: Хмарні бази даних

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Хмарні бази даних".

ТЕОРІЯ (50%):
1. RDBMS vs NoSQL fundamentals
   - Structured vs unstructured
   - Schema vs schema-less
   - ACID vs BASE
2. Database як потенційний bottleneck
   - Single point of failure
   - Vertical scaling limits
   - Read vs write patterns
3. Scaling databases
   - Read replicas
   - Sharding
   - Connection pooling
4. Consistency models
   - Strong consistency
   - Eventual consistency
   - Read-after-write
5. Database anti-patterns
   - Over-normalization у NoSQL
   - NoSQL для всього
   - Ignoring indexes
   - N+1 queries

ПРАКТИКА (50%):
1. RDS (Relational Database Service)
   - MySQL, PostgreSQL, MariaDB
   - Multi-AZ deployment
   - Read replicas
   - Automated backups
   - Parameter groups
2. DynamoDB
   - Key-value та document model
   - Partition keys та sort keys
   - Indexes (GSI, LSI)
   - On-demand vs provisioned
   - DynamoDB Streams
3. Aurora
   - MySQL/PostgreSQL compatible
   - Serverless option
   - Global Database
   - Performance benefits
4. Database migration strategies
   - DMS (Database Migration Service)
   - Minimal downtime migration

ДІАГРАМИ:
- RDBMS vs NoSQL decision tree
- RDS Multi-AZ architecture
- DynamoDB partition key distribution
- Read replica topology

ПРАКТИЧНИЙ КЕЙС:
E-commerce application database design:
- Products catalog → DynamoDB (read-heavy)
- Orders/Transactions → RDS (ACID critical)
- Session data → ElastiCache
- Analytics → Redshift (згадка)

PERFORMANCE:
- Query optimization
- Index strategy
- Connection pooling
- Caching layer (ElastiCache intro)

Обсяг: 4000-4500 слів
```

---

### Лекція 8: Масштабування та відмовостійкість

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Масштабування та відмовостійкість".

ТЕОРІЯ (50%):
1. Single Point of Failure (SPOF)
   - Identification
   - Elimination strategies
2. Load balancing patterns
   - Round-robin
   - Least connections
   - Weighted
   - Sticky sessions
3. Health checks
   - Active vs passive
   - Graceful degradation
   - Circuit breaker pattern
4. Auto Scaling
   - Horizontal vs vertical
   - Scaling metrics
   - Cooldown periods
   - Predictive scaling
5. Disaster Recovery strategies
   - Backup and restore
   - Pilot light
   - Warm standby
   - Multi-site active-active
   - RTO/RPO trade-offs
6. Blue/Green deployment
   - Zero-downtime deployments
   - Rollback strategy
   - DNS vs Load Balancer switching

ПРАКТИКА (50%):
1. Application Load Balancer (ALB)
   - Target groups
   - Health checks configuration
   - Listener rules
   - Path-based routing
   - Host-based routing
2. Auto Scaling Groups
   - Launch templates
   - Desired/Min/Max capacity
   - Scaling policies
   - Target tracking
   - Step scaling
   - Lifecycle hooks
3. Practical scenario:
   - Web tier з ALB + ASG
   - Health check setup
   - Load testing
   - Observing scale-out

ДІАГРАМИ:
- SPOF examples та remediation
- Load balancer types comparison
- Auto Scaling flow
- DR strategies порівняння
- Blue/Green deployment process

РЕАЛЬНИЙ КЕЙС:
Netflix Chaos Monkey:
- Chaos Engineering principles
- Randomly terminating instances
- Building resilient systems
- Culture of resilience

MONITORING:
- CloudWatch metrics для ASG
- Alarms setup
- SNS notifications

Обсяг: 4000-4500 слів
```

---

### Лекція 9: Cloud-native та контейнеризація

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "Cloud-native та контейнеризація".

ТЕОРІЯ (50%):
1. Cloud-native principles (CNCF)
   - Microservices
   - Containers
   - Dynamic orchestration
   - DevOps culture
2. 12-Factor App methodology
   - Codebase, dependencies, config, backing services, etc.
   - Modern application design
3. Microservices architecture
   - Service decomposition
   - API contracts
   - Service mesh concept
4. Containers fundamentals
   - OS-level virtualization
   - Namespaces та cgroups
   - Image vs container
5. Docker (conceptual)
   - Dockerfile
   - Image layers
   - Registry
   - Не практика, але розуміння
6. Kubernetes (high-level idea)
   - Orchestration need
   - Pods, Services, Deployments (concepts)
   - Managed Kubernetes (EKS)
7. Event-driven architecture
   - Pub/Sub patterns
   - Async communication
   - Event sourcing intro

ПРАКТИКА (50%):
1. ECS (Elastic Container Service)
   - Task definitions
   - Services
   - Fargate vs EC2 launch types
2. EKS (Elastic Kubernetes Service) overview
   - Managed control plane
   - When to use
3. AWS Lambda (Serverless compute)
   - Function as a Service
   - Event triggers
   - Cold start concept
   - Use cases
4. Practical example:
   - Serverless web API з Lambda + API Gateway
   - Event-driven processing з SQS + Lambda

ДІАГРАМИ:
- Monolith vs Microservices
- Container vs VM visual
- ECS architecture
- Event-driven flow
- Serverless web application pattern

ПОРІВНЯННЯ:
| Approach | Use Case | Pros | Cons |
|----------|----------|------|------|
| EC2 | Legacy apps | Full control | Management overhead |
| Containers | Microservices | Portability | Learning curve |
| Serverless | Event-driven | Auto-scaling, no servers | Cold starts, limits |

CLOUD-NATIVE THINKING:
- Design for failure
- Stateless services
- Externalize configuration
- Distributed tracing
- Observability

Обсяг: 4000-5000 слів
```

---

### Лекція 10: DevOps, IaC та економіка хмари

**Промпт для ШІ:**
```
Створи матеріали лекції на тему "DevOps, IaC та економіка хмари".

ТЕОРІЯ (50%):
1. DevOps culture
   - Collaboration over silos
   - Automation
   - Continuous improvement
   - Blameless post-mortems
2. DevSecOps evolution
   - Security embedded in pipeline
   - Shift-left security
   - Security as code
3. CI/CD pipelines
   - Continuous Integration
   - Continuous Delivery vs Deployment
   - Pipeline stages
   - Testing pyramid
4. Infrastructure as Code (IaC)
   - Declarative vs imperative
   - Idempotency
   - Version control для infra
   - GitOps
5. Terraform - industry standard
   - HCL syntax (intro)
   - State management
   - Modules
   - AWS provider
6. FinOps basics
   - Cloud cost management
   - Showback vs chargeback
   - Cost allocation tags
   - Reserved Instances vs Savings Plans vs On-Demand

ПРАКТИКА (50%):
1. CI/CD pipeline diagram
   - Source → Build → Test → Deploy
   - AWS CodePipeline, CodeBuild, CodeDeploy (overview)
   - GitHub Actions, GitLab CI (mention)
2. Terraform приклад (basic)
   - Provider configuration
   - Resource definition
   - terraform init/plan/apply
   - Не глибоко, але концептуально
3. CloudWatch для operational insights
   - Metrics, Logs, Alarms
   - Dashboards
   - Log Insights
4. Cost Explorer
   - Cost visualization
   - Cost anomaly detection
   - Savings recommendations
5. AWS Well-Architected Tool
   - 6 pillars review
   - Operational Excellence
   - Security
   - Reliability
   - Performance Efficiency
   - Cost Optimization
   - Sustainability

ДІАГРАМИ:
- DevOps infinity loop
- CI/CD pipeline detailed
- IaC workflow
- Cost optimization strategies

РЕАЛЬНІ ПРАКТИКИ:
- Tagging strategy для cost allocation
- Reserved Instances planning
- Spot Instances для dev/test
- Auto Scaling for cost efficiency
- S3 Intelligent-Tiering

FinOps MINDSET:
- Cost visibility
- Cost optimization
- Continuous improvement
- Collaboration між teams

TERRAFORM ПРИКЛАД:
```hcl
provider "aws" {
  region = "eu-west-1"
}

resource "aws_instance" "web" {
  ami           = "ami-xxxxx"
  instance_type = "t3.micro"
  
  tags = {
    Name        = "WebServer"
    Environment = "Production"
    CostCenter  = "Engineering"
  }
}
```

Обсяг: 4000-4500 слів
```

---

## План генерації практичних робіт

### Практична робота 1: Git + Cloud workflow

**Промпт для ШІ:**
```
Створи детальні інструкції для практичної роботи "Git + Cloud workflow".

Мета: Навчити студентів вести cloud-проєкт у Git з proper documentation.

ЗАВДАННЯ:
1. Основи Git workflow
   - git init, clone, fork
   - Branching strategy (main/develop/feature)
   - Commit messages best practices
   - Pull Requests

2. Repository structure для cloud проєкту
   ```
   project/
   ├── README.md
   ├── docs/
   │   ├── architecture.md
   │   └── deployment.md
   ├── infrastructure/
   │   └── (IaC код буде пізніше)
   ├── scripts/
   └── .gitignore
   ```

3. README як технічна документація
   - Project overview
   - Architecture diagram
   - Prerequisites
   - Deployment instructions
   - Contribution guidelines

4. Architecture diagram створення
   - draw.io / Lucidchart / CloudCraft
   - AWS Architecture Icons
   - Proper labeling
   - Legend
   - Data flow arrows

ПРАКТИЧНІ КРОКИ:
1. Fork course repository
2. Create branch "homework-1"
3. Create README з діаграмою
4. Commit з describe message
5. Push to fork
6. Create Pull Request

КРИТЕРІЇ ЯКОСТІ:
- Clear and concise README
- Professional diagram
- Proper Git usage
- Good commit messages

ПРИКЛАДИ:
- Надати template README
- Приклад good commit messages
- Sample architecture diagrams

Обсяг інструкцій: 2000-2500 слів
```

### Практична робота 2: DevSecOps: мислення і пайплайн

**Промпт для ШІ:**
```
Створи матеріали для практичної роботи "DevSecOps: мислення і пайплайн".

Мета: Сформувати розуміння DevSecOps lifecycle та CI/CD.

ТЕОРЕТИЧНА ЧАСТИНА:
1. DevSecOps lifecycle stages
   - Plan → Code → Build → Test → Release → Deploy → Operate → Monitor
   - Security на кожному етапі

2. CI/CD pipeline anatomy
   - Source control trigger
   - Build stage
   - Test stages (unit, integration, security)
   - Deployment stages (dev, staging, prod)
   - Rollback mechanism

3. Secrets management
   - Never commit secrets
   - Environment variables
   - AWS Secrets Manager / Parameter Store
   - .env files та .gitignore

4. Environment separation
   - Dev / Staging / Production
   - IAM roles per environment
   - Separate AWS accounts vs VPCs
   - Configuration management

ПРАКТИЧНІ ЗАВДАННЯ:
1. Проаналізувати sample CI/CD pipeline diagram
2. Знайти security gaps у pipeline
3. Створити cheat-sheet для secrets management
4. Спроектувати environment separation strategy
5. Написати security checklist для pipeline

DIAGRAM TASKS:
- Draw CI/CD pipeline з security stages
- Environment isolation diagram
- Secrets management flow

CHECKLIST ДЛЯ СТВОРЕННЯ:
- [ ] Code static analysis (SAST)
- [ ] Dependency scanning (SCA)
- [ ] Container image scanning
- [ ] Infrastructure scanning (IaC security)
- [ ] Dynamic testing (DAST)
- [ ] Compliance checks
- [ ] Access controls verification
- [ ] Secrets scanning

TOOLS MENTIONS (conceptually):
- GitHub Actions / GitLab CI
- SonarQube
- Snyk / Dependabot
- AWS CodePipeline
- Terraform Cloud

Обсяг: 2500-3000 слів
```

---

## План генерації лабораторних робіт

### Лабораторна робота 1: IAM + Secure AWS account

**Промпт для ШІ:**
```
Створи step-by-step інструкції для лабораторної роботи "IAM + Secure AWS account".

ПЕРЕДУМОВИ:
- AWS Free Tier account
- MFA device (phone app)
- Git installed

OBJECTIVES:
1. Secure root account
2. Create IAM users та groups
3. Apply least privilege policies
4. Enable MFA
5. Document in Git

ДЕТАЛЬНІ КРОКИ:

ЧАСТИНА 1: Root Account Security
1. Enable MFA for root
   - Відкрити IAM console
   - Security credentials
   - Add MFA device
   - Scan QR code
   - Enter two consecutive codes
2. Create billing alarm
   - CloudWatch Alarms
   - Set threshold (наприклад, $10)
3. Enable CloudTrail
   - Create trail
   - S3 bucket for logs
   - Log all management events

ЧАСТИНА 2: IAM Users and Groups
1. Create IAM group "Developers"
   - PowerUserAccess policy (initially)
2. Create IAM user (your name)
   - Programmatic + Console access
   - Generate access keys
   - Force password change
3. Add user to group
4. Test access

ЧАСТИНА 3: Least Privilege Policy
1. Аналіз PowerUserAccess (занадто broad)
2. Create custom policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "ec2:*",
           "s3:*",
           "rds:*",
           "cloudwatch:*"
         ],
         "Resource": "*",
         "Condition": {
           "StringEquals": {
             "aws:RequestedRegion": "eu-west-1"
           }
         }
       }
     ]
   }
   ```
3. Replace PowerUserAccess з custom policy
4. Test - переконатися що IAM, billing тощо denied

ЧАСТИНА 4: MFA for IAM User
1. Enable MFA for IAM user
2. Test login
3. Enforce MFA policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Effect": "Deny",
       "Action": "*",
       "Resource": "*",
       "Condition": {
         "BoolIfExists": {"aws:MultiFactorAuthPresent": "false"}
       }
     }]
   }
   ```

ЧАСТИНА 5: Git Documentation
1. Create repository
2. Document all policies
3. Screenshot configurations
4. Commit and push

SECURITY BEST PRACTICES:
- Never share access keys
- Rotate keys regularly
- Use roles over users де можливо
- Enable CloudTrail
- Review IAM periodically
- Delete unused users/keys

TROUBLESHOOTING:
- Locked out? Use root to restore
- MFA device lost? Contact AWS support
- Policy deny? Check CloudTrail logs

DELIVERABLES:
- Screenshot root account with MFA
- IAM users/groups configuration
- Custom policy JSON
- Git repository з documentation

Обсяг: 3000-3500 слів з прикладами
```

### Лабораторна робота 2: Networking + EC2

**Промпт для ШІ:**
```
Створи step-by-step інструкції для лабораторної "Networking + EC2".

OBJECTIVE:
Створити VPC та запустити EC2 instance з Linux, налаштувати SSH access.

АРХІТЕКТУРА:
- VPC: 10.0.0.0/16
- Public Subnet: 10.0.1.0/24 (eu-west-1a)
- Internet Gateway
- EC2 t3.micro з Amazon Linux 2
- Security Group для SSH
- Elastic IP (optional)

КРОК ЗА КРОКОМ:

ЧАСТИНА 1: VPC Creation
1. VPC Console → Create VPC
   - Name: "Lab-VPC"
   - CIDR: 10.0.0.0/16
   - No IPv6
   - Default tenancy
2. Verify VPC created
   - Note VPC ID
   - Check default route table
   - Check default NACL

ЧАСТИНА 2: Public Subnet
1. Create Subnet
   - VPC: Lab-VPC
   - Name: "Lab-Public-Subnet"
   - AZ: eu-west-1a
   - CIDR: 10.0.1.0/24
2. Enable auto-assign public IP
   - Subnet settings
   - Enable auto-assign IPv4

ЧАСТИНА 3: Internet Gateway
1. Create IGW
   - Name: "Lab-IGW"
2. Attach to VPC
   - Actions → Attach to VPC
   - Select Lab-VPC

ЧАСТИНА 4: Route Table
1. Create route table
   - Name: "Lab-Public-RT"
   - VPC: Lab-VPC
2. Add route to IGW
   - 0.0.0.0/0 → Lab-IGW
3. Associate with subnet
   - Subnet associations
   - Lab-Public-Subnet

ЧАСТИНА 5: Security Group
1. Create Security Group
   - Name: "Lab-SSH-SG"
   - VPC: Lab-VPC
   - Inbound rule:
     - Type: SSH
     - Port: 22
     - Source: My IP (find via whatismyip.com)
   - Outbound: All traffic (default)

ЧАСТИНА 6: Key Pair
1. EC2 Console → Key Pairs
2. Create key pair
   - Name: "lab-key"
   - Format: .pem (Linux/Mac) або .ppk (Windows)
3. Download and save
4. chmod 400 lab-key.pem (Linux/Mac)

ЧАСТИНА 7: Launch EC2
1. EC2 Console → Launch Instance
2. Choose AMI: Amazon Linux 2
3. Instance type: t3.micro (Free Tier)
4. Network: Lab-VPC
5. Subnet: Lab-Public-Subnet
6. Auto-assign Public IP: Enable
7. Security Group: Lab-SSH-SG
8. Key pair: lab-key
9. Launch!

ЧАСТИНА 8: Connect via SSH
1. Wait for instance running
2. Copy Public IPv4 address
3. SSH command:
   ```bash
   ssh -i lab-key.pem ec2-user@<PUBLIC-IP>
   ```
4. Accept fingerprint
5. You're in!

ЧАСТИНА 9: Linux Commands Practice
```bash
# System info
uname -a
cat /etc/os-release

# Update packages
sudo yum update -y

# Install nginx
sudo yum install nginx -y

# Start nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx

# Verify
curl localhost

# Allow HTTP in Security Group
# Add rule: HTTP (80) from 0.0.0.0/0

# Test from browser
http://<PUBLIC-IP>
```

ЧАСТИНА 10: Cleanup
1. Terminate instance
2. Delete Security Group
3. Delete route table association
4. Delete route table
5. Detach and delete IGW
6. Delete subnet
7. Delete VPC

TROUBLESHOOTING:
- Can't SSH? Check:
  - Security Group has your IP
  - Instance has public IP
  - Key permissions (400)
  - Correct username (ec2-user)
- Timeout? Check route table
- Permission denied? Check key

DELIVERABLES:
- VPC diagram
- Screenshot EC2 running
- Screenshot SSH session
- Screenshot nginx working
- Git commit з documentation

Обсяг: 3500-4000 слів
```

### Лабораторна робота 3: Storage + Database

**Промпт для ШІ:**
```
Створи інструкції для лабораторної "Storage + Database".

OBJECTIVES:
1. S3 bucket для static website
2. RDS MySQL database
3. Backup configuration

ЧАСТИНА 1: S3 Static Website

1. Create S3 Bucket
   - Unique name: "lab-website-[yourname]-[random]"
   - Region: eu-west-1
   - Unblock public access
   - Versioning: Enable
   
2. Upload Website Files
   - index.html:
   ```html
   <!DOCTYPE html>
   <html>
   <head><title>My Cloud Lab</title></head>
   <body>
     <h1>Hello from S3!</h1>
     <p>This is static website hosting.</p>
   </body>
   </html>
   ```
   - error.html
   - styles.css (optional)

3. Enable Static Website Hosting
   - Properties → Static website hosting
   - Enable
   - Index: index.html
   - Error: error.html
   - Note endpoint URL

4. Bucket Policy для Public Read
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Sid": "PublicReadGetObject",
       "Effect": "Allow",
       "Principal": "*",
       "Action": "s3:GetObject",
       "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
     }]
   }
   ```

5. Test Website
   - Open endpoint URL
   - Verify content

6. S3 Lifecycle Policy
   - Transition to Standard-IA після 30 днів
   - Transition to Glacier після 90 днів
   - Expiration після 365 днів

ЧАСТИНА 2: RDS MySQL Database

1. Create DB Subnet Group
   - VPC: Lab-VPC (from Lab 2, або create new)
   - Subnets: min 2 AZs
   - Private subnets preferred

2. Security Group для Database
   - Inbound: MySQL (3306)
   - Source: SG з EC2 (або your IP for testing)

3. Launch RDS Instance
   - Engine: MySQL 8.0
   - Template: Free Tier (db.t3.micro)
   - DB instance identifier: "lab-database"
   - Master username: admin
   - Master password: [secure password]
   - VPC: Lab-VPC
   - Subnet group: Created above
   - Public access: No
   - Security group: DB-SG
   - Initial database: "labdb"

4. Wait for Creation (5-10 minutes)

5. Connect from EC2
   - SSH to EC2 (from Lab 2)
   - Install MySQL client:
   ```bash
   sudo yum install mysql -y
   ```
   - Connect:
   ```bash
   mysql -h <RDS-ENDPOINT> -u admin -p
   ```
   - Enter password

6. Basic SQL Commands
   ```sql
   SHOW DATABASES;
   USE labdb;
   
   CREATE TABLE users (
     id INT AUTO_INCREMENT PRIMARY KEY,
     name VARCHAR(100),
     email VARCHAR(100)
   );
   
   INSERT INTO users (name, email) VALUES 
     ('John Doe', 'john@example.com'),
     ('Jane Smith', 'jane@example.com');
   
   SELECT * FROM users;
   ```

ЧАСТИНА 3: Backup Configuration

1. RDS Automated Backups
   - Verify enabled (default)
   - Retention period: 7 days
   - Backup window: preferred time

2. Manual Snapshot
   - Actions → Take snapshot
   - Name: "lab-db-snapshot-1"
   - Wait for completion

3. S3 Versioning Test
   - Upload file v1
   - Modify and upload v2
   - View versions
   - Restore v1

CLEANUP:
1. Delete RDS snapshot
2. Delete RDS instance (no final snapshot for lab)
3. Empty S3 bucket
4. Delete S3 bucket
5. Delete subnet group
6. Delete security groups

DELIVERABLES:
- S3 website URL
- Screenshot working website
- RDS connection screenshot
- SQL query results
- Architecture diagram
- Git documentation

SECURITY CONSIDERATIONS:
- Never expose RDS publicly
- Use strong passwords
- Enable encryption at rest
- Use SSL/TLS for connections
- Regular backup testing

Обсяг: 3000-3500 слів
```

### Лабораторна робота 4: Highly Available Web Architecture

**Промпт для ШІ:**
```
Створи інструкції для лабораторної "Highly Available Web Architecture".

FINAL ARCHITECTURE:
- VPC з 2 public subnets (різні AZs)
- Application Load Balancer
- Auto Scaling Group (2-4 instances)
- Launch Template з User Data
- CloudWatch моніторинг
- Scaling policies

STEP-BY-STEP:

ЧАСТИНА 1: VPC Setup (Multi-AZ)
1. VPC: 10.0.0.0/16
2. Public Subnet 1: 10.0.1.0/24 (eu-west-1a)
3. Public Subnet 2: 10.0.2.0/24 (eu-west-1b)
4. Internet Gateway
5. Route Table для обох subnets

ЧАСТИНА 2: Security Groups
1. ALB-SG:
   - HTTP (80) from 0.0.0.0/0
   - HTTPS (443) from 0.0.0.0/0
2. Web-SG:
   - HTTP (80) from ALB-SG
   - SSH (22) from My IP

ЧАСТИНА 3: Launch Template
1. Create Launch Template
   - Name: "Web-Server-Template"
   - AMI: Amazon Linux 2
   - Instance type: t3.micro
   - Security group: Web-SG
   - User Data:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   
   INSTANCE_ID=$(ec2-metadata --instance-id | cut -d " " -f 2)
   AZ=$(ec2-metadata --availability-zone | cut -d " " -f 2)
   
   cat <<EOF > /var/www/html/index.html
   <!DOCTYPE html>
   <html>
   <head><title>HA Web App</title></head>
   <body style="font-family: Arial; text-align: center; padding: 50px;">
     <h1>Hello from Highly Available Web App!</h1>
     <p><strong>Instance ID:</strong> $INSTANCE_ID</p>
     <p><strong>Availability Zone:</strong> $AZ</p>
     <p><strong>Hostname:</strong> $(hostname)</p>
   </body>
   </html>
   EOF
   
   systemctl start httpd
   systemctl enable httpd
   ```

ЧАСТИНА 4: Target Group
1. Create Target Group
   - Name: "Web-TG"
   - Protocol: HTTP
   - Port: 80
   - VPC: Lab-VPC
   - Health check path: /
   - Health check interval: 30s
   - Healthy threshold: 2
   - Unhealthy threshold: 2

ЧАСТИНА 5: Application Load Balancer
1. Create ALB
   - Name: "Web-ALB"
   - Scheme: Internet-facing
   - IP address type: IPv4
   - VPC: Lab-VPC
   - Subnets: Both public subnets
   - Security group: ALB-SG
2. Configure Listener
   - Protocol: HTTP
   - Port: 80
   - Forward to: Web-TG
3. Note ALB DNS name

ЧАСТИНА 6: Auto Scaling Group
1. Create ASG
   - Name: "Web-ASG"
   - Launch template: Web-Server-Template
   - VPC: Lab-VPC
   - Subnets: Both public subnets
   - Load balancer: Web-TG
   - Health check type: ELB
   - Health check grace period: 300s
2. Group size:
   - Desired: 2
   - Minimum: 2
   - Maximum: 4
3. Scaling policies:
   - Target tracking:
     - Metric: Average CPU
     - Target: 50%
     - Warmup: 60s

ЧАСТИНА 7: Testing
1. Wait for instances (3-5 min)
2. Check Target Group health
   - Both targets healthy
3. Test ALB URL
   - Refresh multiple times
   - See different instance IDs
   - Confirms load balancing

ЧАСТИНА 8: CloudWatch Monitoring
1. Create Dashboard
   - Add widgets:
     - ALB request count
     - Target healthy count
     - ASG CPU utilization
     - ASG instance count
2. Create Alarms
   - UnhealthyHostCount > 0
   - CPUUtilization > 80%
3. SNS topic для notifications

ЧАСТИНА 9: Load Testing
1. Install Apache Bench:
   ```bash
   sudo yum install httpd-tools -y
   ```
2. Generate load:
   ```bash
   ab -n 10000 -c 100 http://<ALB-DNS>/
   ```
3. Observe:
   - CloudWatch CPU spike
   - ASG scaling out
   - New instances joining

ЧАСТИНА 10: Simulate Failure
1. Terminate one instance manually
2. Observe:
   - Auto Scaling launches replacement
   - Traffic continues (no downtime)
   - Target group removes unhealthy target
3. Resilience demonstrated!

CLEANUP:
1. Delete ASG (will terminate instances)
2. Delete ALB
3. Delete Target Group
4. Delete Launch Template
5. Delete CloudWatch alarms
6. Delete SNS topics
7. Delete Security Groups
8. Clean VPC resources

DELIVERABLES:
- Architecture diagram
- Screenshot ALB working
- Screenshot different instance IDs
- CloudWatch dashboard screenshot
- Load test results
- Scaling activity log
- Git documentation

KEY LEARNINGS:
- High Availability через Multi-AZ
- Load balancing distributes traffic
- Auto Scaling handles demand
- Health checks detect failures
- Monitoring provides visibility

TROUBLESHOOTING:
- Targets unhealthy? Check:
  - Security Groups
  - Health check path
  - Web server running
- ALB timeout? Check route tables
- Scaling not working? Check policies

Обсяг: 4000-4500 слів
```

---

## Додаткові компоненти

### Homework Assignments

Для кожного ДЗ (вже створені README, тут додаткові промпти якщо потрібно):

1. **ДЗ1 (Git)**: Evaluation rubric та приклади good practices
2. **ДЗ2 (Cloud Architecture)**: Приклади case studies
3. **ДЗ3 (DevSecOps mindset)**: Список рекомендованих incidents для аналізу
4. **ДЗ4 (FinOps)**: Templates для cost analysis

### Final Project

Детальний scoring rubric та milestone templates.

---

## Формат слайдів (reveal.js)

```markdown
---
title: Назва Лекції
theme: black
highlightTheme: monokai
---

# Назва Лекції
## Підзаголовок

DevSecOps & Cloud Technologies

---

## Agenda

- Теорія концепції
- AWS реалізація
- Практичні сценарії
- Security considerations
- Best practices

---

## Ключова Концепція

![Diagram](diagram.png)

**Key Point:** Пояснення

---

## AWS Приклад

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Environment,Values=Production"
```

---

## 💡 Best Practice

> Always enable MFA for all accounts

---

## ⚠️ Common Mistake

**Anti-pattern:** Leaving S3 buckets public

**Solution:** Bucket policies + Block Public Access

---

## Summary

- Takeaway 1
- Takeaway 2
- Takeaway 3

**Next:** [Next lecture topic]

---

## Resources

- [AWS Documentation](https://docs.aws.amazon.com/)
- [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

---

# Questions?
```

---

## Загальні рекомендації для промптів ШІ

1. **Контекст**: Курс для підготовки до Junior Cloud/DevOps ролей
2. **Мова**: Українська з технічними термінами
3. **Баланс**: 50% теорія + 50% практика
4. **DevSecOps**: Security на кожному етапі
5. **Real-world**: Використовувати реальні кейси та сценарії
6. **Hands-on**: Практичні приклади з кодом
7. **Діаграми**: Використовувати Mermaid для візуалізації
8. **AWS-specific**: Конкретні сервіси та features
9. **Cost-aware**: Згадувати фінансові implications
10. **Industry practices**: Використовувати індустріальні стандарти

---

## Черговість генерації

1. Лекції 1-10 (notes.md)
2. Слайди для лекцій 1-10
3. Лабораторні роботи 1-4 (instructions)
4. Практичні роботи 1-2 (instructions)
5. Додаткові матеріали для ДЗ
6. Final project templates

---

## Перевірка якості

- [ ] Практична спрямованість (не тільки теорія)
- [ ] DevSecOps підхід інтегровано
- [ ] Українська мова + технічні терміни
- [ ] AWS-specific інформація актуальна
- [ ] Діаграми та візуалізації присутні
- [ ] Hands-on приклади включені
- [ ] Security considerations на місці
- [ ] Cost optimization згадано
- [ ] Real-world scenarios
- [ ] Логічна структура та flow
