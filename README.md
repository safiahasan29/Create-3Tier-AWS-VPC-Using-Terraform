# 3-Tier AWS VPC (Public / Private / DB Subnets) with NAT Gateways — Terraform

This project demonstrates how to design and provision a **production-style 3-tier AWS VPC** using **Terraform** and the widely adopted **terraform-aws-modules/vpc/aws** module.

The repository showcases two approaches:
- **v1 (Hardcoded demo)** – Quick setup for learning and validation
- **v2 (Standardized & reusable)** – Parameterized, modular, and production-ready design

---

## What You’ll Build

A highly available 3-tier network across **two Availability Zones**:

- **Public subnets** – for ALB, bastion hosts, or public-facing services  
- **Private application subnets** – for EC2, EKS worker nodes, internal services  
- **Private database subnets** – for RDS or other database workloads  

Outbound internet access for private subnets is enabled via a **NAT Gateway with Elastic IP**.

### Included Components
- Amazon VPC with custom CIDR
- Internet Gateway (IGW)
- Public, Private, and Database subnets
- Route tables and routing rules
- NAT Gateway for outbound access
- Optional DB subnet group and route table
- Standardized tagging strategy

---

## Architecture Overview

*(Recommended: add a simple diagram under `/images/vpc-3tier-terraform.png`)*

Key design highlights:
- Public subnets route traffic to the Internet Gateway
- Private subnets route outbound traffic via NAT Gateway
- Database subnets remain isolated (no direct internet access)
- Multi-AZ design for high availability

---

## Repository Structure

```
terraform-manifests/
├── v1-vpc-module/                  # Hardcoded demo version
└── v2-vpc-module-standardized/     # Standardized, reusable version
```

---

## Prerequisites

- Terraform installed (latest stable recommended)
- AWS CLI configured (`aws configure`)
- AWS account with permissions to create VPC networking resources

---

## Quick Start – v1 (Hardcoded Demo)

### Navigate to folder
```
cd terraform-manifests/v1-vpc-module
```

### Initialize Terraform
```
terraform init
```

### Validate configuration
```
terraform validate
```

### Plan infrastructure
```
terraform plan
```

### Apply configuration
```
terraform apply -auto-approve
```

### Verify in AWS Console
- VPC and subnets created
- Internet Gateway attached
- NAT Gateway and Elastic IP provisioned
- Correct routing for public, private, and database subnets
- Tags applied as expected

### Destroy resources
```
terraform destroy -auto-approve
```

### Cleanup local files
```
rm -rf .terraform* terraform.tfstate*
```

---

## Standardized Version – v2 (Production-Style)

### Navigate to folder
```
cd terraform-manifests/v2-vpc-module-standardized
```

### Key Concepts Used
- Input variables (generic and VPC-specific)
- Local values for naming and common tags
- `terraform.tfvars` for generic configuration
- `vpc.auto.tfvars` for VPC-specific values
- Output values for downstream integrations

### Important Files
- `c2-generic-variables.tf` – region, environment, business division
- `c3-local-values.tf` – naming conventions and tags
- `c4-01-vpc-variables.tf` – VPC inputs
- `c4-02-vpc-module.tf` – VPC module definition
- `c4-03-vpc-outputs.tf` – outputs
- `terraform.tfvars` – generic values
- `vpc.auto.tfvars` – VPC values (auto-loaded)

### Execute Terraform
```
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

Destroy and cleanup steps are the same as v1.

---

## Terraform Module Used

This project uses the community-maintained module:
- **terraform-aws-modules/vpc/aws**

Best practices when using public registry modules:
- Verify HashiCorp Verified badge
- Review download count and release history
- Pin module versions for stability
- Validate inputs, outputs, and dependencies

---

## Outputs

After successful deployment, the following outputs are available:
- VPC ID
- VPC CIDR block
- Public subnet IDs
- Private subnet IDs
- Availability Zones
- NAT Gateway public IPs

---

## Cost & Design Considerations

- A **single NAT Gateway** is used by default to reduce demo costs
- For production environments, consider **one NAT Gateway per AZ**
- Optional enhancements:
  - VPC Endpoints (S3, DynamoDB, ECR)
  - VPC Flow Logs
  - CI/CD automation for Terraform

---

