# AWS CloudFormation — VPC + EC2 Instance

> Minimal AWS CloudFormation template: provisions a **VPC with public networking and an EC2 instance** — no security group.

[![AWS CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?logo=amazonaws)](https://aws.amazon.com/cloudformation/)
[![IaC](https://img.shields.io/badge/IaC-Infrastructure%20as%20Code-blue)](https://aws.amazon.com/cloudformation/)

## What this deploys

| Resource | Details |
|---|---|
| VPC | `172.16.0.0/24`, DNS enabled |
| Public Subnets | 2× across separate AZs |
| Internet Gateway | With route table association |
| EC2 Instance | `t3.small`, Amazon Linux 2023, public IP |

## Architecture

```
Internet
    │
    ▼
Internet Gateway
    │
    ▼
VPC (172.16.0.0/24)
└── Public Subnet 1 (AZ-1) ── EC2 Instance
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
| `feature_instance` | **EC2 instance without security group** ← you are here |
| `feature/load-balancing` | ALB across 2 EC2 instances |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Auto Scaling + ALB (full HA setup) |

## Keywords

`AWS CloudFormation` · `Infrastructure as Code` · `IaC` · `AWS VPC` · `EC2` · `Amazon Linux 2023` · `CloudFormation template` · `AWS networking` · `DevOps` · `cloud automation` · `AWS re/Start` · `neuefische` · `CloudFormation YAML` · `minimal AWS setup`
