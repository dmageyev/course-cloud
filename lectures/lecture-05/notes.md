# Лекція 5: Обчислювальні ресурси та Linux у хмарі — Детальний конспект

## Огляд лекції

**Тривалість:** 2 години (90 хвилин контенту)  
**Аудиторія:** Бакалаври ІТ, 3 курс  
**Мета:** Розуміння обчислювальних ресурсів у хмарі та основ роботи з Linux у production-середовищі

---

## 1. Гіпервізори та віртуалізація

### 1.1 Що таке гіпервізор

**Гіпервізор (Hypervisor)** — це програмне або апаратне забезпечення, яке дозволяє запускати кілька операційних систем одночасно на одному фізичному сервері. Кожна ОС, що запускається на гіпервізорі, ізольована і не "бачить" інших — вона вважає, що є єдиним власником апаратних ресурсів.

```
Фізичний сервер (32 vCPU, 256 GB RAM)
├── VM 1: Linux (4 vCPU, 16 GB)  ← Ваш EC2 instance
├── VM 2: Linux (8 vCPU, 32 GB)  ← Чужий EC2 instance
├── VM 3: Windows (2 vCPU, 8 GB) ← Ще чийсь instance
└── Hypervisor (управляє ресурсами)
```

**Ключові функції гіпервізора:**
- Розподіл CPU, RAM, I/O між VM
- Ізоляція VM одне від одного
- Live migration (переміщення VM без зупинки)
- Snapshots та відновлення

### 1.2 Type 1 vs Type 2

**Type 1 (Bare-metal hypervisor):**
- Запускається безпосередньо на фізичному залізі
- Немає "host OS" між гіпервізором та залізом
- Максимальна продуктивність та ізоляція
- Приклади: VMware ESXi, Microsoft Hyper-V, KVM, Xen
- **Використання:** Корпоративні дата-центри, хмарні провайдери (AWS, Azure, GCP)

**Type 2 (Hosted hypervisor):**
- Запускається поверх звичайної ОС (Windows, macOS, Linux)
- Host OS → Hypervisor → Guest OS
- Нижча продуктивність через додатковий рівень
- Приклади: VirtualBox, VMware Workstation, Parallels
- **Використання:** Розробка, тестування на локальній машині

### 1.3 AWS Nitro System

AWS розробила власну архітектуру гіпервізора — **Nitro System**. Вона принципово відрізняється від класичних підходів:

```
Класичний підхід:             AWS Nitro System:
┌─────────────────┐           ┌──────────────────────┐
│   Guest OS      │           │   Guest OS (EC2)      │
├─────────────────┤           ├──────────────────────┤
│   Hypervisor    │           │   Nitro Hypervisor   │
│  (управляє ALL) │           │   (тільки CPU/mem)   │
├─────────────────┤           ├──────────┬───────────┤
│  Physical HW    │           │Nitro Card│Nitro Card │
└─────────────────┘           │(Network) │(Storage)  │
                              ├──────────┴───────────┤
                              │     Physical HW       │
                              └──────────────────────┘
```

**Переваги Nitro:**
- I/O (мережа, диск) обробляється спеціалізованими картами, не CPU
- Hypervisor займає <1% CPU хоста (у класичних ~10-15%)
- Краща ізоляція та безпека (Nitro Security Chip)
- Bare-metal instances — EC2 без гіпервізора взагалі

---

## 2. VM vs Containers vs Serverless

### 2.1 Virtual Machines (VM)

**VM** — повна емуляція комп'ютера зі своїм ядром ОС, бібліотеками та додатком.

```
┌──────────────────┐
│   Application    │
├──────────────────┤
│   OS Libraries   │
├──────────────────┤
│  Full OS Kernel  │   ← окрема копія ядра
├──────────────────┤
│   Hypervisor     │
└──────────────────┘
```

**Характеристики:**
- Startup time: 1-5 хвилин
- Розмір: гігабайти
- Ізоляція: найвища (окреме ядро)
- Контроль: повний (root access, kernel tuning)
- AWS: EC2

**Коли обирати VM:**
- Потрібен повний контроль над ОС
- Legacy applications
- Stateful workloads (databases)
- Специфічні вимоги до ядра

### 2.2 Containers

**Контейнер** — ізольований процес, що поділяє ядро ОС з хостом, але має власну файлову систему та namespace.

```
┌──────────┐  ┌──────────┐  ┌──────────┐
│  App A   │  │  App B   │  │  App C   │
├──────────┤  ├──────────┤  ├──────────┤
│ Libs A   │  │ Libs B   │  │ Libs C   │
├──────────┴──┴──────────┴──┴──────────┤
│        Shared OS Kernel               │
├───────────────────────────────────────┤
│        Container Runtime (Docker)     │
└───────────────────────────────────────┘
```

**Характеристики:**
- Startup time: секунди
- Розмір: мегабайти
- Ізоляція: середня (shared kernel)
- Контроль: обмежений (userspace only)
- AWS: ECS, EKS

**Коли обирати Containers:**
- Мікросервіси
- Швидкий deployment
- Висока щільність на хості
- CI/CD pipelines

### 2.3 Serverless (FaaS)

**Serverless** — код виконується в response на event, без управління серверами. Провайдер автоматично масштабує та управляє інфраструктурою.

```
Event (HTTP, queue, schedule)
        │
        ▼
┌──────────────────────┐
│   Function (Lambda)  │
│   ваш код тут        │
└──────────────────────┘
        │
        ▼
Result → Response/side effect
(Після завершення — ресурси звільняються)
```

**Характеристики:**
- Startup time: мілісекунди (cold start: ~1-5с)
- Ізоляція: висока (окремий мікро-VM)
- Контроль: мінімальний
- Оплата: тільки за execution time
- AWS: Lambda, Fargate

**Коли обирати Serverless:**
- Event-driven processing
- APIs з нерівномірним трафіком
- Автоматизація, cron jobs
- Коли немає потреби в постійно запущеному сервісі

### 2.4 Матриця рішень

```
                 Контроль
                    ↑
         VM         │
                    │
         Containers │
                    │
         Serverless │
                    └──────────────→ Швидкість (time-to-market)
```

---

## 3. Linux-first Mindset

### 3.1 Чому Linux у Production

**Статистика:** ~96% публічних хмарних серверів використовують Linux (StackOverflow Survey 2023).

**Причини:**
1. **Безкоштовний** — жодних ліцензійних витрат на ОС
2. **Відкритий код** — повний контроль та аудит
3. **Стабільний** — роки uptime без перезавантаження
4. **Ефективний** — менший overhead порівняно з Windows
5. **Автоматизація** — shell scripting, cron, systemd
6. **Ecosystem** — більшість DevOps інструментів Linux-native

### 3.2 Amazon Linux 2023 vs Ubuntu

**Amazon Linux 2023 (AL2023):**
- Оптимізований для AWS EC2
- Швидкі security patches від AWS
- Включає AWS CLI, SSM Agent з коробки
- Package manager: `dnf` (RPM-based)
- Підтримка: 5 років
- **Рекомендується для production AWS workloads**

```bash
# AL2023 — встановлення пакету
sudo dnf install nginx -y
sudo dnf update -y

# Перегляд встановлених пакетів
dnf list installed
```

**Ubuntu (22.04 LTS / 24.04 LTS):**
- Широка спільнота, багато туторіалів
- Package manager: `apt` (Debian-based)
- Більший вибір пакетів у репозиторіях
- Знайомий для розробників
- LTS (Long Term Support): 5 років

```bash
# Ubuntu — встановлення пакету
sudo apt-get update
sudo apt-get install nginx -y

# Оновлення всієї системи
sudo apt-get upgrade -y
```

### 3.3 Структура файлової системи Linux

```
/ (root)
├── /etc/          ← конфігурації (nginx.conf, sshd_config)
├── /var/
│   ├── /var/log/  ← логи (nginx, syslog, auth.log)
│   └── /var/www/  ← веб-контент (типово)
├── /home/         ← домашні директорії користувачів
│   └── /home/ec2-user/
├── /tmp/          ← тимчасові файли (очищається при reboot)
├── /usr/
│   ├── /usr/bin/  ← бінарники (nginx, python, git)
│   └── /usr/lib/  ← бібліотеки
├── /opt/          ← стороннє ПЗ (Java apps, etc.)
├── /proc/         ← virtual FS з info про процеси
└── /sys/          ← virtual FS з info про hardware
```

**Ключові конфіг файли:**
```
/etc/nginx/nginx.conf         ← nginx конфігурація
/etc/ssh/sshd_config          ← SSH daemon config
/etc/hosts                    ← local DNS overrides
/etc/fstab                    ← mount points
/etc/crontab                  ← scheduled tasks
```

---

## 4. systemd та управління сервісами

### 4.1 Що таке systemd

**systemd** — це init system та service manager для Linux. Запускається першим після ядра (PID 1) та управляє всіма сервісами.

```
Kernel boots
     │
     ▼
systemd (PID 1)
     ├── sshd.service     ← SSH daemon
     ├── nginx.service    ← Web server
     ├── cron.service     ← Scheduled tasks
     └── network.service  ← Networking
```

**Чому systemd важливий:**
- Паралельний запуск сервісів (швидший boot)
- Автоматичний restart при падінні
- Dependency management між сервісами
- Централізована система логування (journald)

### 4.2 systemctl команди

```bash
# Управління сервісом
sudo systemctl start nginx      # Запустити
sudo systemctl stop nginx       # Зупинити
sudo systemctl restart nginx    # Перезапустити
sudo systemctl reload nginx     # Перезавантажити конфіг (без зупинки)

# Автозапуск
sudo systemctl enable nginx     # Увімкнути автозапуск
sudo systemctl disable nginx    # Вимкнути автозапуск
sudo systemctl is-enabled nginx # Перевірити статус

# Статус та інформація
sudo systemctl status nginx     # Детальний статус
sudo systemctl list-units --type=service  # Всі сервіси
sudo systemctl list-units --failed        # Падаючі сервіси
```

**Приклад виводу `systemctl status`:**
```
● nginx.service - A high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled)
   Active: active (running) since Mon 2024-01-15 10:00:00 UTC
  Process: 1234 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
 Main PID: 1235 (nginx)
   CGroup: /system.slice/nginx.service
           ├─1235 nginx: master process /usr/sbin/nginx
           └─1236 nginx: worker process
```

### 4.3 journalctl — читання логів

```bash
# Базові команди
sudo journalctl                         # Всі логи
sudo journalctl -u nginx                # Логи сервісу nginx
sudo journalctl -u nginx -n 50          # Останні 50 рядків
sudo journalctl -u nginx -f             # Live (слідкування)
sudo journalctl -u nginx --since today  # Логи з сьогодні

# Часовий діапазон
sudo journalctl --since "2024-01-15 10:00:00" \
                --until "2024-01-15 11:00:00"

# Рівень логування
sudo journalctl -p err          # Тільки errors
sudo journalctl -p warning      # Warning та вище

# Формат виводу
sudo journalctl -u nginx -o json-pretty  # JSON формат
```

---

## 5. Порти та мережа

### 5.1 Важливі порти

| Порт | Протокол | Сервіс | Де відкривати |
|------|----------|--------|---------------|
| 22 | TCP | SSH | Тільки trusted IP |
| 80 | TCP | HTTP | 0.0.0.0/0 |
| 443 | TCP | HTTPS | 0.0.0.0/0 |
| 3306 | TCP | MySQL | Тільки app servers |
| 5432 | TCP | PostgreSQL | Тільки app servers |
| 6379 | TCP | Redis | Тільки app servers |
| 8080 | TCP | HTTP alternate | App-specific |

### 5.2 Перевірка мережі

```bash
# Які порти слухаємо?
ss -tlnp          # Всі TCP listening ports з process info
ss -tlnp | grep :80  # Чи слухає port 80?

# Активні з'єднання
ss -tnp           # Всі активні TCP connections

# Мережеві інтерфейси
ip addr show      # IP адреси
ip route show     # Таблиця маршрутизації

# DNS
nslookup google.com
dig google.com

# Перевірка доступності
curl -I http://localhost
wget --spider https://example.com

# Трасування маршруту
traceroute google.com
```

### 5.3 Security Groups як firewall

В AWS основний захист на рівні мережі — **Security Groups**:

```
Internet
   │
   ▼
[Security Group: WebSG]
   Allow: 80, 443 from 0.0.0.0/0
   Allow: 22 from 203.0.113.0/24 (your office IP)
   │
   ▼
EC2 Instance
   │
   ▼
[Security Group: DBSG]
   Allow: 3306 from WebSG only
   │
   ▼
RDS MySQL
```

> **Security Groups — stateful (дозволяє return traffic автоматично)**

---

## 6. Immutable vs Mutable Infrastructure

### 6.1 Mutable Infrastructure (старий підхід)

```
Сервер запущено → Тижні роботи → Оновлення → Патчі → Конфіги →
Ще оновлення → Конфіг дрейф → "Унікальний" сервер → Страх змін 😱
```

**Проблема:** Після місяців роботи сервер стає "сніжинкою" — унікальним, непередбачуваним. Якщо він впаде — відновлення може зайняти годину.

**Cattle vs Pets аналогія:**
- **Pets (домашні тварини):** Кожен сервер має ім'я (web01, db-master). За ними доглядають, їх лікують, не замінюють.
- **Cattle (худоба):** Сервери пронумеровані (i-0a1b2c3d). Захворів — замінити новим.

### 6.2 Immutable Infrastructure (сучасний підхід)

**Принцип:** Сервер ніколи не змінюється після запуску. Замість оновлення — створення нового сервера з нового образу.

```
Зміна коду/конфігу
        │
        ▼
Білд нового AMI (з усіма змінами)
        │
        ▼
Тестування нового AMI
        │
        ▼
Launch нових EC2 з нового AMI
        │
        ▼
Перенаправити трафік (ALB target group update)
        │
        ▼
Terminate старих EC2
```

**Переваги:**
- Predictable deployments — завжди знаємо стан
- Легкий rollback — повернутися до попереднього AMI
- Немає configuration drift
- Консистентність dev/staging/prod

### 6.3 Packer для Golden AMI

```json
// packer.json
{
  "builders": [{
    "type": "amazon-ebs",
    "region": "us-east-1",
    "source_ami": "ami-0abcdef1234567890",
    "instance_type": "t3.micro",
    "ssh_username": "ec2-user",
    "ami_name": "my-app-{{timestamp}}"
  }],
  "provisioners": [{
    "type": "shell",
    "script": "setup.sh"
  }]
}
```

---

## 7. Stateless Systems

### 7.1 Проблема стану

**Stateful (проблема при масштабуванні):**

```
User → Request 1 → Server A (зберігає session в RAM)
User → Request 2 → Server B (не знає про session!) → 401 Error!
```

При горизонтальному масштабуванні (декілька серверів) стан у пам'яті сервера стає проблемою.

### 7.2 Stateless Pattern

**Рішення:** Виносимо стан назовні.

```
                    ┌────────────────┐
                    │   Redis/       │
                    │   DynamoDB     │
                    │   (sessions)   │
                    └────────┬───────┘
                             │ read/write
User → Load Balancer → Server A ──┤
                    └──────────────→ Server B ──┤
                                                │
                    ┌───────────────────────────┘
                    │
                    ▼
              Shared State Storage
```

**Де зберігати різні типи стану:**
- Session data → Redis (ElastiCache), DynamoDB
- Uploaded files → S3
- Database → RDS, Aurora
- Cache → ElastiCache

### 7.3 12-Factor App — фактори для stateless

З [12factor.net](https://12factor.net):

- **Factor 6 (Processes):** Execute the app as one or more stateless processes
- **Factor 7 (Port binding):** Export services via port binding
- **Factor 8 (Concurrency):** Scale out via the process model
- **Factor 9 (Disposability):** Maximize robustness with fast startup and graceful shutdown

---

## 8. AWS EC2 в деталях

### 8.1 EC2 Instance Types

Формат назви: `<family><generation><attributes>.<size>`

Наприклад: `t3.medium`, `m6i.xlarge`, `c5n.4xlarge`

| Розмір | vCPU | RAM |
|--------|------|-----|
| nano | 2 | 0.5 GB |
| micro | 2 | 1 GB |
| small | 2 | 2 GB |
| medium | 2 | 4 GB |
| large | 2 | 8 GB |
| xlarge | 4 | 16 GB |
| 2xlarge | 8 | 32 GB |

**Сімейства:**
- `t3/t4g` — буrstable, для непостійного навантаження (Dev, small apps)
- `m5/m6i` — загальне призначення (Web servers, app servers)
- `c5/c6i` — compute-optimized (Video processing, ML inference)
- `r5/r6i` — memory-optimized (In-memory DB, Elasticsearch)
- `i3/i4i` — storage-optimized (High IOPS databases)

### 8.2 EC2 Pricing Models

**On-Demand:**
- Платиш за годину (або секунду)
- Без commitment
- Найдорожче, але гнучко
- Підходить: Dev/Test, нерегулярні workloads

**Reserved Instances:**
- Резервуєш на 1 або 3 роки
- До 75% дешевше від On-Demand
- Підходить: Stable production workloads

**Spot Instances:**
- Використовуємо spare capacity AWS
- До 90% дешевше
- Може бути terminated з 2 хвилинами попередження
- Підходить: Batch processing, fault-tolerant workloads

**Savings Plans:**
- Гнучкіші за Reserved
- Commitment на spend ($/год), не конкретний instance type
- До 66% дешевше

### 8.3 User Data

**User Data** — скрипт, що виконується один раз при першому запуску EC2 instance (bootstrap).

```bash
#!/bin/bash
# /var/log/cloud-init-output.log — лог виконання

# Оновлення
dnf update -y

# Встановлення пакетів
dnf install -y nginx git

# Клонування коду
git clone https://github.com/org/app.git /opt/app

# Конфігурація nginx
cat > /etc/nginx/conf.d/app.conf << 'EOF'
server {
    listen 80;
    server_name _;
    root /opt/app/public;
    index index.html;
}
EOF

# Запуск
systemctl start nginx
systemctl enable nginx

# Теги для ідентифікації (через instance metadata)
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
echo "Instance $INSTANCE_ID is ready" >> /var/log/bootstrap.log
```

### 8.4 AMI (Amazon Machine Image)

**AMI = Template для EC2**

```
Що входить до AMI:
├── Root volume snapshot (EBS)
│   ├── ОС (Amazon Linux, Ubuntu)
│   ├── Встановлені пакети
│   ├── Конфігурації
│   └── Код застосунку (або без нього)
├── Launch permissions (хто може використовувати)
└── Block device mapping (які диски додати)
```

**Типи AMI:**
- **AWS Marketplace AMI** — готові образи (Amazon Linux, Ubuntu, Windows)
- **Community AMI** — від спільноти
- **Custom AMI (Golden AMI)** — ваш власний, з усім необхідним

**Lifecycle AMI:**
```
1. Launch EC2 від базового AMI
2. Встановити пакети, налаштувати
3. AWS Console → Actions → Image → Create Image
4. Очікувати (5-10 хвилин)
5. Новий AMI доступний → запускаємо нові EC2
```

### 8.5 Auto Scaling Group

**Auto Scaling Group (ASG)** — автоматично додає або видаляє EC2 instances на основі:
- Метрик (CPU, RAM, custom)
- Розкладу (вночі менше, вдень більше)
- Predictions (ML-based forecasting)

```
ASG Configuration:
├── Launch Template (який AMI, type, security groups)
├── Min capacity: 2     ← завжди є мінімум 2 instances
├── Desired capacity: 4 ← зараз хочемо 4
└── Max capacity: 10    ← не більше 10

Scaling Policies:
├── Target Tracking: "тримай CPU 70%"
├── Step Scaling: "+2 instances якщо CPU > 80% на 5 хв"
└── Scheduled: "в 8:00 встановити desired=8"
```

**Health Checks:**
- EC2 health check (status checks)
- ELB health check (Application Load Balancer перевіряє `/health` endpoint)
- При неуспішному health check → terminate та launch новий

---

## 9. Практичний Hands-On

### 9.1 Запустити EC2 з Nginx через User Data

```bash
#!/bin/bash
# User Data Script — встановлення nginx та demo page

# 1. Оновлення системи
dnf update -y

# 2. Встановлення nginx
dnf install nginx -y

# 3. Запуск
systemctl start nginx
systemctl enable nginx

# 4. Demo page з інформацією про instance
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
AZ=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)

cat > /usr/share/nginx/html/index.html << EOF
<!DOCTYPE html>
<html>
<body>
<h1>Hello from EC2!</h1>
<p>Instance ID: $INSTANCE_ID</p>
<p>AZ: $AZ</p>
</body>
</html>
EOF

echo "Bootstrap complete at $(date)" >> /var/log/user-data.log
```

### 9.2 Підключення по SSH

```bash
# Зберегти key pair (.pem) при створенні EC2
chmod 400 ~/.ssh/my-ec2-key.pem

# Підключитись
ssh -i ~/.ssh/my-ec2-key.pem ec2-user@<PUBLIC_IP>

# Або якщо Ubuntu:
ssh -i ~/.ssh/my-ec2-key.pem ubuntu@<PUBLIC_IP>

# SSM Session Manager (без key pair, через IAM)
aws ssm start-session --target i-0a1b2c3d4e5f67890
```

### 9.3 Базові команди для troubleshooting

```bash
# Система
uptime                    # Час роботи, load average
free -h                   # RAM usage
df -h                     # Disk usage

# Nginx troubleshooting
systemctl status nginx
sudo nginx -t             # Перевірка конфігу
sudo journalctl -u nginx -n 100

# Логи
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log

# Процеси
ps aux | grep nginx
top -p $(pgrep nginx | head -1)

# Мережа
ss -tlnp | grep :80
curl -I http://localhost
```

---

## 10. Security Considerations

### 10.1 EC2 Security Best Practices

```bash
# 1. Мінімальний Security Group
# SSH тільки з вашого IP, не 0.0.0.0/0!
# Inbound: 22 → your_ip/32
# Inbound: 80 → 0.0.0.0/0
# Inbound: 443 → 0.0.0.0/0

# 2. Вимкнути root SSH login
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart sshd

# 3. Ключі замість паролів
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' \
    /etc/ssh/sshd_config
sudo systemctl restart sshd

# 4. Автоматичні security updates
sudo dnf install dnf-automatic -y
sudo systemctl enable dnf-automatic-install.timer
sudo systemctl start dnf-automatic-install.timer
```

### 10.2 IMDSv2 — захист від SSRF

**SSRF (Server-Side Request Forgery)** — атака, при якій зловмисник може змусити сервер зробити запит до `http://169.254.169.254/` та отримати IAM credentials.

**IMDSv2 вирішує цю проблему:**

```bash
# IMDSv1 (небезпечно) — простий GET
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/

# IMDSv2 (безпечно) — потрібен token
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
    -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
    http://169.254.169.254/latest/meta-data/
```

> **Завжди вмикайте IMDSv2 при створенні EC2!**

### 10.3 IAM Instance Profile

Замість hard-coded credentials — використовуйте **IAM Instance Profile**:

```
EC2 Instance
    │
    ▼ (авто-отримує temporary credentials)
IAM Instance Profile → IAM Role
    │
    ▼ (permissions)
S3:GetObject на specific bucket
DynamoDB:PutItem на specific table
```

```bash
# Перевірка поточної ролі
curl -s http://169.254.169.254/latest/meta-data/iam/security-credentials/
# або з IMDSv2:
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
    -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
    http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

---

## Підсумок лекції

```
Гіпервізор
  └─ Type 1 (bare-metal) > Type 2 (hosted)
  └─ AWS Nitro = bare-metal performance + isolation

VM vs Containers vs Serverless
  └─ Різні рівні абстракції та trade-offs
  └─ EC2 (VM) → ECS/EKS (Containers) → Lambda (Serverless)

Linux-first
  └─ 96% production серверів у хмарі
  └─ Amazon Linux 2023 (AL2023) для AWS

systemd
  └─ systemctl start/stop/enable/status
  └─ journalctl для логів

Immutable Infrastructure
  └─ AMI = Golden image
  └─ Launch new → test → redirect → terminate old

Stateless
  └─ Стан зовні EC2 (Redis, S3, RDS)
  └─ Дозволяє горизонтальне масштабування

Auto Scaling Group
  └─ Min/Desired/Max
  └─ Launch Template + Scaling Policies
```

---

## Anti-Patterns ❌

| Anti-pattern | Проблема | Рішення |
|---|---|---|
| SSH та ручні зміни на prod | Configuration drift | Immutable AMI |
| Зберігати state в EC2 RAM | Не масштабується | Redis / DynamoDB |
| SSH з 0.0.0.0/0 | Security risk | Обмежити IP / SSM |
| Один EC2 без ASG | SPOF | ASG з min=2 |
| Hard-coded credentials | Security risk | IAM Instance Profile |
| Ігнорувати security updates | Vulnerability | Автоматичні updates |
