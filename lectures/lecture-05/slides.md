# Лекція 5: Обчислювальні ресурси та Linux у хмарі — Слайди (Markdown)

<!-- Markdown версія слайдів. HTML версія: slides.html -->

---

## Slide 1: Назва

# Лекція 5: Обчислювальні ресурси та Linux у хмарі

---

## Slide 2: Еволюція обчислень

Bare Metal → VM → Container → Serverless → AI Instances

| Рівень | Startup | Ізоляція | AWS |
|--------|---------|----------|-----|
| VM | секунди | kernel | EC2 |
| Container | мс | process | ECS/EKS |
| Serverless | мс (warm) | runtime | Lambda |

---

## Slide 3: Гіпервізори Type 1 vs Type 2

**Type 1**: KVM, Xen, ESXi, Hyper-V, AWS Nitro — напряму на залізі
**Type 2**: VirtualBox, VMware Workstation — через хост-ОС

---

## Slide 4: AWS Nitro System

- Виносить I/O на окремі ASIC-чіпи
- CPU Guest VM: ~97–99% (не витрачається на гіпервізор)
- Nitro Security Chip — hardware root of trust
- Всі сучасні EC2 (m5+, c5+, r5+) — на Nitro

---

## Slide 5: VM vs Container vs Serverless

| | VM | Container | Serverless |
|--|--|--|--|
| ОС | окрема | спільна | runtime |
| RAM overhead | ~512MB+ | ~5MB | ~0 idle |
| AWS | EC2 | ECS/EKS | Lambda |

---

## Slide 6: Linux-first mindset

- ~90% cloud workloads = Linux
- Amazon Linux 2023 — AWS-optimized
- Open source, автоматизація, SSH, менша attack surface

---

## Slide 7: Базові команди Linux

```bash
ls -la           # список файлів
tail -f log.txt  # real-time лог
find /etc -name "*.conf"
grep -r "error" /var/log/
ss -tlnp         # відкриті порти
```

---

## Slide 8: systemd

```bash
systemctl start/stop/restart nginx
systemctl enable nginx     # autostart
journalctl -u nginx -f     # лог
```

---

## Slide 9: User Data (cloud-init)

```bash
#!/bin/bash
dnf update -y
dnf install -y nginx
systemctl enable --now nginx
```

---

## Slide 10: Immutable Infrastructure

- Сервери не змінюються — замінюються
- Golden AMI → Launch Template → ASG
- Нема config drift, легкий rollback

---

## Slide 11: Stateless системи

- HTTP сервер без локального стану
- Сесії → Redis (ElastiCache)
- Файли → S3
- Конфіги → Parameter Store

---

## Slide 12: EC2 Instance Types

| Сімейство | Тип | Use Case |
|-----------|-----|----------|
| T | Burstable | Dev/test |
| M | General | Web/app |
| C | Compute | CPU-intensive |
| R | Memory | In-memory DB |
| P/G | GPU | ML training |

---

## Slide 13: EC2 Purchasing Options

| Тип | Знижка | Use Case |
|-----|--------|----------|
| On-Demand | 0% | Dev, нерегулярне |
| Reserved 1yr | ~40% | Stable baseline |
| Savings Plans | до 66% | Flexible |
| Spot | до 90% | Batch, ML |

---

## Slide 14: Auto Scaling Group

- Min / Desired / Max capacity
- Target Tracking: CPU = 60%
- Step Scaling: поступове збільшення
- Scheduled: за розкладом

---

## Slide 15: Ключові висновки

1. Вибір VM/Container/Serverless = архітектурне рішення
2. Linux — стандарт хмарних workloads
3. Immutable infra = відтворюваність та безпека
4. Stateless = легкий horizontal scaling
5. EC2: правильний тип + правильна purchasing option = cost efficiency
