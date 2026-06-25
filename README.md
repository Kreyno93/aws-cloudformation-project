# AWS CloudFormation Auto Scaling Group
## Automatically Scale EC2 Instances Up and Down with a Launch Template

This branch introduces **EC2 Auto Scaling** to the infrastructure. Instead of manually defined instances, a Launch Template defines the server configuration once, and an Auto Scaling Group uses it to spin instances up or down across two Availability Zones based on demand. The group is configured with a minimum of 1, a desired capacity of 2, and a maximum of 3 instances — AWS manages the rest.

This is part of a progressive CloudFormation learning series built during the [neuefische](https://www.neuefische.de/) AWS re/Start programme, showing how elasticity works in practice before combining it with a load balancer in the next branch.

---

```
Internet
    │
    ▼
Internet Gateway
    │
    ▼
VPC (172.16.0.0/24)
├── Public Subnet 1 (AZ-a) ─┐
└── Public Subnet 2 (AZ-b) ─┴─ Auto Scaling Group (min:1 desired:2 max:3)
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

# Tear down
aws cloudformation delete-stack --stack-name CF-Project --region us-west-2
```

---

## Contributing

```bash
git clone https://github.com/Kreyno93/aws-cloudformation-project.git
cd aws-cloudformation-project
git checkout feature/auto-scaling
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
| `feature/auto-scaling` | Auto Scaling Group + Launch Template ← you are here |
| `feature/auto-scaling-with-alb` | Full HA: Auto Scaling + ALB |

---

## Known Issues

- The `vockey` key pair must exist in your AWS region before deploying.
- `t3.small` is not free-tier eligible. With `DesiredCapacity: 2`, two instances run by default.
- There is no load balancer in this branch — instances are not reachable via a single endpoint. See `feature/auto-scaling-with-alb` for the full setup.
- Security group allows SSH from `0.0.0.0/0` — restrict to your IP in production.

---

`AWS CloudFormation` · `Auto Scaling Group` · `ASG` · `EC2 Launch Template` · `EC2 auto scaling` · `AWS elasticity` · `scale EC2` · `Infrastructure as Code` · `IaC` · `CloudFormation YAML` · `DevOps` · `AWS re/Start` · `neuefische` · `CloudFormation auto scaling example` · `multi-AZ EC2`
