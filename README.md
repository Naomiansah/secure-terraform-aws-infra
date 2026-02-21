# ShopNow Dev — Secure AWS Network Foundation (Terraform)

This repository is a hands-on Infrastructure-as-Code project where I design and deploy a production-style AWS networking baseline for a fictional e-commerce platform called **ShopNow**.

The goal is to demonstrate real cloud architecture skills (networking, security mindset, environment separation) using **Terraform**.

---

## What has been implemented (Phase 1)

### ✅ Multi-AZ VPC foundation

- Custom **VPC**: `10.0.0.0/16`
- **2 Availability Zones** (eu-west-1a, eu-west-1b)
- **2 Public Subnets** (one per AZ)
- **2 Private Subnets** (one per AZ)
- **Internet Gateway** attached to the VPC
- **Public Route Table** with default route `0.0.0.0/0 → IGW`
- Public route table associated to both public subnets

### Why this matters (architecture reasoning)

This layout forms the baseline for real applications:

- **Public subnets** host internet-facing components (e.g., Load Balancer, NAT Gateway)
- **Private subnets** host internal systems (e.g., app servers, databases)
- Multi-AZ design supports **high availability**

---

## Network design (CIDR plan)

- VPC: `10.0.0.0/16`
- Public subnets: `10.0.1.0/24`, `10.0.2.0/24`
- Private subnets: `10.0.101.0/24`, `10.0.102.0/24`

---

## Project roadmap

- [x] Phase 1: VPC + subnets + routing (current)
- [ ] Phase 2: Security Groups + Dev web server (EC2)
- [ ] Phase 3: NAT Gateway + private route table (controlled outbound)
- [ ] Phase 4: Load Balancer + private app tier (more realistic)
- [ ] Phase 5: Remote state (S3 + DynamoDB locking)
- [ ] Phase 6: Logging & security (CloudTrail, VPC Flow Logs)

---

## How to run

### Prerequisites

- Terraform installed
- AWS credentials configured
- AWS account (Free Tier supported; avoid NAT/ALB running for long periods)

### Plan

```bash
terraform init
terraform fmt
terraform validate
terraform plan -var-file="dev.tfvars"
```

<!-- Apply -->

terraform apply -var-file="dev.tfvars"

<!-- destroy -->

terraform destroy -var-file="dev.tfvars"

<!-- Notes on version control -->

State files are excluded from git via .gitignore (best practice).

The provider lock file .terraform.lock.hcl is committed to ensure consistent provider versions.
