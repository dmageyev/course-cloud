# Лекція 10: DevOps, IaC та економіка хмари — Конспект

## 1. DevOps та DevSecOps

**DevOps**: культура та практики, що об'єднують розробку (Dev) та операції (Ops) для швидшого delivery.

**Ключові принципи**:
- CI/CD (Continuous Integration / Continuous Delivery)
- Infrastructure as Code (IaC)
- Моніторинг та Observability
- Collaboration та shared responsibility

**DevSecOps**: Security інтегрується на кожному етапі (shift-left), а не наприкінці.

---

## 2. CI/CD Pipelines

```
Code Commit → CI → CD → Production

CI (Continuous Integration):
  git push
    → Unit Tests
    → Integration Tests
    → Build Docker image
    → Push to ECR
    → Security scan (SAST)

CD (Continuous Delivery):
  → Deploy to Staging
  → Smoke Tests
  → Manual/Auto approval
  → Deploy to Production
  → Health checks
```

**AWS інструменти**: CodePipeline, CodeBuild, CodeDeploy. Або: GitHub Actions, GitLab CI.

---

## 3. Infrastructure as Code (IaC)

Інфраструктура описується кодом, а не клікається вручну.

**Переваги**:
- Версіонування (git history)
- Відтворюваність (однакові середовища)
- Code Review для infrastructure
- Автоматизація (CI/CD для infra)
- Документація = сам код

---

## 4. Terraform — Industry Standard

**HashiCorp Terraform**: декларативний IaC інструмент.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abc12345"
  instance_type = "t3.micro"

  tags = {
    Name        = "web-server"
    Environment = "production"
  }
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

**Workflow**: `terraform init` → `terraform plan` → `terraform apply`

**State file**: `terraform.tfstate` — зберігає поточний стан. Зберігай у S3 + DynamoDB locking для команд.

**Terraform Registry**: [registry.terraform.io](https://registry.terraform.io) — готові modules.

---

## 5. AWS CloudFormation

**CloudFormation**: рідний AWS IaC. YAML/JSON шаблони.

```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-app-storage
      VersioningConfiguration:
        Status: Enabled
```

**Переваги над Terraform**: нативна інтеграція з AWS, Stack sets для multi-account.
**Недоліки**: тільки AWS, складніший синтаксис.

---

## 6. FinOps — Cloud Economics

**FinOps**: практика оптимізації хмарних витрат через collaboration між Engineering, Finance та Business.

**Цикл**: Inform → Optimize → Operate

**AWS Cost Tools**:
- **Cost Explorer**: аналіз та прогнозування витрат
- **AWS Budgets**: алерти при перевищенні бюджету
- **Trusted Advisor**: рекомендації з оптимізації
- **Compute Optimizer**: правильний розмір EC2/RDS

---

## 7. Observability

**Three Pillars of Observability**:
1. **Metrics**: числові показники (CPU, latency, error rate) → CloudWatch Metrics
2. **Logs**: структуровані події → CloudWatch Logs
3. **Traces**: шлях запиту через сервіси → AWS X-Ray

**AWS CloudWatch**:
- Dashboards: візуалізація
- Alarms: тригери для AutoScaling, SNS
- Logs Insights: SQL-подібні запити по логах

---

## 8. AWS Well-Architected Framework

6 стовпів:
1. **Operational Excellence**: автоматизація операцій
2. **Security**: захист даних та систем
3. **Reliability**: відновлення після збоїв
4. **Performance Efficiency**: правильні ресурси
5. **Cost Optimization**: уникнення непотрібних витрат
6. **Sustainability**: зменшення впливу на довкілля
