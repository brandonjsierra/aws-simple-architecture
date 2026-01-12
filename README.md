# AWS VPC + EC2 + S3 (Terraform)

This repository contains a small, intentionally scoped AWS environment built with Terraform. The purpose of this project is to demonstrate solid foundational architecture skills—networking, security boundaries, identity, and compute—using clear, readable infrastructure as code.

This is designed to be easy to review in a technical screen and easy to explain verbally, without unnecessary complexity.

---

## Architecture Overview

**Components**
- **VPC** with a single CIDR block and DNS support enabled
- **One public subnet** in a specified Availability Zone
- **Internet Gateway and public route table** to allow inbound and outbound internet access
- **Security group** permitting:
  - SSH (port 22) for administrative access
  - HTTP (port 80) for basic application testing
- **EC2 instance** running Amazon Linux in the public subnet
- **IAM role attached to the EC2 instance** using an instance profile
- **S3 bucket** for basic object storage

**Conceptual model**
- The VPC, subnet, route table, and internet gateway define the network boundary and traffic flow.
- The security group acts as the instance-level firewall.
- The IAM role provides AWS permissions to the instance without using static credentials.
- S3 is a regional managed service and is accessed by the instance through IAM permissions, not by being “inside” the VPC.

---

## Design Intent

This environment is deliberately minimal. It focuses on correctness and clarity rather than completeness.

The goal is to show:
- Clear understanding of AWS networking fundamentals
- Proper use of IAM roles instead of embedded credentials
- Clean, readable Terraform that can be reasoned about quickly
- Awareness of security tradeoffs and next-step improvements

If this were extended toward production, the next changes would likely include:
- Restricting SSH access to a known IP range or removing it entirely
- Moving compute into private subnets behind a load balancer
- Adding remote Terraform state with locking (S3 + DynamoDB)
- Introducing VPC endpoints (for example, S3) to reduce public internet exposure

---

## Repository Structure

```
.
├── main.tf
├── variables.tf
└── outputs.tf
```

---

## Prerequisites

- Terraform version 1.5 or newer
- An AWS account with credentials configured locally
- An existing EC2 key pair in the target region

---

## Configuration

Create a `terraform.tfvars` file (do not commit this file):

```hcl
ami_id         = "ami-xxxxxxxxxxxxxxxxx"
key_name       = "your-keypair-name"
s3_bucket_name = "globally-unique-bucket-name"
```

Notes:
- The S3 bucket name must be globally unique.
- The AMI ID should correspond to an Amazon Linux image available in the selected region.

---

## Deploy

```bash
terraform init
terraform plan
terraform apply
```

After apply completes, Terraform will output the public IP address of the EC2 instance.

### Verification

- Open `http://<EC2_PUBLIC_IP>` in a browser to confirm the instance is reachable.
- Verify the S3 bucket exists in the AWS console.

---

## Tear Down

```bash
terraform destroy
```

---

## What This Demonstrates

- Core AWS networking concepts (VPC, subnetting, routing)
- Instance-level security using security groups
- Identity and access management using IAM roles and instance profiles
- Basic compute and storage integration using EC2 and S3
- Infrastructure as Code practices with Terraform

---
