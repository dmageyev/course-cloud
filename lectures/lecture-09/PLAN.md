# PLAN.md — Лекція 9: Cloud-native та контейнеризація

**Курс:** Хмарні технології інформаційних систем
**Аудиторія:** Бакалавр ІТ · 3 курс
**Тривалість:** 2 академічні години (90 хв)

---

## Мета лекції

Студенти повинні вміти:
- Розуміти cloud-native принципи та філософію
- Пояснювати переваги та недоліки мікросервісної архітектури
- Розрізняти контейнери та віртуальні машини
- Розуміти концепції Docker та базові поняття контейнеризації
- Розуміти ідею Kubernetes для оркестрації контейнерів
- Застосовувати event-driven підходи в архітектурі
- Розбиратися в AWS сервісах: ECS, EKS, Lambda

---

## Структура лекції (40 слайдів)

### Блок 1 — Вступ (слайди 1–3) · ~5 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 1 | Титульний | Назва, курс, аудиторія |
| 2 | Agenda | Огляд тем лекції |
| 3 | Еволюція архітектури | Monolith → SOA → Microservices → Cloud-native |

### Блок 2 — Cloud-native Principles (слайди 4–9) · ~15 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 4 | Cloud-native — визначення | CNCF definition, характеристики |
| 5 | 12-Factor App | Основні принципи (codebase, dependencies, config, backing services) |
| 6 | Cloud-native patterns | API-first, Stateless, Disposability, Dev/Prod parity |
| 7 | Immutable Infrastructure | Що це, навіщо, як реалізувати |
| 8 | Observability | Metrics, Logs, Traces (три стовпи) |
| 9 | Cloud-native vs Traditional | Порівняльна таблиця, переваги та виклики |

### Блок 3 — Microservices (слайди 10–16) · ~18 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 10 | Microservices — що це? | Визначення, характеристики, bounded context |
| 11 | Monolith vs Microservices | Порівняння, коли що використовувати |
| 12 | Переваги Microservices | Independent deployment, tech diversity, scalability |
| 13 | Недоліки Microservices | Distributed complexity, network latency, data consistency |
| 14 | Communication patterns | Sync (REST, gRPC) vs Async (Message queue, Events) |
| 15 | Service Discovery | Що це, як працює (Consul, Eureka, K8s Service) |
| 16 | API Gateway pattern | Єдина точка входу, aggregation, authentication |

### Блок 4 — Containers (слайди 17–22) · ~15 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 17 | Containers — навіщо? | Isolation, portability, efficiency |
| 18 | VM vs Container | Порівняння архітектури, ресурси, startup time |
| 19 | Container anatomy | Namespaces, cgroups, Union FS, layers |
| 20 | Container image | Що це, layers, immutability, registries |
| 21 | Container lifecycle | Build → Ship → Run → Stop → Remove |
| 22 | Best practices | Small images, one process, non-root user, health checks |

### Блок 5 — Docker (слайди 23–27) · ~12 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 23 | Docker — що це? | Platform для контейнеризації, Docker Engine architecture |
| 24 | Dockerfile | Структура, основні інструкції (FROM, RUN, COPY, CMD, ENTRYPOINT) |
| 25 | Docker image layers | Layer caching, optimization strategies |
| 26 | Docker networking | Bridge, host, overlay networks |
| 27 | Docker volumes | Persistence, data sharing between containers |

### Блок 6 — Kubernetes (слайди 28–33) · ~15 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 28 | Kubernetes — навіщо? | Container orchestration, problems it solves |
| 29 | K8s Architecture | Control Plane (API Server, Scheduler, etcd) + Worker Nodes |
| 30 | K8s основні об'єкти | Pod, Deployment, Service, ConfigMap, Secret |
| 31 | Pod lifecycle | Pending → Running → Succeeded/Failed |
| 32 | Service types | ClusterIP, NodePort, LoadBalancer, Ingress |
| 33 | K8s scaling & healing | HPA, self-healing, rolling updates |

### Блок 7 — Event-driven Systems (слайди 34–36) · ~8 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 34 | Event-driven — концепція | Events, producers, consumers, event bus |
| 35 | Event-driven patterns | Pub/Sub, Event Sourcing, CQRS |
| 36 | Переваги та виклики | Decoupling, scalability vs eventual consistency, debugging |

### Блок 8 — AWS Services (слайди 37–39) · ~5 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 37 | AWS ECS | Elastic Container Service, Fargate, Task Definitions |
| 38 | AWS EKS | Elastic Kubernetes Service, managed K8s, integrations |
| 39 | AWS Lambda | Serverless, FaaS, use cases, limitations |

### Блок 9 — Підсумок (слайд 40) · ~2 хв
| # | Слайд | Зміст |
|---|-------|-------|
| 40 | Ключові висновки та ресурси | Чеклист знань, посилання для поглибленого вивчення |

---

## Ключові поняття

| Термін | Визначення |
|--------|-----------|
| Cloud-native | Підхід до розробки, який використовує переваги cloud computing (containers, microservices, orchestration) |
| 12-Factor App | Методологія для побудови SaaS додатків (codebase, dependencies, config, etc.) |
| Microservices | Архітектурний стиль: додаток як набір малих незалежних сервісів |
| Container | Ізольоване середовище для запуску додатків з власною ФС, але спільним ядром |
| Docker | Платформа для розробки, доставки та запуску контейнерізованих додатків |
| Kubernetes (K8s) | Система оркестрації контейнерів (automated deployment, scaling, management) |
| Pod | Найменша одиниця в K8s, може містити один або більше контейнерів |
| Orchestration | Автоматизація deployment, scaling, networking контейнерів |
| Event-driven | Архітектурний паттерн, де компоненти спілкуються через події (events) |
| Serverless | Модель виконання, де cloud provider керує інфраструктурою (FaaS) |
| ECS | AWS Elastic Container Service — managed container orchestration |
| EKS | AWS Elastic Kubernetes Service — managed Kubernetes |
| Lambda | AWS serverless compute service (Function-as-a-Service) |

---

## Рекомендована підготовка

- Пригадати основи Linux (processes, filesystem)
- Переглянути: [The Twelve-Factor App](https://12factor.net/)
- Прочитати: [CNCF Cloud Native Definition](https://github.com/cncf/toc/blob/main/DEFINITION.md)
- Ознайомитись: [Docker Overview](https://docs.docker.com/get-started/overview/)

---

## Практичні навички (для самостійної роботи)

Після лекції студенти можуть самостійно:
1. Створити простий Dockerfile для Node.js/Python додатку
2. Запустити контейнер локально з Docker
3. Розгорнути multi-container додаток з docker-compose
4. Створити Task Definition в AWS ECS
5. Розгорнути Lambda функцію через AWS Console

---

## Посилання

**Cloud-native:**
- CNCF Landscape: <https://landscape.cncf.io/>
- Cloud Native Patterns (book) by Cornelia Davis
- AWS Well-Architected Framework

**Containers & Docker:**
- Docker Documentation: <https://docs.docker.com/>
- "Docker Deep Dive" by Nigel Poulton
- Container Best Practices: <https://cloud.google.com/architecture/best-practices-for-building-containers>

**Kubernetes:**
- Kubernetes Documentation: <https://kubernetes.io/docs/>
- "Kubernetes in Action" by Marko Lukša
- Kubernetes Patterns: <https://www.redhat.com/en/resources/oreilly-kubernetes-patterns-ebook>

**Microservices:**
- "Building Microservices" by Sam Newman
- Martin Fowler — Microservices: <https://martinfowler.com/articles/microservices.html>
- Microservices Patterns: <https://microservices.io/patterns/>

**AWS:**
- AWS ECS Documentation: <https://docs.aws.amazon.com/ecs/>
- AWS EKS Best Practices: <https://aws.github.io/aws-eks-best-practices/>
- AWS Lambda Documentation: <https://docs.aws.amazon.com/lambda/>

---

## Зв'язок з іншими лекціями

- **Лекція 8 (Scaling & HA):** Auto Scaling застосовується до containers (ECS, K8s HPA)
- **Лекція 7 (Databases):** Microservices often use database-per-service pattern
- **Лекція 10 (Monitoring):** Observability критична для distributed systems
- **Лекція 11 (Security):** Container security, secrets management
