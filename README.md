# AWS CloudFormation — Full HA: Auto Scaling + ALB

> CloudFormation template for a **production-ready, highly available AWS setup**: Application Load Balancer + Auto Scaling Group across multiple Availability Zones.

[![AWS CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?logo=amazonaws)](https://aws.amazon.com/cloudformation/)
[![Auto Scaling](https://img.shields.io/badge/AWS-Auto%20Scaling-orange?logo=amazonaws)](https://aws.amazon.com/autoscaling/)
[![ALB](https://img.shields.io/badge/AWS-ALB-orange?logo=amazonaws)](https://aws.amazon.com/elasticloadbalancing/)
[![IaC](https://img.shields.io/badge/IaC-Infrastructure%20as%20Code-blue)](https://aws.amazon.com/cloudformation/)

## What this deploys

| Resource | Details |
|---|---|
| VPC | `172.16.0.0/24`, DNS enabled |
| Public Subnets | 2× across separate AZs |
| Internet Gateway | With route table association |
| Security Groups | ALB SG + EC2 SG (scoped to ALB) |
| Launch Template | `t3.small`, Amazon Linux 2023 |
| Auto Scaling Group | Multi-AZ, registers instances to ALB |
| Application Load Balancer | Internet-facing, HTTP listener on port 80 |
| Target Group | Health-checked, ALB routes to ASG instances |

## Architecture

```
Internet
    │
    ▼
Application Load Balancer (HTTP:80)
    │
    ├──────────────────────┐
    ▼                      ▼
Public Subnet 1 (AZ-1)  Public Subnet 2 (AZ-2)
    │                      │
    └──────── Auto Scaling Group ──────────
                  └── EC2 Instances (Launch Template)
```

## Deploy

```bash
aws cloudformation deploy \
  --stack-name CF-Project \
  --template-file CF-Project.yaml \
  --region us-west-2
```

## Tear down

```bash
aws cloudformation delete-stack --stack-name CF-Project --region us-west-2
```

## Branches

| Branch | Feature |
|---|---|
| `main` | VPC + EC2 + Security Group baseline |
| `feature_instance` | EC2 instance without security group |
| `feature/load-balancing` | ALB across 2 EC2 instances |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | **Auto Scaling + ALB (full HA setup)** ← you are here |

## Keywords

`AWS CloudFormation` · `Auto Scaling Group` · `ASG` · `Application Load Balancer` · `ALB` · `high availability` · `multi-AZ` · `Launch Template` · `EC2 auto scaling` · `Infrastructure as Code` · `IaC` · `fault tolerant AWS` · `production AWS architecture` · `CloudFormation template` · `DevOps` · `cloud automation` · `AWS re/Start` · `neuefische` · `CloudFormation YAML`
