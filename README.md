# Terraform Infrastructure for Amazon EKS

This repository provisions a complete AWS infrastructure for deploying applications on **Amazon EKS** using **Terraform**.

The infrastructure follows a modular approach where each directory represents an independent infrastructure component. Modules are deployed sequentially to build a production-ready Kubernetes platform.

---

## Architecture

```
                    GitHub
                       │
                       ▼
                Terraform Apply
                       │
                       ▼
                 AWS Infrastructure
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
     VPC         Security Groups      ECR
      │                │                │
      └────────────┬───┘                │
                   ▼                    │
              Bastion Host              │
                   │                    │
                   ▼                    ▼
              Amazon EKS  <──────── Docker Images
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
   Storage     IAM Roles    Applications
        │
        ▼
   AWS Load Balancer
        │
        ▼
     Internet
```

---

# Repository Structure

```
terraform-infra-eks/
│
├── 00-vpc/
├── 10-sg/
├── 20-sg-rules/
├── 30-bastion/
├── 31-cicd-tools/
├── 40-rds/
├── 50-ecr/
├── 60-eks/
├── 61-eks-iam/
├── 70-acm/
├── 80-frontend-alb/
│
└── terraform.tfstate
```

---

# Infrastructure Modules

## 00-vpc

Creates the networking foundation.

Resources:

- VPC
- Public Subnets
- Private Subnets
- Database Subnets
- Internet Gateway
- NAT Gateway
- Route Tables

---

## 10-sg

Creates Security Groups for every component.

Examples:

- Bastion
- EKS Control Plane
- Worker Nodes
- ALB
- RDS

---

## 20-sg-rules

Adds ingress and egress rules between Security Groups.

Configuration is managed using:

```
sg_rules.yaml
```

Making security rules reusable and easy to manage.

---

## 30-bastion

Creates a Bastion Host for secure administration.

Includes:

- EC2 Instance
- Bootstrap Script
- SSH Configuration

---

## 31-cicd-tools

Deploys CI/CD infrastructure.

Includes:

- Jenkins Server
- GitHub Actions Runner
- Route53 Records
- Bootstrap Scripts
- Runner Service

---

## 40-rds

Creates Amazon RDS database.

Used by Roboshop microservices.

---

## 50-ecr

Creates Amazon Elastic Container Registry repositories.

Stores Docker images built from CI pipelines.

---

## 60-eks

Creates the Kubernetes platform.

Includes:

- Amazon EKS Cluster
- Managed Node Groups
- Kubernetes Provider
- Storage Classes
- Namespaces
- Backend Applications
- Frontend Applications
- Gateway
- Target Group Binding

Application manifests are stored under:

```
app/
```

---

## 61-eks-iam

Creates IAM Roles and Policies for Kubernetes workloads.

Supports IAM Roles for Service Accounts (IRSA).

---

## 70-acm

Creates AWS Certificate Manager certificates.

Used for HTTPS.

---

## 80-frontend-alb

Creates the Application Load Balancer.

Features:

- HTTPS Listener
- Target Groups
- Listener Rules
- ACM Integration

---

# Deployment Order

Modules should be applied in the following order:

```
00-vpc

↓

10-sg

↓

20-sg-rules

↓

30-bastion

↓

31-cicd-tools

↓

40-rds

↓

50-ecr

↓

60-eks

↓

61-eks-iam

↓

70-acm

↓

80-frontend-alb
```

---

# Technologies Used

- Terraform
- Amazon VPC
- Amazon EKS
- Amazon RDS
- Amazon ECR
- AWS ACM
- Route53
- EC2
- IAM
- Application Load Balancer
- Kubernetes
- GitHub Actions
- Jenkins

---

# Terraform Commands

Initialize

```bash
terraform init
```

Validate

```bash
terraform validate
```

Plan

```bash
terraform plan
```

Deploy

```bash
terraform apply
```

Destroy

```bash
terraform destroy
```

---

# Features

- Modular Terraform Design
- Production-ready Infrastructure
- Kubernetes Platform on Amazon EKS
- CI/CD Integration
- Automated Networking
- Secure Security Group Management
- HTTPS with ACM
- Load Balancer Automation
- Bastion Host
- Amazon RDS
- Amazon ECR
- IAM Roles for Service Accounts (IRSA)

---

# Infrastructure Workflow

```
Developer
      │
      ▼
GitHub Repository
      │
      ▼
Terraform
      │
      ▼
AWS Infrastructure
      │
      ▼
Amazon EKS
      │
      ▼
Kubernetes Applications
```

---

# Prerequisites

- AWS Account
- Terraform
- AWS CLI
- kubectl
- Helm
- Git

---

# Notes

- Each directory is an independent Terraform module.
- Modules are designed to be executed sequentially.
- Configuration is parameterized using variables and local values.
- IAM roles are configured separately to support Kubernetes service accounts.
- Kubernetes application manifests are maintained inside the `60-eks/app` directory.

