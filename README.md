# AWS CloudFormation VPC + EC2 — No Security Group
## Understanding EC2 Network Exposure Without a Security Group

This branch is a **learning exercise** that intentionally deploys an EC2 instance inside a VPC with no security group attached. It demonstrates what AWS infrastructure looks like at its most minimal — just the networking layer: a VPC, public subnets, an internet gateway, and an EC2 instance. The absence of a security group is deliberate, to make visible what a security group actually protects you from.

This is part of a progressive CloudFormation learning series built during the [neuefische](https://www.neuefische.de/) AWS re/Start programme. Start here to understand the baseline before adding security layers in the other branches.

---

```
Internet
    │
    ▼
Internet Gateway
    │
    ▼
VPC (172.16.0.0/24)
└── Public Subnet 1 (AZ-a) ── EC2 Instance (no Security Group)
```

---

## Usage

**Prerequisites:** AWS CLI configured with appropriate credentials.

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
git checkout feature_instance
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
| `feature_instance` | EC2 without security group ← you are here |
| `feature/load-balancing` | Application Load Balancer across 2 EC2 instances |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Full HA: Auto Scaling + ALB |

---

## Known Issues

- The `ImageId` field in this template requires a valid AMI ID for your region — it is not auto-resolved via SSM in this branch.
- No security group means no inbound access is possible by default — the instance cannot be reached via SSH or HTTP until a security group is added.
- `t2.micro` is used here; both t2.micro and t3.small are free-tier eligible.

---

`AWS CloudFormation` · `EC2 no security group` · `VPC CloudFormation` · `Infrastructure as Code` · `IaC` · `AWS networking` · `CloudFormation beginner` · `AWS re/Start` · `neuefische` · `EC2 without security group` · `AWS learning project`
