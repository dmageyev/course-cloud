# Лекція 5: Обчислювальні ресурси та Linux у хмарі

<!-- reveal.js presentation -->
<!-- Separator: --- (horizontal), -- (vertical) -->

---

## Agenda

1. Гіпервізори та віртуалізація
2. VM vs Containers vs Serverless
3. Linux-first mindset
4. systemd та управління сервісами
5. Ports та мережа
6. Immutable vs Mutable інфраструктура
7. Stateless системи
8. AWS: EC2, AMI, Auto Scaling

---

## Де живе ваш код у хмарі?

```
Ваш код
  └─ Process
       └─ OS (Linux)
            └─ VM (Virtual Machine)
                 └─ Hypervisor
                      └─ Physical Server
                           └─ AWS Data Center
```

> **Хмара = чужий сервер + рівні абстракції**

---

## Гіпервізори: що це?

**Hypervisor** — програмне забезпечення, що дозволяє запускати кілька ОС на одному фізичному сервері.

```
┌─────────────────────────────────────┐
│  VM 1      │  VM 2      │  VM 3     │
│  Linux     │  Windows   │  Linux    │
├─────────────────────────────────────┤
│          Hypervisor                 │
├─────────────────────────────────────┤
│       Physical Hardware             │
└─────────────────────────────────────┘
```

---

## Type 1 vs Type 2 Hypervisor

| | Type 1 (Bare-metal) | Type 2 (Hosted) |
|---|---|---|
| Запускається на | Залізі напряму | Поверх ОС |
| Приклади | VMware ESXi, KVM, Xen | VirtualBox, VMware Workstation |
| Продуктивність | ✅ Висока | ❌ Нижча |
| Використання | Production | Dev/Testing |

> **AWS використовує KVM → AWS Nitro System**

---

## AWS Nitro System

```
┌──────────────────────────────────────────┐
│              EC2 Instance                │
├──────────────────────────────────────────┤
│           Nitro Hypervisor               │
│  (легковаговий, ~1% CPU overhead)        │
├───────────────┬──────────────────────────┤
│  Nitro Card   │  Nitro Security Chip     │
│  (Network/    │  (Hardware root of       │
│   Storage)    │   trust)                 │
├───────────────┴──────────────────────────┤
│         Physical Server (AWS)            │
└──────────────────────────────────────────┘
```

**Nitro переніс I/O на dedicated hardware → краща ізоляція та продуктивність**

---

## VM vs Containers vs Serverless

```
┌──────────┐  ┌──────────┐  ┌──────────┐
│ App  A   │  │ App  A   │  │          │
├──────────┤  ├──────────┤  │ Function │
│  OS      │  │Container │  │          │
│ (full)   │  │ Runtime  │  └──────────┘
├──────────┤  ├──────────┤  ┌──────────┐
│Hypervisor│  │  OS      │  │          │
├──────────┤  ├──────────┤  │ Function │
│ Hardware │  │ Hardware │  │          │
└──────────┘  └──────────┘  └──────────┘
     VM         Container    Serverless
```

---

## Порівняння: коли що обирати

| | VM | Container | Serverless |
|---|---|---|---|
| Startup time | ~хвилини | ~секунди | ~мілісекунди |
| Isolation | ✅ Висока | Середня | ✅ Висока |
| Control | ✅ Повний | Середній | ❌ Мінімальний |
| Cost model | Погодинно | Погодинно | Per-request |
| Stateful | ✅ Легко | Складніше | ❌ Важко |
| AWS сервіс | EC2 | ECS/EKS | Lambda |

---

## Linux-first mindset 🐧

**Чому Linux у production?**

- **~95%** серверів у хмарі — Linux
- Безкоштовний та open-source
- Стабільний, передбачуваний
- Широка екосистема інструментів
- AWS-native: Amazon Linux 2023

```bash
# Amazon Linux 2023 (рекомендовано для AWS)
cat /etc/os-release

# Ubuntu — популярний вибір
lsb_release -a
```

---

## Amazon Linux 2023 vs Ubuntu

| | Amazon Linux 2023 | Ubuntu 22.04 LTS |
|---|---|---|
| Package manager | `dnf` | `apt` |
| Оптимізація | AWS-native | Generic |
| Підтримка | AWS | Canonical |
| Kernel patches | Швидко | Стандартно |
| Використання | EC2 prod | Flexible |

```bash
# AL2023
sudo dnf install nginx -y

# Ubuntu
sudo apt-get install nginx -y
```

---

## Файлова система Linux

```
/
├── etc/        ← конфігурації
├── var/        ← змінні дані (logs, db)
│   └── log/   ← логи
├── home/       ← домашні директорії
├── tmp/        ← тимчасові файли
├── usr/        ← програми
│   └── bin/   ← бінарники
├── opt/        ← стороннє ПО
└── proc/       ← інформація про процеси (virtual)
```

> **Ключові шляхи: `/etc/`, `/var/log/`, `/tmp/`**

---

## systemd — серце Linux сервера

```
┌─────────────────────────────────────┐
│              systemd                │
│         (PID 1, init system)        │
├───────────┬─────────────┬───────────┤
│  Services │  Sockets    │  Timers   │
│ (nginx,   │ (systemd-   │ (cron-    │
│  sshd..)  │  journald)  │  like)    │
└───────────┴─────────────┴───────────┘
```

**Перший процес після ядра — systemd**

---

## systemctl — управління сервісами

```bash
# Запустити сервіс
sudo systemctl start nginx

# Зупинити сервіс
sudo systemctl stop nginx

# Перезапустити
sudo systemctl restart nginx

# Авто-запуск при завантаженні
sudo systemctl enable nginx

# Статус
sudo systemctl status nginx

# Список всіх сервісів
sudo systemctl list-units --type=service
```

---

## journalctl — централізовані логи

```bash
# Всі логи
sudo journalctl

# Логи конкретного сервісу
sudo journalctl -u nginx

# Останні 50 рядків
sudo journalctl -u nginx -n 50

# Live-моніторинг
sudo journalctl -u nginx -f

# Логи з часового проміжку
sudo journalctl --since "2024-01-01" --until "2024-01-02"
```

---

## Важливі порти (Common Ports)

| Порт | Протокол | Сервіс |
|------|----------|--------|
| 22 | TCP | SSH |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 27017 | TCP | MongoDB |

```bash
# Переглянути відкриті порти
ss -tlnp
# або
netstat -tlnp
```

---

## Перевірка мережі на сервері

```bash
# Активні з'єднання
ss -tlnp

# Чи слухає порт 80?
ss -tlnp | grep :80

# Перевірка доступності хоста
curl -I http://localhost

# DNS lookup
nslookup google.com

# Маршрутизація
ip route show

# Мережеві інтерфейси
ip addr show
```

---

## Immutable vs Mutable Infrastructure

```
MUTABLE (старий підхід):           IMMUTABLE (сучасний):
                                   
Server існує давно                 Server = одноразовий
      ↓                                  ↓
Update / patch                     Новий AMI → New server
      ↓                                  ↓
Config drift 😱                    Tear down old server ✅
      ↓
"Snowflake server" 🦄
```

> **Cattle not Pets: сервери як худоба, не як домашні тварини**

---

## Immutable Infrastructure Flow

```
┌─────────┐    ┌─────────┐    ┌─────────┐
│  Code   │───►│  Build  │───►│   AMI   │
│ Change  │    │  & Test │    │ (Image) │
└─────────┘    └─────────┘    └────┬────┘
                                   │
                              ┌────▼────┐
                              │ Deploy  │
                              │ New EC2 │
                              └────┬────┘
                                   │
                              ┌────▼────┐
                              │Terminate│
                              │ Old EC2 │
                              └─────────┘
```

---

## Stateless Systems

**12-Factor App — фактор 6: Processes**

```
❌ Stateful (погано):               ✅ Stateless (добре):

Request 1 → Server A              Request 1 → Server A
  зберігає session в пам'яті         зберігає session в Redis
Request 2 → Server B              Request 2 → Server B
  не знає про Request 1! 😱          читає session з Redis ✅
```

**Стан зберігається ЗОВНІ обчислювального ресурсу:**
- Sessions → Redis / DynamoDB
- Files → S3
- Database → RDS

---

## EC2 — Elastic Compute Cloud

```
┌─────────────────────────────────┐
│           EC2 Instance          │
│  ┌───────┐  ┌──────────────┐   │
│  │  AMI  │  │ Instance Type│   │
│  │(Image)│  │  (t3.micro)  │   │
│  └───────┘  └──────────────┘   │
│  ┌──────────────────────────┐  │
│  │     Security Group       │  │
│  └──────────────────────────┘  │
│  ┌──────┐  ┌───────────────┐  │
│  │  EBS │  │   Key Pair    │  │
│  └──────┘  └───────────────┘  │
└─────────────────────────────────┘
```

---

## EC2 Instance Types

| Сімейство | Оптимізовано для | Приклади |
|-----------|-----------------|---------|
| `t3`, `t4g` | General purpose (burst) | Dev, small web |
| `m5`, `m6i` | Balanced CPU/RAM | Production web |
| `c5`, `c6i` | Compute (CPU) | Batch, ML inference |
| `r5`, `r6i` | Memory | Databases, caches |
| `i3`, `i4i` | Storage (NVMe) | High IOPS DB |
| `p3`, `g4` | GPU | ML training |

> **t3.micro → Free Tier eligible**

---

## EC2 Pricing Models

```
┌─────────────────────────────────────────────┐
│  On-Demand    │  Spot        │  Reserved     │
│               │              │               │
│  Pay per hour │  Spare cap.  │  1 or 3 year  │
│  No commit    │  Up to 90%   │  Up to 75%    │
│               │  cheaper     │  cheaper      │
│  ✅ Flexible  │  ✅ Cheap    │  ✅ Predictable│
│  ❌ Expensive │  ❌ Can be   │  ❌ Upfront   │
│               │   terminated │   commitment  │
└─────────────────────────────────────────────┘
```

---

## User Data — Bootstrap Script

```bash
#!/bin/bash
# Цей скрипт виконується при першому запуску EC2

# Оновлення пакетів
dnf update -y

# Встановлення nginx
dnf install nginx -y

# Запуск та enable nginx
systemctl start nginx
systemctl enable nginx

# Кастомна сторінка
echo "<h1>Hello from EC2!</h1>" > /usr/share/nginx/html/index.html
```

> **User Data запускається від root один раз при launch**

---

## AMI — Amazon Machine Image

```
┌────────────────────────────────────────────┐
│              AMI = Template                │
│                                            │
│  ┌────────┐  ┌────────┐  ┌────────────┐  │
│  │  OS    │  │Packages│  │   Config   │  │
│  │(Linux) │  │(nginx, │  │(users, env │  │
│  │        │  │ python)│  │ variables) │  │
│  └────────┘  └────────┘  └────────────┘  │
└────────────────────────────────────────────┘
        │
        ▼ Launch
┌───────────────┐  ┌───────────────┐
│  EC2 Instance │  │  EC2 Instance │
│  (identical)  │  │  (identical)  │
└───────────────┘  └───────────────┘
```

**Golden AMI — основа immutable infrastructure**

---

## Lifecycle: від AMI до Auto Scaling

```
1. Пишемо User Data або Packer script
        │
        ▼
2. Запускаємо базовий EC2, налаштовуємо
        │
        ▼
3. Створюємо AMI (знімок стану)
        │
        ▼
4. Налаштовуємо Launch Template з новим AMI
        │
        ▼
5. Auto Scaling Group використовує Launch Template
        │
        ▼
6. При зміні — новий AMI → новий Launch Template
```

---

## Auto Scaling Group

```
           Traffic ↑↑↑
               │
    ┌──────────▼──────────┐
    │   Auto Scaling Group │
    │  ┌────┐ ┌────┐ ┌────┐│
    │  │EC2 │ │EC2 │ │EC2 ││  ← scale out
    │  └────┘ └────┘ └────┘│
    └─────────────────────-┘
    Min: 2 | Desired: 3 | Max: 10
    
           Traffic ↓↓↓
    ┌──────────────────────┐
    │   Auto Scaling Group │
    │       ┌────┐ ┌────┐  │
    │       │EC2 │ │EC2 │  │  ← scale in
    │       └────┘ └────┘  │
    └──────────────────────┘
```

---

## Essential Linux Commands для Cloud

```bash
# Ресурси системи
top             # CPU та RAM в реальному часі
htop            # Покращена версія top
free -h         # Використання пам'яті
df -h           # Використання диску
iostat          # Disk I/O

# Процеси
ps aux          # Всі запущені процеси
ps aux | grep nginx
kill -9 <PID>   # Примусово завершити процес

# Файли та мережа
tail -f /var/log/nginx/access.log
curl -v https://example.com
wget https://example.com/file.txt
```

---

## SSH: підключення до EC2

```bash
# Підключення через key pair
ssh -i ~/.ssh/my-key.pem ec2-user@<PUBLIC_IP>

# Для Ubuntu (інший default user)
ssh -i ~/.ssh/my-key.pem ubuntu@<PUBLIC_IP>

# Копіювання файлів (SCP)
scp -i ~/.ssh/my-key.pem file.txt ec2-user@<IP>:/home/ec2-user/

# SSH config для зручності (~/.ssh/config)
Host myserver
    HostName 54.123.45.67
    User ec2-user
    IdentityFile ~/.ssh/my-key.pem

# Потім просто:
ssh myserver
```

---

## Hands-On: Launch EC2 з Web Server

```bash
#!/bin/bash
# User Data для EC2

# 1. Оновити систему
dnf update -y

# 2. Встановити nginx
dnf install nginx -y

# 3. Запустити
systemctl start nginx
systemctl enable nginx

# 4. Відкрити порт 80 у Security Group (AWS Console)
# Inbound: HTTP (80) from 0.0.0.0/0

# 5. Перевірити
curl http://localhost
```

> **Результат: публічний IP → nginx working!**

---

## Security: Linux Hardening Basics

```bash
# Заборонити root SSH login
echo "PermitRootLogin no" >> /etc/ssh/sshd_config

# Дозволити тільки key-based auth
echo "PasswordAuthentication no" >> /etc/ssh/sshd_config

# Перезапустити SSH
systemctl restart sshd

# Перевірити відкриті порти
ss -tlnp

# Оновлення безпеки автоматично (AL2023)
dnf install dnf-automatic -y
systemctl enable dnf-automatic-install.timer
```

---

## Ключові висновки

```
✅ Hypervisor → ізоляція VM на фізичному залізі
✅ AWS Nitro → bare-metal продуктивність у хмарі
✅ VM vs Containers vs Serverless → різні trade-offs
✅ Linux-first → 95% production workloads
✅ systemd → управління сервісами (start/enable/status)
✅ Immutable infra → AMI + replace, не patch
✅ Stateless → зберігати стан поза EC2
✅ Auto Scaling → масштабуємо горизонтально
```

---

## Що далі?

**Лекція 6: Зберігання даних та життєвий цикл**
- Object / Block / File storage
- S3, EBS, EFS
- Data lifecycle та backup

**Лабораторна 2: Networking + EC2**
- Запустити власний EC2
- Налаштувати nginx через User Data
- SSH підключення

---

## Питання?

```
       ___
      /   \
     | O O |   "Будь-які питання?"
      \___/
      
  🐧 Linux завжди відповість
     у /var/log/
```

> **Наступного разу: де зберігати дані?**
