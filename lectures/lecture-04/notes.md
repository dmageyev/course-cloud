# Лекція 4: Мережева архітектура хмар — Конспект

## 1. VPC як логічна ізоляція

**VPC (Virtual Private Cloud)** — логічно ізольована мережа в AWS. Аналог фізичної мережі датацентру, але повністю програмований.

**Характеристики VPC**:
- Прив'язаний до одного регіону
- Одна або кілька AZ
- CIDR блок (наприклад, `10.0.0.0/16`)
- Ізольований від інших VPC за замовчуванням

---

## 2. CIDR та IP-адресація

**CIDR (Classless Inter-Domain Routing)**: нотація для позначення IP діапазонів.

```
10.0.0.0/16   → 65,534 хостів (255.255.0.0 маска)
10.0.1.0/24   → 254 хости    (255.255.255.0 маска)
10.0.1.0/28   → 14 хостів    (255.255.255.240 маска)
```

AWS резервує 5 адрес у кожній підмережі:
- Network address (перша)
- VPC router (друга)
- DNS (третя)
- Future use (четверта)
- Broadcast (остання)

---

## 3. Public vs Private Subnet

**Public Subnet**: пов'язана з Internet Gateway (IGW). Ресурси можуть мати публічний IP.

**Private Subnet**: немає прямого доступу до інтернету. Для виходу в інтернет потрібен NAT Gateway.

**Best Practice Architecture**:
```
Internet
    │
  IGW
    │
Public Subnet (10.0.1.0/24)
  ALB, Bastion Host, NAT Gateway
    │
Private Subnet (10.0.2.0/24)
  EC2 (App servers)
    │
Private Subnet (10.0.3.0/24)
  RDS (Database)
```

---

## 4. Routing та NAT

**Route Table**: правила маршрутизації для підмережі.

```
Public Route Table:
  10.0.0.0/16  → local
  0.0.0.0/0    → igw-0abc123  (internet)

Private Route Table:
  10.0.0.0/16  → local
  0.0.0.0/0    → nat-0abc123  (NAT Gateway)
```

**NAT Gateway**: дозволяє ресурсам у приватній підмережі ініціювати з'єднання в інтернет, але блокує вхідні з'єднання. Розміщується в публічній підмережі.

---

## 5. Ingress / Egress

**Ingress**: трафік що входить у систему (від клієнтів).
**Egress**: трафік що виходить із системи (до зовнішніх сервісів).

**Security Group правила**:
- Inbound rules: контроль вхідного трафіку
- Outbound rules: контроль вихідного трафіку (default: allow all)

---

## 6. Bastion Host

**Bastion Host (Jump Server)**: публічний EC2 в Public Subnet для SSH доступу до приватних ресурсів.

```
Developer → Bastion (public) → Private EC2
            SSH:22 allowed     SSH:22 від bastion IP
```

Альтернатива: **AWS Systems Manager Session Manager** — без Bastion, без відкритого порту 22.

---

## 7. 2-tier та 3-tier Architecture

**2-tier**:
```
Internet → ALB (public) → App Server (private)
```

**3-tier (N-tier)**:
```
Internet → ALB (public) → App Server (private) → DB (private)
```

Кожен шар у своїй підмережі, доступ між шарами через Security Groups.
