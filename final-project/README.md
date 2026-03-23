# Фінальний проєкт курсу

## Тема

**Спроєктувати хмарну інформаційну систему та захистити архітектуру**

## Мета

Продемонструвати комплексне розуміння хмарних технологій, DevSecOps практик та здатність проектувати production-ready системи.

## Опис проєкту

Студенти (індивідуально або в команді до 3 осіб) мають спроєктувати повноцінну хмарну інформаційну систему для обраного кейсу з урахуванням всіх аспектів, вивчених у курсі.

## Етапи проєкту

### 1. Вибір кейсу та аналіз вимог (1 тиждень)

Вибрати один з запропонованих кейсів або запропонувати власний:
- **E-commerce платформа**: онлайн магазин з каталогом, корзиною, оплатою
- **SaaS B2B система**: CRM, project management, або подібне
- **Data analytics platform**: збір, обробка, візуалізація даних
- **IoT backend**: обробка даних з сенсорів/пристроїв
- **Social media application**: контент, коментарі, real-time features
- **Власний кейс**: потребує узгодження з викладачем

**Deliverable:** Документ з описом вимог (functional, non-functional)

### 2. Проектування архітектури (2 тижні)

Створити детальну архітектуру системи:

**Обов'язкові компоненти:**
- Compute layer (EC2, containers, або serverless)
- Storage strategy (S3, EBS, EFS)
- Database solution (RDS, DynamoDB, або обидва)
- Network architecture (VPC, subnets, routing)
- Security implementation (IAM, Security Groups, encryption)
- Scalability & High Availability (ALB, ASG, Multi-AZ)
- Monitoring & Logging (CloudWatch)

**Deliverable:** 
- Architecture diagram (draw.io, Lucidchart, або CloudCraft)
- Опис кожного компоненту
- Data flow diagrams

### 3. DevSecOps pipeline (1 тиждень)

Спроектувати CI/CD pipeline та security practices:
- Git workflow
- CI/CD stages (build, test, deploy)
- Security scanning points
- Environment separation (Dev/Staging/Prod)
- Secrets management
- Rollback strategy

**Deliverable:** Pipeline diagram з поясненням

### 4. Безпека та Compliance (1 тиждень)

Описати security measures:
- Threat modeling
- Security controls для кожного layer
- Compliance considerations (GDPR, PCI-DSS, тощо)
- Incident response plan (basic)

**Deliverable:** Security documentation

### 5. FinOps та оптимізація (1 тиждень)

Економічний аналіз:
- Cost estimation (AWS Pricing Calculator)
- Cost optimization strategy
- Scaling plan та його вплив на вартість
- Monitoring and alerting for costs

**Deliverable:** Cost analysis spreadsheet

### 6. Підготовка до захисту (1 тиждень)

- Git репозиторій з повною документацією
- Презентація (15-20 слайдів)
- Demo або walkthrough архітектури

## Структура Git репозиторію

```
project-name/
├── README.md                 # Overview проєкту
├── docs/
│   ├── requirements.md       # Функціональні та нефункціональні вимоги
│   ├── architecture.md       # Детальний опис архітектури
│   ├── security.md          # Security documentation
│   ├── devops.md            # CI/CD та DevOps practices
│   └── finops.md            # Cost analysis
├── diagrams/
│   ├── architecture.png     # Головна архітектурна діаграма
│   ├── network.png          # Мережева діаграма
│   ├── pipeline.png         # CI/CD pipeline
│   └── data-flow.png        # Data flow diagrams
├── infrastructure/          # IaC код (опціонально, бонус)
│   └── terraform/           # Terraform конфігурація
└── presentation/
    └── slides.pdf           # Презентація для захисту
```

## Критерії оцінювання

### Архітектурне мислення (30%)
- Правильний вибір сервісів для задачі
- Врахування trade-offs
- Масштабованість та гнучкість рішення
- Best practices та патерни

### Безпека (25%)
- Comprehensive security approach
- IAM та least privilege
- Network security
- Data protection
- Threat modeling

### Масштабування (15%)
- High Availability design
- Auto Scaling strategy
- Load balancing
- Multi-AZ або Multi-Region (де доречно)

### Вартість (10%)
- Реалістична оцінка
- Cost optimization
- Trade-offs між вартістю та якістю

### Git-документація (10%)
- Повнота документації
- Якість діаграм
- Структурованість
- README як точка входу

### Презентація та захист (10%)
- Якість презентації
- Вміння пояснити рішення
- Відповіді на питання
- Професійність

## Формат захисту

- **Тривалість:** 15-20 хвилин презентація + 10-15 хвилин питання
- **Формат:** очно або онлайн
- **Команди:** всі члени команди мають брати участь

### Структура презентації

1. Вступ: кейс та вимоги (2-3 хв)
2. Архітектура: огляд та key decisions (5-7 хв)
3. Security approach (3-4 хв)
4. DevOps та CI/CD (2-3 хв)
5. Scalability та HA (2-3 хв)
6. Cost analysis (2 хв)
7. Висновки та lessons learned (1-2 хв)

## Терміни

- **Реєстрація теми:** [дата]
- **Milestone 1 (Requirements):** [дата]
- **Milestone 2 (Architecture):** [дата]
- **Фінальна здача:** [дата]
- **Захист:** [період]

## Додаткові бали (бонус)

- Реалізація Infrastructure as Code (Terraform) - до 10%
- Створення Working Proof of Concept - до 15%
- Особливо креативне або складне рішення - до 10%
- Contribution до open-source інструментів - до 5%

## Корисні ресурси

- [AWS Solutions Library](https://aws.amazon.com/solutions/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)
- [Terraform AWS Modules](https://registry.terraform.io/browse/modules?provider=aws)

## Консультації

Викладач доступний для консультацій протягом виконання проєкту. Рекомендовано показувати проміжні результати на milestone етапах.

---

**Успіхів у виконанні проєкту! 🚀**
