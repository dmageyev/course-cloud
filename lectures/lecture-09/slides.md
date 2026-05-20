# Лекція 9: Cloud-native та контейнеризація — Слайди

---

## Slide 1: Назва (Title)

# Лекція 9: Cloud-native та контейнеризація
**Курс:** Хмарні технології інформаційних систем
Бакалавр ІТ · 3 курс · 2 години

---

## Slide 2: Agenda

1. Еволюція архітектури (Monolith → Cloud-native)
2. Cloud-native principles
3. 12-Factor App
4. Microservices архітектура
5. Containers — що це та навіщо?
6. Docker концепція та використання
7. Kubernetes основи (idea, no practice)
8. Event-driven systems
9. AWS: ECS, EKS, Lambda
10. Підсумки

---

## Slide 3: Еволюція архітектури

**Шлях до Cloud-native:**

```
1990s: MONOLITH
  [Single big application]
  ❌ Scaling = scale everything
  ❌ Update = redeploy everything

2000s: SOA (Service-Oriented Architecture)
  [Services + ESB]
  ⚠️ Still coupled through ESB

2010s: MICROSERVICES
  [Independent services]
  ✓ Independent deployment
  ⚠️ Complexity

2015+: CLOUD-NATIVE
  [Microservices + Containers + Orchestration + DevOps]
  ✓ Full automation
  ✓ Resilience
  ✓ Scalability
```

---

## Slide 4: Cloud-native — визначення

**CNCF (Cloud Native Computing Foundation) definition:**

> Cloud-native technologies empower organizations to build and run **scalable applications** in modern, **dynamic environments** such as public, private, and hybrid clouds.

**Характеристики:**
- 🐳 **Containers** — portable, isolated runtime
- 🔄 **Microservices** — loosely coupled services
- 🎯 **Orchestration** — automated deployment & scaling (Kubernetes, ECS)
- 📊 **Observability** — metrics, logs, traces
- 🚀 **DevOps & CI/CD** — automation first
- ♻️ **Resilience** — design for failure

---

## Slide 5: 12-Factor App

**Методологія для побудови SaaS-додатків:**

```
I. Codebase
   Один codebase в version control, багато deploys

II. Dependencies
   Явне оголошення (package.json, requirements.txt)

III. Config
   Зберігай в environment variables, не в коді

IV. Backing services
   Treat as attached resources (DB, cache, queue)

V. Build, release, run
   Strict separation of stages

VI. Processes
   Stateless! Store state in backing services
```

(продовження: 7. Port binding, 8. Concurrency, 9. Disposability, 10. Dev/prod parity, 11. Logs, 12. Admin processes)

---

## Slide 6: Cloud-native patterns

**Основні паттерни:**

| Pattern | Що це | Приклад |
|---------|-------|---------|
| **API-first** | Всі компоненти через REST/gRPC API | Microservices communication |
| **Stateless** | Не зберігай стан локально | Session в Redis, не в memory |
| **Disposability** | Fast startup + graceful shutdown | SIGTERM handling |
| **Immutable Infrastructure** | Never update, always replace | New container image, not patch |
| **Declarative** | Describe desired state, not steps | K8s manifests, Terraform |

---

## Slide 7: Immutable Infrastructure

**Що це?**

Серверна інфраструктура, яка **ніколи не змінюється** після deployment. Замість update → replace.

```
Traditional (Mutable):
  Server 1.0 → apt update → patch → Server 1.1
  ❌ Configuration drift
  ❌ "Works on my machine"

Immutable:
  Server 1.0 → Deploy Server 1.1 (new image) → Terminate Server 1.0
  ✓ Consistent environments
  ✓ Easy rollback
  ✓ No snowflakes
```

**Реалізація:** Docker images, AMI, golden images

---

## Slide 8: Observability

**Три стовпи (Three Pillars):**

```
1. METRICS (що відбувається?)
   CPU, Memory, Request rate, Latency
   → Prometheus, CloudWatch

2. LOGS (деталі події)
   Application logs, error logs
   → ELK Stack, CloudWatch Logs

3. TRACES (де час витрачається?)
   Distributed tracing через microservices
   → Jaeger, X-Ray, OpenTelemetry
```

**Разом = повна картина системи**

---

## Slide 9: Cloud-native vs Traditional

| | Traditional | Cloud-native |
|---|-------------|--------------|
| **Architecture** | Monolith | Microservices |
| **Deployment** | VM, bare metal | Containers |
| **Scaling** | Vertical (bigger VM) | Horizontal (more containers) |
| **Updates** | Downtime, manual | Zero-downtime, automated |
| **Recovery** | Manual intervention | Self-healing |
| **Infrastructure** | Mutable (patch servers) | Immutable (replace) |
| **Cost** | Fixed capacity | Pay-per-use |

**Виклики Cloud-native:** Складність, навчання команди, debugging distributed systems

---

## Slide 10: Microservices — що це?

**Визначення:**

Архітектурний стиль, де додаток будується як набір **малих, незалежних сервісів**, кожен:
- Працює у власному процесі
- Спілкується через lightweight protocols (HTTP, gRPC, messaging)
- Deployиться незалежно
- Організований навколо business capabilities
- Може мати власну базу даних

**Bounded Context (DDD):**
Кожен microservice = один bounded context

```
Order Service → Orders DB
Payment Service → Payments DB
Inventory Service → Inventory DB
```

---

## Slide 11: Monolith vs Microservices

| | Monolith | Microservices |
|---|----------|---------------|
| **Deployment** | Весь додаток разом | Кожен сервіс окремо |
| **Scaling** | Scale all (навіть якщо навантаження на одну частину) | Scale тільки потрібні сервіси |
| **Database** | Спільна БД | Database per service |
| **Team** | Одна велика команда | Маленькі команди ("pizza team" 6-8 осіб) |
| **Technology** | Один stack | Tech diversity (Java, Python, Go) |
| **Complexity** | Operational (легко) | Distributed (складно) |
| **Testing** | Integration легко | E2E складно |
| **Initial cost** | Низький | Високий |

**Коли Monolith?** Startup, невелика команда, uncertrain domain

**Коли Microservices?** Scale, large teams, mature domain

---

## Slide 12: Переваги Microservices

```
✅ Independent Deployment
   Оновлюй Payment Service без торкання Orders

✅ Technology Diversity
   User Service = Node.js
   Analytics Service = Python (ML libs)
   Legacy Service = Java

✅ Fine-grained Scaling
   Black Friday → scale Order Service x10
   → Payment Service x5
   → Catalog Service x2

✅ Fault Isolation
   Recommendation Service down →
   Core shopping still works

✅ Team Autonomy
   Кожна команда відповідає за свій сервіс
```

---

## Slide 13: Недоліки Microservices

```
❌ Distributed System Complexity
   Network calls, latency, partial failures

❌ Data Consistency
   No distributed transactions
   Eventual consistency only

❌ Testing Complexity
   End-to-end testing складне

❌ Operational Overhead
   More services = more to monitor, deploy, manage

❌ Network Overhead
   Internal calls через мережу (vs in-memory)

❌ Versioning & Compatibility
   Backward compatibility API важлива
```

**Рекомендація:** Почни з monolith, переходь на microservices коли є потреба і команда готова.

---

## Slide 14: Communication Patterns

**Synchronous (Sync):**

```
Client → REST API → Service B
         (wait)

Client → gRPC → Service B
         (wait)

✓ Simple, immediate response
❌ Coupling, cascading failures
```

**Asynchronous (Async):**

```
Service A → Message Queue → Service B
            (non-blocking)

Service A → Event Bus → Service B, C, D
            (pub/sub)

✓ Decoupling, resilience
❌ Complexity, eventual consistency
```

**Hybrid:** Sync для critical paths, Async для everything else.

---

## Slide 15: Service Discovery

**Проблема:**
Microservice A хоче викликати Microservice B. Де B? IP:port?

```
Hard-coded:
  http://payment-service:8080
  ❌ Що якщо IP змінюється? Scale out?

Service Discovery:
  1. Payment Service registers itself
     → Service Registry (Consul, Eureka)
  2. Order Service asks Registry
     "Де Payment Service?"
  3. Registry returns: [10.0.1.5:8080, 10.0.1.6:8080]
```

**Kubernetes:** Built-in Service Discovery через DNS

---

## Slide 16: API Gateway Pattern

**Проблема:**
Клієнт (mobile app) потрібно викликати 5 microservices → 5 HTTP calls → slow!

```
Mobile App → API Gateway → [Auth Service]
                        ↘ [User Service]
                        ↘ [Orders Service]
                        ↘ [Payments Service]
                        ↘ [Catalog Service]
```

**API Gateway робота:**
- ✓ Єдина точка входу (Single entry point)
- ✓ Request aggregation (1 call замість 5)
- ✓ Authentication & Authorization
- ✓ Rate limiting
- ✓ Caching

**AWS:** Amazon API Gateway, ALB

---

## Slide 17: Containers — навіщо?

**Проблема традиційного deployment:**

```
Dev: "It works on my machine!"
Ops: "But not on production..."

Причини:
- Different OS versions
- Different library versions
- Missing dependencies
```

**Container = рішення:**

```
📦 Container = application + all dependencies
   ✓ Same environment (dev, test, prod)
   ✓ Portable (laptop → AWS → Azure)
   ✓ Isolated (не конфліктує з іншими apps)
   ✓ Lightweight (vs VM)
```

---

## Slide 18: VM vs Container

```
VIRTUAL MACHINES:
┌──────────────────────────────┐
│ App A    │    App B           │
├──────────┴──────────┤         │
│ Guest OS │ Guest OS │         │
├──────────┴──────────┤         │
│    Hypervisor       │         │
├─────────────────────┤         │
│     Host OS         │         │
├─────────────────────┤         │
│     Hardware        │         │
└──────────────────────────────┘

CONTAINERS:
┌──────────────────────────────┐
│ App A    │    App B    │ App C│
├──────────┴─────────────┴──────┤
│    Container Runtime (Docker) │
├──────────────────────────────┤
│        Host OS               │
├──────────────────────────────┤
│        Hardware              │
└──────────────────────────────┘
```

| | VM | Container |
|---|-------|-----------|
| **Size** | GBs | MBs |
| **Startup** | Minutes | Seconds |
| **Isolation** | Strong (separate kernel) | Process-level (shared kernel) |
| **Resource** | More overhead | Minimal overhead |

---

## Slide 19: Container Anatomy

**Як працює контейнер (Linux):**

```
1. NAMESPACES (ізоляція)
   PID: процеси
   NET: network interfaces
   MNT: filesystem mounts
   UTS: hostname
   IPC: inter-process communication

2. CGROUPS (resource limits)
   CPU: max 1 core
   Memory: max 512MB
   Disk I/O: limits

3. UNION FILESYSTEM (layers)
   Read-only image layers
   + Writable container layer
```

**Контейнер ≠ VM. Контейнер = ізольований процес на host OS.**

---

## Slide 20: Container Image

**Що це?**

Read-only template для створення контейнера.

```
IMAGE LAYERS (immutable):
┌──────────────────────┐
│ App code (5MB)       │ ← Layer 4
├──────────────────────┤
│ Dependencies (50MB)  │ ← Layer 3
├──────────────────────┤
│ Runtime (100MB)      │ ← Layer 2
├──────────────────────┤
│ Base OS (20MB)       │ ← Layer 1
└──────────────────────┘

CONTAINER = Image + Writable layer
```

**Image Registry:**
- Docker Hub (public)
- AWS ECR (Elastic Container Registry)
- Google GCR, Azure ACR

---

## Slide 21: Container Lifecycle

```
1. BUILD
   Dockerfile → docker build → Image

2. SHIP
   docker push → Registry (ECR, Docker Hub)

3. RUN
   docker run → Container (running process)

4. STOP
   docker stop → Graceful shutdown (SIGTERM)

5. REMOVE
   docker rm → Delete container
   docker rmi → Delete image
```

**Immutability:** Never update running container. Build new image → deploy new container.

---

## Slide 22: Container Best Practices

```
✓ Small Images
  Use alpine base (5MB vs 100MB)
  Multi-stage builds

✓ One Process per Container
  Container = single concern (не monolith inside)

✓ Non-root User
  Security: USER node (not root)

✓ Health Checks
  HEALTHCHECK in Dockerfile

✓ Explicit Versions
  FROM node:20-alpine (not node:latest)

✓ .dockerignore
  Exclude node_modules/, .git/

✗ Don't store data in container
  Use volumes for persistence
```

---

## Slide 23: Docker — що це?

**Docker Platform = tools для роботи з контейнерами**

```
DOCKER ARCHITECTURE:

┌─────────────────────────────────┐
│        Docker Client            │
│   (docker build, run, push)     │
└────────────┬────────────────────┘
             │ REST API
┌────────────▼────────────────────┐
│       Docker Daemon             │
│  (dockerd - manages containers) │
├─────────────────────────────────┤
│   Images │ Containers │ Networks│
└─────────────────────────────────┘
             │
┌────────────▼────────────────────┐
│    Container Runtime            │
│   (containerd → runc)           │
└─────────────────────────────────┘
```

**Docker ≠ тільки спосіб запуску контейнерів.**
Alternatives: Podman, containerd (used by K8s)

---

## Slide 24: Dockerfile

**Dockerfile = інструкції для побудови image**

```dockerfile
# Base image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Switch to non-root user
USER node

# Start command
CMD ["node", "server.js"]
```

**Основні інструкції:** FROM, WORKDIR, COPY, RUN, EXPOSE, USER, CMD, ENTRYPOINT

---

## Slide 25: Docker Image Layers

**Layer Caching для швидкої збірки:**

```
Step 1: FROM node:20-alpine
  → Layer 1 (cached if exists)

Step 2: WORKDIR /app
  → Layer 2 (cached)

Step 3: COPY package*.json ./
  → Layer 3 (cached if files unchanged)

Step 4: RUN npm ci
  → Layer 4 (cached if package.json unchanged) ← ЕКОНОМІЯ!

Step 5: COPY . .
  → Layer 5 (rebuild if code changed)
```

**Optimization:** Copy dependencies BEFORE code → reuse npm install layer.

**Bad order:**
```dockerfile
COPY . .              ← Code changes = rebuild all
RUN npm install       ← Slow every time!
```

---

## Slide 26: Docker Networking

**Network Types:**

```
1. BRIDGE (default)
   Isolated network, containers communicate via IP
   docker run → automatically bridge network

2. HOST
   Container uses host network (no isolation)
   Performance++, Security--

3. OVERLAY
   Multi-host networking (Docker Swarm, Kubernetes)
   Containers on different hosts communicate

4. NONE
   No networking
```

**Port Mapping:**
```bash
docker run -p 8080:3000 myapp
  Host:8080 → Container:3000
```

---

## Slide 27: Docker Volumes

**Problem:** Container filesystem ephemeral (втрачається при видаленні)

**Solution: Volumes** (persistent storage)

```
Types:

1. NAMED VOLUME
   docker volume create mydata
   docker run -v mydata:/app/data myapp
   → Managed by Docker

2. BIND MOUNT
   docker run -v /host/path:/container/path myapp
   → Direct host filesystem access

3. TMPFS (in-memory)
   For temporary data
```

**Use cases:**
- Databases (PostgreSQL data)
- Shared data between containers
- Development (bind mount source code)

---

## Slide 28: Kubernetes — навіщо?

**Проблеми з простим Docker:**

```
❌ Де запускати 100 containers?
❌ Як розподілити по серверах?
❌ Що якщо container crashes?
❌ Як масштабувати (add/remove containers)?
❌ Як оновити без downtime?
❌ Як балансувати трафік?
```

**Kubernetes (K8s) = Container Orchestration Platform**

Автоматизує:
- ✅ Deployment
- ✅ Scaling
- ✅ Self-healing
- ✅ Load balancing
- ✅ Rolling updates
- ✅ Service discovery

---

## Slide 29: K8s Architecture

```
CONTROL PLANE (master):
┌───────────────────────────────────┐
│ API Server (kubectl → API)        │
│ Scheduler (де запустити pod?)     │
│ Controller Manager (desired state)│
│ etcd (cluster state DB)           │
└───────────────────────────────────┘
              ↓ manages
┌─────────────────────────────────────┐
│        WORKER NODES                 │
│  ┌────────────────────────────┐    │
│  │ Node 1                      │    │
│  │ - kubelet (agent)           │    │
│  │ - kube-proxy (networking)   │    │
│  │ - Container Runtime (Docker)│    │
│  │ - Pods (containers)         │    │
│  └────────────────────────────┘    │
│  ┌────────────────────────────┐    │
│  │ Node 2, Node 3...           │    │
│  └────────────────────────────┘    │
└─────────────────────────────────────┘
```

**Managed K8s:** AWS EKS, GKE, AKS → Control Plane керується cloud provider

---

## Slide 30: K8s Основні Об'єкти

| Об'єкт | Що це | Приклад |
|--------|-------|---------|
| **Pod** | Найменша одиниця. 1+ контейнерів | Web container + sidecar logging |
| **Deployment** | Manages Pods: replicas, rolling updates | "Запусти 3 копії nginx" |
| **Service** | Stable network endpoint для Pods | ClusterIP: internal, LoadBalancer: external |
| **ConfigMap** | Configuration (non-sensitive) | API_URL=https://api.example.com |
| **Secret** | Sensitive data (base64) | DB_PASSWORD=secret123 |
| **Ingress** | HTTP routing (domain → service) | api.example.com → api-service |

---

## Slide 31: Pod Lifecycle

```
Pod States:

Pending
  ↓ (Scheduler assigns node)
Running
  ↓ (Container exits)
Succeeded (exit 0) or Failed (exit ≠ 0)

Restart Policies:
- Always (default for Deployment)
- OnFailure (Jobs)
- Never
```

**Pod = ephemeral.** Не покладайся на конкретний Pod. Service абстрагує.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.25
    ports:
    - containerPort: 80
```

---

## Slide 32: Service Types

```
1. ClusterIP (default)
   Internal-only access
   my-service.default.svc.cluster.local

2. NodePort
   Exposes on each Node's IP:port
   <NodeIP>:30080

3. LoadBalancer
   Cloud provider's LB (AWS ELB)
   External IP for internet access

4. Ingress (not a Service type, but related)
   HTTP routing rules
   api.example.com → api-service:80
   web.example.com → web-service:3000
```

**Typical:** Ingress (external) → Service (internal) → Pods

---

## Slide 33: K8s Scaling & Healing

**Horizontal Pod Autoscaler (HPA):**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web-hpa
spec:
  scaleTargetRef:
    kind: Deployment
    name: web-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

**Self-healing:**
- Container crashes → restart
- Pod crashes → recreate on another Node
- Node fails → reschedule all Pods to healthy Nodes

**Rolling Updates:** Zero-downtime deployment (update Pods one by one)

---

## Slide 34: Event-driven — Концепція

**Event-driven Architecture (EDA):**

Система реагує на **події (events)** замість direct calls.

```
Traditional (Sync):
  Service A → calls → Service B
  (tight coupling, Service A blocks)

Event-driven (Async):
  Service A → publishes Event → Event Bus
  Service B, C, D → subscribe → consume Event
  (loose coupling, non-blocking)
```

**Event = immutable fact про що сталося:**
- OrderPlaced
- PaymentProcessed
- UserRegistered

**Компоненти:** Producer, Event Bus (SQS, SNS, Kafka, EventBridge), Consumer

---

## Slide 35: Event-driven Patterns

**1. Pub/Sub (Publish/Subscribe):**
```
Publisher → Topic → Subscriber 1, 2, 3
AWS: SNS → SQS (fanout)
```

**2. Event Sourcing:**
```
Store events (не final state)
OrderPlaced → PaymentReceived → OrderShipped
Replay events → reconstruct state
```

**3. CQRS (Command Query Responsibility Segregation):**
```
Write Model (commands) ≠ Read Model (queries)
Event → update both
```

**AWS приклади:**
- S3 Event Notifications → Lambda
- DynamoDB Streams → Lambda
- EventBridge rules → targets

---

## Slide 36: Переваги та Виклики Event-driven

**Переваги:**
```
✅ Loose Coupling
   Producer не знає про consumers

✅ Scalability
   Add consumers без зміни producer

✅ Resilience
   Queue buffering → retry failed messages

✅ Asynchronous
   Non-blocking, better performance
```

**Виклики:**
```
❌ Eventual Consistency
   Data not immediately consistent

❌ Complexity
   Distributed debugging складне

❌ Message Ordering
   Не гарантується (без додаткових зусиль)

❌ Duplicate Messages
   At-least-once delivery → handle idempotency
```

---

## Slide 37: AWS ECS (Elastic Container Service)

**Managed container orchestration від AWS**

```
ECS Components:

1. CLUSTER
   Logical group of EC2 or Fargate resources

2. TASK DEFINITION
   Blueprint: which image, CPU, memory, env vars
   (like Dockerfile for ECS)

3. SERVICE
   Runs & maintains N tasks (like K8s Deployment)

4. TASK
   Running instance of Task Definition (like Pod)
```

**Launch Types:**
- **EC2:** You manage instances (more control, cheaper)
- **Fargate:** Serverless (AWS manages, easier, pricier)

**Integration:** ALB, CloudWatch, ECR, IAM roles

---

## Slide 38: AWS EKS (Elastic Kubernetes Service)

**Managed Kubernetes на AWS**

```
EKS = K8s Control Plane (managed by AWS)
    + Worker Nodes (EC2 or Fargate)

Переваги:
✅ Standard Kubernetes (portable)
✅ CNCF ecosystem (Helm, Prometheus, etc.)
✅ No vendor lock-in
✅ Multi-cloud experience

Integration:
- VPC networking
- IAM for RBAC
- ALB Ingress Controller
- EBS/EFS for storage
```

**ECS vs EKS:**
- **ECS:** Simple, AWS-specific, швидкий старт
- **EKS:** Complex, portable, Kubernetes ecosystem

---

## Slide 39: AWS Lambda

**Serverless Compute — Function as a Service (FaaS)**

```
Lambda = code без серверів

Trigger → Lambda Function → Result
         (your code)

Triggers:
- API Gateway (HTTP requests)
- S3 events (file upload)
- DynamoDB Streams (DB changes)
- SQS, SNS, EventBridge
- CloudWatch Events (schedule)
```

**Limitations:**
- Max runtime: 15 min
- Max memory: 10 GB
- Cold start: 100-500ms (or use Provisioned Concurrency)

**Pricing:** Pay per invocation + duration (ms)

**Use cases:** Event processing, ETL, APIs, scheduled jobs

---

## Slide 40: Ключові Висновки

**Принципи Cloud-native:**

```
1. ✓ Cloud-native = Microservices + Containers + Orchestration
   12-Factor App, API-first, Observability

2. ✓ Microservices
   Independent deployment, tech diversity
   Trade-off: complexity vs scalability

3. ✓ Containers > VMs
   Lightweight, portable, fast
   Docker = standard

4. ✓ Orchestration
   Kubernetes (EKS) = powerful, portable
   ECS = simple, AWS-specific

5. ✓ Event-driven
   Async, decoupled, scalable
   SQS, SNS, EventBridge, Lambda

6. ✓ Serverless (Lambda)
   No infrastructure management
   Pay-per-use, fast iteration
```

**Наступна лекція:** Моніторинг та Observability (Metrics, Logs, Traces)

---

## Питання для самоперевірки

1. Яка різниця між Container та VM?
2. Що таке 12-Factor App? Назвіть 3 принципи.
3. Коли використовувати Monolith vs Microservices?
4. Що таке Pod у Kubernetes?
5. В чому різниця між ECS та EKS?
6. Коли використовувати Lambda замість ECS?
7. Що таке Event-driven Architecture? Переваги?

**Ресурси:**
- 12factor.net
- kubernetes.io/docs
- docs.docker.com
- AWS Container Services documentation
