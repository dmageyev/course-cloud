# Лекція 5: Обчислювальні ресурси та Linux у хмарі — Конспект

## 1. Обчислювальні ресурси у хмарі

Обчислювальний ресурс — це абстракція, що надає CPU, RAM та мережеві можливості. У хмарі ви орендуєте ресурси, а не фізичне обладнання.

### Еволюція парадигм

| Рівень | Епоха | Overhead | Щільність |
|--------|-------|----------|-----------|
| Bare Metal | 1990s | 0% | 1:1 |
| VM | 2000s | 5–10% | ~10–50 VM/host |
| Container | 2013+ | 1–3% | ~100–1000/host |
| Serverless | 2014+ | ~0% idle | ∞ |

---

## 2. Гіпервізори

**Type 1 (Bare-metal)**: KVM, Xen, VMware ESXi, Hyper-V, AWS Nitro. Запускаються безпосередньо на залізі. Менший overhead, вища продуктивність.

**Type 2 (Hosted)**: VirtualBox, VMware Workstation. Запускаються поверх хост-ОС. Зручні для розробки.

**AWS Nitro System**: кастомні ASIC-чіпи для мережевого та дискового I/O, що звільняє CPU повністю для Guest VM (97–99%).

---

## 3. Virtual Machines, Containers, Serverless

- **VM**: повна ізоляція на рівні kernel, будь-яка ОС, snapshot/migration, важчі за containers
- **Container**: спільний kernel хосту, ізоляція через Linux namespace + cgroups, легкі (MB), швидкий startup
- **Serverless**: тільки runtime, оплата за виклик, без управління сервером, cold start

---

## 4. Linux у хмарі

~90% cloud workloads — Linux. AWS пропонує Amazon Linux 2023 як оптимізований дистрибутив.

**Основні команди**:
- Навігація: `pwd`, `ls -la`, `cd`, `find`
- Файли: `cp`, `mv`, `rm -rf`, `mkdir -p`
- Перегляд: `cat`, `less`, `head`, `tail -f`
- Пошук: `find /etc -name "*.conf"`, `grep -r "error" /var/log/`

---

## 5. systemd та сервіси

systemd — PID 1 у сучасних Linux системах. Управляє сервісами, logging, boot process.

```bash
systemctl start/stop/restart/reload nginx
systemctl status nginx
systemctl enable/disable nginx      # autostart
journalctl -u nginx -f              # лог сервісу
```

Unit файл описує як запускати сервіс (`/etc/systemd/system/myapp.service`).

---

## 6. Мережа та порти в Linux

```bash
ip addr show          # IP адреси
ss -tlnp              # відкриті порти
curl -I https://...   # HTTP заголовки
```

Порти 0–1023 потребують root. Security Group в AWS — перший рівень захисту, потім iptables/nftables.

---

## 7. Файлова система та логи

**FHS (Filesystem Hierarchy Standard)**:
- `/etc/` — конфіги
- `/var/log/` — логи
- `/opt/` — сторонні застосунки
- `/tmp/` — тимчасові файли (ephemeral!)

**journald**: `journalctl -u service -f --since "1h ago"`

---

## 8. Bash scripting

```bash
#!/bin/bash
NAME="cloud"
if [ -f "/etc/nginx/nginx.conf" ]; then echo "nginx found"; fi
for s in web-01 web-02; do ping -c1 $s && echo UP; done
function check() { systemctl is-active --quiet $1 || systemctl start $1; }
```

---

## 9. SSH та доступ

```bash
ssh -i ~/.ssh/key.pem ec2-user@IP
```

Типові usernames: `ec2-user` (Amazon Linux), `ubuntu` (Ubuntu), `admin` (Debian).
**AWS Systems Manager Session Manager** — доступ без відкритого порту 22.

---

## 10. Immutable Infrastructure

Принцип: сервери не змінюються — вони замінюються. Golden AMI → Launch Template → Auto Scaling Group.

Переваги: відтворюваність, відсутність config drift, легкий rollback, безпека.

---

## 11. Stateless vs Stateful

**Stateless**: HTTP сервер не зберігає стан між запитами. Сесії → Redis, файли → S3, конфіги → Parameter Store.

**Stateful**: БД, черги. Потребують окремого підходу до scaling.

---

## 12. AWS EC2

- **AMI**: образ ОС (Amazon Linux 2023, Ubuntu)
- **Instance Types**: t3 (burstable), m6i (general), c6i (compute), r6i (memory), p3 (GPU)
- **Purchasing**: On-Demand, Reserved (1/3yr), Savings Plans, Spot (до 90% дешевше)
- **Auto Scaling Group**: min/desired/max, scaling policies, cooldown
- **Launch Template**: versioned конфіг інстансу
- **User Data**: cloud-init скрипт при першому boot
