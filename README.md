# AWS CloudFormation Web Server
## Deploy a Fully Networked EC2 Instance on AWS — Zero Console Clicks

This project provisions a production-ready AWS web server entirely through **CloudFormation Infrastructure as Code**. With a single CLI command, it builds a VPC, two public subnets across availability zones, an internet gateway, a security group, and a running EC2 instance on Amazon Linux 2023. No manual AWS Console setup required — everything is declarative, repeatable, and version-controlled.

This is the baseline branch of a progressive learning project built during the [neuefische](https://www.neuefische.de/) AWS re/Start programme, demonstrating IaC fundamentals from a single EC2 instance all the way to a fully highly available auto-scaled architecture.

---

```
Internet
    │
    ▼
Internet Gateway
    │
    ▼
VPC (172.16.0.0/24)
├── Public Subnet 1 (AZ-a) ── EC2 Web Server (t3.small)
└── Public Subnet 2 (AZ-b)   Security Group: HTTP 80, SSH 22
```

---

## Usage

**Prerequisites:** AWS CLI configured, and a key pair named `vockey` in your target region.

```bash
# Deploy
aws cloudformation deploy \
  --stack-name CF-Project \
  --template-file CF-Project.yaml \
  --region us-west-2

# Tear down
aws cloudformation delete-stack --stack-name CF-Project --region us-west-2
```

---

## Contributing

Clone the repo and check out the branch you want to work on:

```bash
git clone https://github.com/Kreyno93/aws-cloudformation-project.git
cd aws-cloudformation-project
git checkout main
```

Validate your changes before pushing:

```bash
aws cloudformation validate-template --template-body file://CF-Project.yaml
```

---

## Contributor Expectations

- Open an issue before starting work on a new feature or fix.
- One logical change per pull request — keep diffs small and focused.
- PR title format: `feat:`, `fix:`, or `docs:` prefix (e.g. `feat: add outputs section`).
- All templates must pass `aws cloudformation validate-template` before submitting a PR.
- Do not commit real AWS account IDs, key material, or credentials.

---

## Other Branches

| Branch | What it adds |
|---|---|
| `main` | VPC + EC2 baseline ← you are here |
| `feature_instance` | EC2 without security group (learning exercise) |
| `feature/load-balancing` | Application Load Balancer across 2 EC2 instances |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Full HA: Auto Scaling + ALB |

---

## Known Issues

- The `vockey` key pair must exist in your AWS region before deploying — it is not created by the template.
- `t3.small` is free-tier eligible.
- Security group allows SSH from `0.0.0.0/0` — restrict to your IP in production.

---

`AWS CloudFormation` · `Infrastructure as Code` · `IaC` · `EC2` · `VPC` · `Amazon Linux 2023` · `AWS networking` · `CloudFormation YAML` · `DevOps` · `AWS re/Start` · `neuefische` · `cloud automation` · `AWS beginner project` · `deploy EC2 with CloudFormation`
