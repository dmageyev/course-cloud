# Лекція 6: Зберігання даних та життєвий цикл — Конспект

## 1. Типи сховищ даних

**Object Storage (S3)**: плоска namespace, HTTP REST API, нескінченна масштабованість. Для backup, статики, Data Lake.

**Block Storage (EBS)**: томи підключаються до EC2. Аналог жорсткого диску. Для БД, ОС дисків.

**File Storage (EFS, FSx)**: NFS/SMB протокол. Shared filesystem між EC2 instances. Для CMS, ML training data.

---

## 2. Amazon S3

- Глобально унікальна назва bucket
- Об'єкт = binary data + metadata + унікальний key
- Клас зберігання: Standard, Standard-IA, Glacier, Deep Archive, Intelligent-Tiering
- Версіонування: захист від випадкового видалення
- Lifecycle policies: автоматичний перехід між класами → економія до 70%
- Event Notifications: → Lambda, SQS, SNS, EventBridge

---

## 3. Data Lifecycle

```
Hot Data (Standard) →[30d]→ Warm (Standard-IA) →[90d]→ Cold (Glacier) →[7yr]→ Delete
```

Lifecycle Rule: JSON конфіг в S3 bucket. Автоматизує переміщення та видалення.

---

## 4. S3 Versioning та Replication

**Versioning**: кожне PUT = нова версія. DELETE = delete marker (не видаляє!).
**CRR** (Cross-Region Replication): DR, latency reduction.
**SRR** (Same-Region Replication): агрегація логів, dev копія prod.

---

## 5. EBS — Block Storage

Типи: gp3 (general, default), io2 (critical DB), st1 (big data), sc1 (cold).

**gp3 vs gp2**: gp3 на 20% дешевше, IOPS та throughput налаштовуються незалежно.

**EBS Snapshots**: інкрементальні, зберігаються в S3, копіювання між regions.

---

## 6. EFS та FSx

**EFS**: NFS v4, multi-AZ, автоматичне масштабування, Linux workloads.
**FSx for Windows**: SMB, Active Directory.
**FSx for Lustre**: HPC, ML, sub-ms latency.

---

## 7. CAP Теорема

Distributed система не може одночасно гарантувати:
- **C**onsistency (всі вузли бачать однакові дані)
- **A**vailability (кожен запит отримує відповідь)
- **P**artition Tolerance (система працює при розриві мережі)

P — завжди в реальній мережі. Обирай C або A.

S3 з 2020 = strongly consistent (CP для більшості операцій).

---

## 8. Security

- Block Public Access: завжди ON
- Bucket Policy: deny non-HTTPS
- SSE-KMS: для чутливих даних (audit trail)
- S3 Object Lock (WORM): compliance, FINRA, SEC 17a-4

---

## 9. GDPR та Data Residency

- EU дані → eu-central-1 або eu-west-1
- AWS не переміщує дані між regions без дозволу
- Інструменти: SCPs, AWS Config Rules
- Логи та метадані теж = персональні дані
