# Лекція 10: DevOps, IaC та економіка хмари — Слайди

---

## Slide 1: Назва

# Лекція 10: DevOps, IaC та економіка хмари

---

## Slide 2: DevOps культура

```
Dev Team    Ops Team
     \      /
      DevOps
     /      \
Швидкість   Стабільність

CALMS:
  Culture, Automation, Lean, Measurement, Sharing
```

---

## Slide 3: CI/CD Pipeline

```
git push
    ↓
[CI] Build → Unit Tests → Integration → Docker build → ECR
    ↓
[CD] Deploy Staging → Smoke Tests → Approval → Deploy Prod
    ↓
Health checks → Rollback if fails
```

AWS: CodePipeline + CodeBuild. Або GitHub Actions.

---

## Slide 4: Infrastructure as Code

| Підхід | Інструмент | Переваги |
|--------|-----------|----------|
| ClickOps | AWS Console | Простіше спочатку |
| Scripts | Bash, AWS CLI | Автоматизація |
| IaC | Terraform, CDK | Версіонування, review |

→ Production = завжди IaC

---

## Slide 5: Terraform Workflow

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abc12345"
  instance_type = "t3.micro"
}
```

```
terraform init   → завантажити providers
terraform plan   → показати зміни
terraform apply  → застосувати
terraform destroy → видалити
```

---

## Slide 6: Terraform State

```
terraform.tfstate (зберігай у S3!)

Remote backend:
  terraform {
    backend "s3" {
      bucket = "tf-state-bucket"
      key    = "prod/terraform.tfstate"
      region = "eu-central-1"
    }
  }
```

DynamoDB = distributed lock для team роботи

---

## Slide 7: FinOps

```
Inform:    Cost Explorer, Budgets, Tags
Optimize:  Reserved, Savings Plans, Right-sizing
Operate:   Continuous monitoring, automation

$ перед deploy:
  aws ce get-cost-forecast --...
  → очікувані витрати
```

---

## Slide 8: CloudWatch Observability

```
Metrics → Алерти → Auto Scaling
Logs    → Logs Insights → Аналіз проблем
Traces  → X-Ray → Distributed tracing

Dashboard: один екран стану системи
```

---

## Slide 9: AWS Well-Architected

| Стовп | Ключова ідея |
|-------|-------------|
| Operational Excellence | Автоматизація операцій |
| Security | Defense in depth |
| Reliability | Design for failure |
| Performance | Right-size resources |
| Cost | Pay only for what you use |
| Sustainability | Ефективне використання |

---

## Slide 10: Ключові висновки

1. DevOps = культура + автоматизація
2. CI/CD = швидкий надійний деплой
3. IaC = інфраструктура як код (Terraform = стандарт)
4. FinOps = collaborative cost optimization
5. Observability = Metrics + Logs + Traces
6. Well-Architected = чекліст production систем
