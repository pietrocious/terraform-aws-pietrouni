# pietrouni.com

[![CI/CD](https://github.com/pietrocious/terraform-aws-pietrouni/actions/workflows/production.yml/badge.svg)](https://github.com/pietrocious/terraform-aws-pietrouni/actions/workflows/production.yml)
[![](https://img.shields.io/website?down_color=red&down_message=offline&label=pietrouni.com&up_color=green&up_message=online&url=https%3A%2F%2Fpietrouni.com)](https://pietrouni.com)


# AWS infrastructure for a personal portfolio site

[pietrouni.com](https://pietrouni.com)

This repository manages the AWS infrastructure for `pietrouni.com` using Terraform.

The current stack is intentionally small and focused:
- S3 for static site storage
- CloudFront for CDN delivery and HTTPS
- Route53 for DNS
- ACM for certificate management

Built using [Terraform](https://github.com/hashicorp/terraform) and [AWS](https://github.com/aws).

---

## project overview

This project is a learning-focused Terraform repository for a production static website on AWS.

The goal is to keep the infrastructure small enough to understand end to end, while still applying good IaC practices around DNS, certificates, content delivery, state management, and deployment safety.

## architecture

The current Terraform config provisions:
- an S3 bucket for site content
- a CloudFront distribution in front of the bucket
- an origin access control (OAC) so CloudFront can read from S3
- an ACM certificate for `pietrouni.com` and `www.pietrouni.com`
- a Route53 hosted zone and alias records for the domain

### tech stack

**infrastructure**
- aws: s3, cloudfront, route53, acm
- terraform: infrastructure as code
- github actions: terraform validation workflow

**devops practices**
- infrastructure as code (iac) for reproducibility
- versioned infrastructure changes through git
- secure static-site delivery through cloudfront and oac
- iterative improvement of state management, validation, and deployment workflows

### project structure
```
.
├── README.md
├── .gitignore
├── .github/
│   └── workflows/
│       └── production.yml
└── infrastructure/
    ├── main.tf
    ├── s3.tf
    ├── cloudfront.tf
    ├── acm.tf
    ├── route53.tf
    └── outputs.tf
```

Terraform files live under `infrastructure/`:
- `main.tf`: Terraform and provider configuration
- `s3.tf`: S3 bucket, public access block, and bucket policy
- `cloudfront.tf`: CloudFront distribution and origin access control
- `acm.tf`: ACM certificate and DNS validation records
- `route53.tf`: Route53 hosted zone and DNS alias records
- `outputs.tf`: deployment outputs such as bucket name, distribution ID, and nameservers

---

## deployment

### prerequisites
- aws account with access to s3, cloudfront, route53, and acm
- terraform
- a registered domain for `pietrouni.com`

### deploy infrastructure
```bash
terraform init
terraform plan
terraform apply
```

After the first apply, update your registrar with the Route53 nameservers from Terraform output.

---

## security features

- **encryption in transit:** https enforced through cloudfront and acm
- **restricted origin access:** the s3 bucket is intended to be reachable through cloudfront, not directly
- **infrastructure as code:** all infrastructure changes are tracked in terraform configuration

---

## roadmap

### phase 1

- [ ] add remote state management with an s3 backend and dynamodb locking
- [ ] commit the Terraform lock file and tighten Terraform/provider version constraints
- [ ] add missing DNS `AAAA` records for full IPv6 support
- [ ] add CloudFront SPA fallback behavior if the site requires client-side routing
- [ ] tighten S3 public access settings around the CloudFront OAC pattern
- [ ] improve CI to run `terraform fmt`, `terraform init -backend=false`, `terraform validate`, and lint/security checks
- [ ] keep the documentation aligned with what is actually deployed

### phase 2

- [ ] add multi-environment support only if the project actually needs separate `dev`, `staging`, and `prod` stacks
- [ ] add CloudFront response headers and other lightweight security hardening
- [ ] improve cost visibility with basic tagging, usage review, and cost-focused cleanup
- [ ] extract the shared static-site pattern into a reusable module if the three site repos continue to evolve in parallel

---

## special thanks

- adrian cantrill's [SAA course](https://learn.cantrill.io/p/aws-certified-solutions-architect-associate-saa-c03)
- logan marchione's [best devops project for a beginner](https://loganmarchione.com/2022/10/the-best-devops-project-for-a-beginner/)
- [r/devops](https://www.reddit.com/r/devops/?rdt=63211)

## contact

- [in/pietrouni](https://linkedin.com/in/pietrouni)
- [pietrouni@gmail.com](mailto:pietrouni@gmail.com)
