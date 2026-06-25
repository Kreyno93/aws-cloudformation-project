# AWS CloudFormation — High Availability Web Infrastructure
## Auto Scaling Group + Application Load Balancer Across Multiple Availability Zones

This is the **capstone branch** of the project: a complete, production-grade AWS web infrastructure deployed with a single CloudFormation template. An Application Load Balancer sits in front of an Auto Scaling Group, spreading HTTP traffic across EC2 instances in two Availability Zones. If an instance fails a health check, the ASG replaces it automatically. The ALB DNS name is emitted as a stack output — your app is reachable the moment the stack is deployed.

Built during the [neuefische](https://www.neuefische.de/) AWS re/Start programme, this branch ties together every concept from the previous branches: networking, security groups, load balancing, and auto scaling — all declarative, all version-controlled, all repeatable.

---

```
Internet
    │
    ▼
Application Load Balancer  (HTTP :80)
CF-ALB — internet-facing
    │
    ├──────────────────────────┐
    ▼                          ▼
Public Subnet 1 (AZ-a)    Public Subnet 2 (AZ-b)
    │                          │
    └──────── Auto Scaling Group (min:1 desired:2 max:3) ────────┘
                    └── EC2 Instances via Launch Template
                          t3.small · Amazon Linux 2023
```

---

## Usage

**Prerequisites:** AWS CLI configured, `vockey` key pair in your target region.

```bash
# Deploy
aws cloudformation deploy \
  --stack-name CF-Project \
  --template-file CF-Project.yaml \
  --region us-west-2

# Get the ALB DNS name after deploy
aws cloudformation describe-stacks \
  --stack-name CF-Project \
  --region us-west-2 \
  --query "Stacks[0].Outputs"

# Tear down
aws cloudformation delete-stack --stack-name CF-Project --region us-west-2
```

---

## Contributing

```bash
git clone https://github.com/Kreyno93/aws-cloudformation-project.git
cd aws-cloudformation-project
git checkout feature/auto-scaling-with-alb
```

Validate before pushing:

```bash
aws cloudformation validate-template --template-body file://CF-Project.yaml
```

---

## Contributor Expectations

- Open an issue before starting work on a new feature or fix.
- One logical change per pull request.
- PR title format: `feat:`, `fix:`, or `docs:` prefix.
- All templates must pass `aws cloudformation validate-template` before submitting a PR.
- Do not commit real AWS account IDs, key material, or credentials.

---

## Other Branches

| Branch | What it adds |
|---|---|
| `main` | VPC + EC2 + Security Group baseline |
| `feature_instance` | EC2 without security group (learning exercise) |
| `feature/load-balancing` | Application Load Balancer across 2 EC2 instances |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Full HA: Auto Scaling + ALB ← you are here |

---

## Known Issues

- The `vockey` key pair must exist in your AWS region before deploying.
- `t3.small` is free-tier eligible. With `DesiredCapacity: 2`, two instances run by default.
- The EC2 security group allows SSH from `0.0.0.0/0` — restrict to your IP in production.
- There is no HTTPS listener — traffic runs over HTTP only. Add an ACM certificate and an HTTPS listener for production use.

---

`AWS CloudFormation` · `Auto Scaling Group` · `ASG` · `Application Load Balancer` · `ALB` · `high availability AWS` · `multi-AZ` · `EC2 Launch Template` · `fault tolerant architecture` · `Infrastructure as Code` · `IaC` · `CloudFormation YAML` · `production AWS setup` · `DevOps` · `AWS re/Start` · `neuefische` · `CloudFormation ALB ASG example`
