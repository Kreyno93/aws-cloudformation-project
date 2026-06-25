# AWS CloudFormation — VPC + Auto Scaling Group

> CloudFormation template deploying an **EC2 Auto Scaling Group with Launch Template** — automatically scales EC2 instances based on demand.

[![AWS CloudFormation](https://img.shields.io/badge/AWS-CloudFormation-orange?logo=amazonaws)](https://aws.amazon.com/cloudformation/)
[![Auto Scaling](https://img.shields.io/badge/AWS-Auto%20Scaling-orange?logo=amazonaws)](https://aws.amazon.com/autoscaling/)
[![IaC](https://img.shields.io/badge/IaC-Infrastructure%20as%20Code-blue)](https://aws.amazon.com/cloudformation/)

## What this deploys

| Resource | Details |
|---|---|
| VPC | `172.16.0.0/24`, DNS enabled |
| Public Subnets | 2× across separate AZs |
| Internet Gateway | With route table association |
| Security Group | Inbound HTTP (80) + SSH (22) |
| Launch Template | `t3.small`, Amazon Linux 2023 |
| Auto Scaling Group | Spans both AZs, manages EC2 lifecycle |

## Architecture

```
Internet
    │
    ▼
Internet Gateway
    │
    ▼
VPC (172.16.0.0/24)
├── Public Subnet 1 (AZ-1) ─┐
└── Public Subnet 2 (AZ-2) ─┴── Auto Scaling Group
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
| `feature/auto-scaling` | **Auto Scaling Group + Launch Template** ← you are here |
| `feature/auto-scaling-with-alb` | Auto Scaling + ALB (full HA setup) |

## Keywords

`AWS CloudFormation` · `Auto Scaling Group` · `ASG` · `Launch Template` · `EC2 auto scaling` · `Infrastructure as Code` · `IaC` · `AWS VPC` · `CloudFormation template` · `AWS elasticity` · `scale EC2` · `DevOps` · `cloud automation` · `AWS re/Start` · `neuefische` · `CloudFormation YAML`
