# AWS CloudFormation — VPC + ALB + EC2

> CloudFormation template deploying a **highly available Application Load Balancer** distributing traffic across two EC2 web servers in separate Availability Zones.

[![AWS CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?logo=amazonaws)](https://aws.amazon.com/cloudformation/)
[![ALB](https://img.shields.io/badge/AWS-ALB-orange?logo=amazonaws)](https://aws.amazon.com/elasticloadbalancing/)
[![IaC](https://img.shields.io/badge/IaC-Infrastructure%20as%20Code-blue)](https://aws.amazon.com/cloudformation/)

## What this deploys

| Resource | Details |
|---|---|
| VPC | `172.16.0.0/24`, DNS enabled |
| Public Subnets | 2× across separate AZs |
| Internet Gateway | With route table association |
| Security Group | Inbound HTTP (80) + SSH (22) |
| EC2 Instances | 2× `t3.small`, Amazon Linux 2023 |
| Application Load Balancer | Internet-facing, HTTP listener on port 80 |
| Target Group | Both EC2 instances registered |

## Architecture

```
Internet
    │
    ▼
Application Load Balancer (HTTP:80)
    │
    ├──────────────────┐
    ▼                  ▼
EC2 (AZ-1)        EC2 (AZ-2)
Public Subnet 1   Public Subnet 2
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
| `feature/load-balancing` | **ALB across 2 EC2 instances** ← you are here |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Auto Scaling + ALB (full HA setup) |

## Keywords

`AWS CloudFormation` · `Application Load Balancer` · `ALB` · `EC2` · `load balancing AWS` · `multi-AZ` · `high availability` · `Infrastructure as Code` · `IaC` · `AWS VPC` · `CloudFormation template` · `AWS networking` · `DevOps` · `cloud automation` · `AWS re/Start` · `neuefische` · `CloudFormation YAML`
