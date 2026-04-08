# Лекція 3: Безпека хмарних ІС: DevSecOps mindset — Конспект

## 1. Threat Model у хмарі

**Threat modeling**: систематичний аналіз потенційних загроз для системи.

**STRIDE модель**:
- **S**poofing — підробка ідентичності
- **T**ampering — модифікація даних
- **R**epudiation — заперечення дій
- **I**nformation Disclosure — витік даних
- **D**enial of Service — відмова в обслуговуванні
- **E**levation of Privilege — підвищення привілеїв

**Типові вектори атак на хмарні системи**:
1. Compromised credentials (витік AWS Access Key)
2. Misconfiguration (відкритий S3 bucket, Security Group 0.0.0.0/0)
3. Vulnerable dependencies (Log4Shell, Spring4Shell)
4. Insider threat
5. Supply chain attacks

---

## 2. Misconfiguration — головний ризик

За даними Gartner, 99% security failures до 2025 будуть викликані помилками клієнтів, а не провайдерів.

**Топ помилок конфігурації AWS**:
- S3 bucket з публічним доступом
- Security Group: SSH/RDP відкрито для 0.0.0.0/0
- IAM users з надмірними правами (або без MFA)
- Незашифровані EBS volumes
- CloudTrail вимкнений
- Root account без MFA

---

## 3. IAM як архітектурний компонент

**Identity and Access Management** — серце безпеки AWS.

**Компоненти IAM**:
- **User**: людина або сервіс з довгостроковими credentials
- **Group**: колекція users зі спільними правами
- **Role**: тимчасові credentials, призначаються сервісам/людям
- **Policy**: JSON документ з дозволами (`Effect`, `Action`, `Resource`)

```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject", "s3:PutObject"],
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

**IAM Role для EC2**: інстанс отримує тимчасові credentials автоматично. Ніколи не захардкоджуй AWS keys у коді!

---

## 4. Least Privilege Principle

Давай мінімальні необхідні права. Не `s3:*` якщо потрібно тільки `s3:GetObject`.

**Практика**: AWS IAM Access Analyzer — автоматично визначає надмірні права.

---

## 5. Zero Trust

**Традиційна модель**: довіряй всередині периметру.
**Zero Trust**: "Never trust, always verify" — кожен запит автентифікується, авторизується, шифрується незалежно від мережі.

В AWS: VPC не є периметром безпеки. IAM + Security Groups + NACLs + encryption = multi-layer.

---

## 6. Dev / Staging / Prod розділення

- Окремі AWS рахунки (accounts) — найсильніша ізоляція
- Або окремі VPC з мінімальним доступом між ними
- Prod данні ніколи не повинні потрапляти в Dev середовище
- Different IAM roles for different environments

---

## 7. AWS IAM практика

**Security Groups vs NACLs**:

| | Security Group | NACL |
|--|----------------|------|
| Рівень | Instance-level | Subnet-level |
| Stateful | ✅ | ❌ |
| Default | Deny all inbound | Allow all |

**MFA**: завжди для root та привілейованих користувачів.
