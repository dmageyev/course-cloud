# Лекція 4: Мережева архітектура хмар — Слайди

---

## Slide 1: Назва

# Лекція 4: Мережева архітектура хмар

---

## Slide 2: VPC — Virtual Private Cloud

```
AWS Region (eu-central-1)
  └── VPC: 10.0.0.0/16
        ├── AZ-a: subnet 10.0.1.0/24 (public)
        │         subnet 10.0.2.0/24 (private)
        └── AZ-b: subnet 10.0.3.0/24 (public)
                  subnet 10.0.4.0/24 (private)
```

VPC = ваша ізольована мережа в AWS

---

## Slide 3: CIDR — IP Range Notation

```
10.0.0.0/8   → 16,777,214 хостів
10.0.0.0/16  →     65,534 хостів
10.0.0.0/24  →        254 хостів
10.0.0.0/28  →         14 хостів
```

AWS резервує 5 адрес у кожній subnet → /24 дає 251 usable

---

## Slide 4: Public vs Private Subnet

| | Public | Private |
|--|--------|---------|
| Internet Gateway | ✅ | ❌ |
| Public IP | Можливо | Ні |
| Розміщення | ALB, NAT GW | EC2, RDS |

---

## Slide 5: Route Tables

```
Public Route Table:      Private Route Table:
  10.0.0.0/16 → local     10.0.0.0/16 → local
  0.0.0.0/0   → IGW       0.0.0.0/0   → NAT GW
```

---

## Slide 6: NAT Gateway

```
Private EC2 → NAT Gateway (public subnet) → Internet
          ↑                              ↑
     outbound only              stateful, blocks inbound
```

NAT Gateway: AWS managed, ~$0.045/год + $0.045/GB

---

## Slide 7: 3-tier Architecture

```
Internet
   │
 ALB  ──── public subnet
   │
App Server ── private subnet (Security Group: allow ALB)
   │
Database ── private subnet (Security Group: allow App only)
```

---

## Slide 8: Security Groups vs NACL

| | Security Group | NACL |
|--|--|--|
| Рівень | Instance | Subnet |
| Stateful | ✅ | ❌ |
| Rules | Allow only | Allow + Deny |
| Processing | All rules | By order number |

---

## Slide 9: Bastion Host vs Session Manager

```
Традиційно:         Сучасно:
  Dev               Dev
   └── SSH → Bastion → EC2    └── HTTPS → SSM → EC2
             (port 22 open)        (no open ports)
```

Session Manager: аудит, IAM контроль, без key management

---

## Slide 10: Ключові висновки

1. VPC = ізольована мережа у регіоні
2. Public subnet = IGW. Private subnet = NAT GW
3. Routing table = мозок мережі
4. Least privilege для Security Groups
5. 3-tier: internet → ALB → App → DB
