# AWS CloudFormation Application Load Balancer
## Distribute Web Traffic Across Two EC2 Instances in Different Availability Zones

This branch adds an **Application Load Balancer (ALB)** to the baseline infrastructure, distributing incoming HTTP traffic across two EC2 web servers running in separate AWS Availability Zones. The ALB is internet-facing, health-checks both instances, and exposes a single DNS endpoint — no direct instance IPs needed. The stack outputs the ALB DNS name so you can hit it immediately after deploy.

This is part of a progressive CloudFormation learning series built during the [neuefische](https://www.neuefische.de/) AWS re/Start programme, showing how to move from a single EC2 instance to a load-balanced, multi-AZ setup — all in one CloudFormation template.

---

```
Internet
    │
    ▼
Application Load Balancer  (HTTP :80)
CF-ALB — internet-facing
    │
    ├─────────────────────┐
    ▼                     ▼
EC2 WebServer         EC2 WebServer
Public Subnet 1 (AZ-a) Public Subnet 2 (AZ-b)
t3.small · AL2023      t3.small · AL2023
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
git checkout feature/load-balancing
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
| `feature/load-balancing` | Application Load Balancer across 2 EC2 instances ← you are here |
| `feature/auto-scaling` | Auto Scaling Group + Launch Template |
| `feature/auto-scaling-with-alb` | Full HA: Auto Scaling + ALB |

---

## Known Issues

- The `vockey` key pair must exist in your AWS region before deploying.
- `t3.small` is free-tier eligible.
- The EC2 instance in Subnet 1 is registered directly to the target group — it is a static target, not managed by an Auto Scaling Group. See `feature/auto-scaling-with-alb` for the dynamic version.
- Security group allows SSH from `0.0.0.0/0` — restrict to your IP in production.

---

`AWS CloudFormation` · `Application Load Balancer` · `ALB` · `EC2 load balancing` · `multi-AZ` · `high availability AWS` · `Infrastructure as Code` · `IaC` · `AWS VPC` · `CloudFormation YAML` · `DevOps` · `AWS re/Start` · `neuefische` · `CloudFormation ALB example` · `AWS networking`
