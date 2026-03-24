# Лекція 3: Безпека хмарних ІС: DevSecOps mindset — Слайди

---

## Slide 1: Назва

# Лекція 3: Безпека хмарних ІС: DevSecOps mindset

---

## Slide 2: Shift Left Security

```
Dev → [Security] → Staging → [Security] → Prod
 ↑                                          ↑
Дешевше виявити тут             Дорого виправляти тут
```

DevSecOps = безпека на кожному етапі SDLC, не наприкінці.

---

## Slide 3: Топ помилок конфігурації

```
❌ S3 bucket: публічний доступ
❌ Security Group: SSH/RDP → 0.0.0.0/0
❌ IAM user без MFA
❌ Root account активний
❌ CloudTrail вимкнений
❌ Незашифровані EBS volumes
```

99% security failures = помилки конфігурації клієнта (Gartner)

---

## Slide 4: IAM Компоненти

| Компонент | Що це | Credentials |
|-----------|-------|-------------|
| User | Людина / сервіс | Довгострокові |
| Group | Колекція users | — |
| Role | Ідентичність для сервісу | Тимчасові |
| Policy | JSON правила доступу | — |

---

## Slide 5: Least Privilege

```json
// ❌ Погано:
{"Effect":"Allow","Action":"s3:*","Resource":"*"}

// ✅ Добре:
{"Effect":"Allow",
 "Action":["s3:GetObject"],
 "Resource":"arn:aws:s3:::my-bucket/reports/*"}
```

---

## Slide 6: Zero Trust

- "Never trust, always verify"
- Кожен запит = автентифікація + авторизація
- Мережа не є периметром безпеки
- mTLS між сервісами

---

## Slide 7: Security Groups vs NACL

| | Security Group | NACL |
|--|--|--|
| Рівень | Instance | Subnet |
| Stateful | ✅ | ❌ |
| Default inbound | Deny all | Allow all |

---

## Slide 8: Dev / Staging / Prod

- Окремі AWS accounts = найсильніша ізоляція
- Або окремі VPC з мінімальним routing між ними
- Prod дані ≠ Dev середовище
- Різні IAM ролі для кожного оточення

---

## Slide 9: AWS Security Services

| Сервіс | Призначення |
|--------|-------------|
| IAM | Identity & Access Management |
| AWS Config | Compliance monitoring |
| CloudTrail | Audit logs API calls |
| GuardDuty | Threat detection (ML) |
| Security Hub | Centralized dashboard |

---

## Slide 10: Ключові висновки

1. Misconfiguration — #1 ризик у хмарі
2. IAM Role > IAM User для сервісів
3. Least Privilege — завжди
4. Zero Trust — не довіряй мережі
5. Розділяй оточення (окремі accounts)
