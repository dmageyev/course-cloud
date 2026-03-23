# Лекція 5: Ресурси та посилання

## Офіційна документація AWS

### EC2
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
- [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
- [EC2 Pricing](https://aws.amazon.com/ec2/pricing/)
- [Amazon Machine Images (AMI)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
- [User Data for EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)
- [AWS Nitro System](https://aws.amazon.com/ec2/nitro/)
- [Instance Metadata Service v2 (IMDSv2)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html)

### Auto Scaling
- [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)
- [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html)
- [Scaling Policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html)

### Security
- [IAM Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)
- [EC2 Security Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html)
- [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)

---

## Linux та systemd

- [systemd Documentation](https://www.freedesktop.org/wiki/Software/systemd/)
- [systemctl Man Page](https://www.man7.org/linux/man-pages/man1/systemctl.1.html)
- [journalctl Man Page](https://www.man7.org/linux/man-pages/man1/journalctl.1.html)
- [The Linux Command Line (book, free online)](https://linuxcommand.org/tlcl.php)
- [Linux Journey — interactive learning](https://linuxjourney.com/)

---

## Архітектурні концепції

### VM vs Containers vs Serverless
- [AWS: Choose the right compute option](https://docs.aws.amazon.com/decision-guides/latest/compute-on-aws/compute-on-aws.html)
- [Containers vs VMs (Red Hat)](https://www.redhat.com/en/topics/containers/containers-vs-vms)
- [AWS Lambda vs EC2 — when to use what](https://aws.amazon.com/blogs/compute/choosing-between-aws-lambda-and-aws-fargate/)

### Immutable Infrastructure
- [Immutable Infrastructure (HashiCorp)](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure)
- [Packer by HashiCorp — Golden AMI builder](https://developer.hashicorp.com/packer/docs)
- [Kief Morris: Infrastructure as Code, Chapter on Immutable Servers](https://infrastructure-as-code.com/)

### 12-Factor App
- [12factor.net](https://12factor.net/) — оригінальний ресурс
- [12 Factor App — Factor VI: Processes](https://12factor.net/processes)

---

## AWS Blog та практичні статті

- [Building a Golden AMI Pipeline](https://aws.amazon.com/blogs/awsmarketplace/announcing-the-golden-ami-pipeline/)
- [EC2 Auto Scaling Best Practices](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-best-practices.html)
- [Security Best Practices for EC2](https://aws.amazon.com/blogs/security/security-best-practices-for-ec2-instances/)
- [Migrate from IMDSv1 to IMDSv2](https://aws.amazon.com/blogs/security/defense-in-depth-open-firewalls-reverse-proxies-ssrf-vulnerabilities-ec2-instance-metadata-service/)

---

## Сертифікаційні ресурси

- [AWS Certified Solutions Architect - Associate](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
  - Домен 1: Design Resilient Architectures (охоплює EC2, Auto Scaling)
- [AWS Certified SysOps Administrator](https://aws.amazon.com/certification/certified-sysops-admin-associate/)
  - Фокус на EC2 management, Linux troubleshooting

---

## Інструменти

| Інструмент | Призначення | Посилання |
|-----------|-------------|-----------|
| AWS CLI | Управління EC2 через CLI | [docs.aws.amazon.com/cli](https://docs.aws.amazon.com/cli/latest/reference/ec2/) |
| Packer | Створення Golden AMI | [developer.hashicorp.com/packer](https://developer.hashicorp.com/packer) |
| Ansible | Configuration management | [docs.ansible.com](https://docs.ansible.com/) |
| SSM Session Manager | SSH-less access до EC2 | [AWS SSM docs](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) |
| htop | Interactive process viewer | [htop.dev](https://htop.dev/) |

---

## Для самостійного вивчення

### Безкоштовні курси та лабораторії
- [AWS Skill Builder — EC2 Getting Started](https://skillbuilder.aws/)
- [AWS Free Tier — практика на реальному AWS](https://aws.amazon.com/free/)
- [KodeKloud — Linux for Beginners](https://kodekloud.com/courses/the-linux-basics-course/)
- [OverTheWire: Bandit — Linux wargame](https://overthewire.org/wargames/bandit/)

### Книги
- **"The Linux Command Line"** by William Shotts (безкоштовно онлайн)
- **"How Linux Works"** by Brian Ward
- **"Infrastructure as Code"** by Kief Morris (O'Reilly)
- **"AWS in Action"** by Michael Wittig & Andreas Wittig (Manning)

---

## Пов'язані теми курсу

| Лекція | Тема | Зв'язок |
|--------|------|---------|
| Лекція 4 | Мережева архітектура | VPC, Security Groups для EC2 |
| Лекція 6 | Зберігання даних | EBS для EC2, S3 для stateless |
| Лекція 8 | Масштабування | Auto Scaling Group, ALB |
| Лабораторна 2 | Networking + EC2 | Практика: запуск EC2 |
