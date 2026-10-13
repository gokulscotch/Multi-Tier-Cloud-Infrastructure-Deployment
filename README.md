# Multi-Tier-Cloud-Infrastructure-Deployment

# Multi-Tier Cloud Infrastructure Deployment

A personal project that deploys a **3-tier architecture on AWS** to simulate a production-grade environment, with secure networking, least-privilege access, cost optimization, and monitoring built in.

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Status](https://img.shields.io/badge/status-completed-brightgreen)
![Type](https://img.shields.io/badge/type-personal%20project-blue)

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [AWS Services Used](#aws-services-used)
- [Key Features](#key-features)
- [Network Design](#network-design)
- [Security](#security)
- [Cost Optimization](#cost-optimization)
- [Monitoring](#monitoring)
- [Deployment Steps](#deployment-steps)
- [Screenshots](#screenshots)
- [Challenges and Learnings](#challenges-and-learnings)
- [Future Improvements](#future-improvements)
- [Author](#author)

---

## Overview

This project demonstrates how to design and deploy a multi-tier application infrastructure on AWS. The environment separates concerns into three tiers so that each layer can be secured, scaled, and monitored independently:

| Tier | Purpose | AWS Service |
|------|---------|-------------|
| Presentation / Web | Serves user requests | Amazon EC2 (public subnet) |
| Application / Storage | Static assets, logs, backups | Amazon S3 |
| Database | Persistent relational data | Amazon RDS (private subnet) |

---

## Architecture

```
                         Internet
                             |
                    [ Internet Gateway ]
                             |
        +--------------------+--------------------+
        |                 Custom VPC              |
        |                                         |
        |   +---------------------------------+   |
        |   |  Public Subnet                  |   |
        |   |  EC2 (Web / App Server)         |-------> Amazon S3
        |   |  Security Group: HTTP/HTTPS/SSH |   |    (lifecycle policies)
        |   +----------------+----------------+   |
        |                    |                    |
        |   +----------------v----------------+   |
        |   |  Private Subnet                 |   |
        |   |  Amazon RDS (Database)          |   |
        |   |  Security Group: DB port only   |   |
        |   |  from EC2 security group        |   |
        |   +---------------------------------+   |
        |                                         |
        +--------------------+--------------------+
                             |
                  [ Amazon CloudWatch ]
                  Dashboards and Alarms
```

> Replace this diagram with your own architecture image (for example, `docs/architecture.png`) and reference it here:
> `![Architecture Diagram](docs/architecture.png)`

---

## AWS Services Used

- **Amazon EC2**: compute layer hosting the web/application server
- **Amazon S3**: object storage for static content, backups, and logs
- **Amazon RDS**: managed relational database in a private subnet
- **Amazon VPC**: custom network with public and private subnets
- **AWS IAM**: least-privilege roles and policies
- **Amazon CloudWatch**: dashboards, metrics, and alarms

---

## Key Features

- **3-tier architecture** simulating a real production environment
- **Custom VPC** with isolated public and private subnets
- **Security groups** that restrict traffic between tiers
- **Least-privilege IAM roles** for secure access management
- **S3 lifecycle policies** to reduce storage costs automatically
- **CloudWatch dashboards and alarms** for EC2 and RDS visibility

---

## Network Design

| Component | Configuration |
|-----------|---------------|
| VPC | Custom VPC with a defined CIDR block (e.g., `10.0.0.0/16`) |
| Public Subnet | Hosts the EC2 instance; routed through an Internet Gateway |
| Private Subnet | Hosts RDS; no direct internet access |
| Route Tables | Separate tables for public and private subnets |
| Security Groups | Tier-specific rules, with the database accepting traffic only from the app tier |

---

## Security

- The database sits in a **private subnet** and is not publicly accessible.
- The RDS security group allows inbound traffic **only from the EC2 security group** on the database port.
- EC2 uses an **IAM instance role** rather than hard-coded access keys.
- IAM policies follow the **principle of least privilege**, granting only the permissions each resource needs.
- SSH access is restricted to trusted IP addresses.

---

## Cost Optimization

S3 lifecycle policies automatically manage object storage costs:

- Transition infrequently accessed objects to lower-cost storage classes (e.g., S3 Standard-IA, Glacier)
- Expire old or temporary objects after a defined retention period
- Clean up incomplete multipart uploads

---

## Monitoring

Amazon CloudWatch provides observability for the environment:

- **EC2**: CPU utilization, network traffic, status checks
- **RDS**: CPU utilization, free storage space, database connections
- **Dashboards**: a single view of key EC2 and RDS metrics
- **Alarms**: threshold-based alerts (e.g., high CPU, low free storage)

---

## Deployment Steps

1. **Create the VPC**: define the CIDR block, public and private subnets, Internet Gateway, and route tables.
2. **Configure security groups**: one for the EC2 tier and one for the RDS tier with restricted rules.
3. **Create IAM roles**: attach least-privilege policies and assign the role to the EC2 instance.
4. **Launch the EC2 instance** in the public subnet and install the web/application stack.
5. **Provision RDS** in the private subnet group and connect it to the application.
6. **Create the S3 bucket** and attach lifecycle policies.
7. **Set up CloudWatch**: build dashboards and configure alarms for EC2 and RDS.
8. **Test**: verify connectivity, access restrictions, and alarm triggers.

---

## Screenshots

Add screenshots under a `docs/` or `screenshots/` folder and link them here:

- VPC and subnet configuration
- Security group rules
- IAM role and policy
- EC2 and RDS instances
- S3 lifecycle rules
- CloudWatch dashboard and alarms

---

## Challenges and Learnings

- Designing subnet and routing layouts that keep the database private yet reachable from the app tier
- Writing IAM policies that grant just enough access
- Balancing cost savings with data availability through lifecycle rules
- Choosing meaningful CloudWatch metrics and alarm thresholds

---

## Future Improvements

- Add an **Application Load Balancer** and **Auto Scaling Group** for high availability
- Enable **RDS Multi-AZ** for failover
- Add a **NAT Gateway** for outbound access from private subnets
- Automate provisioning with **Terraform** or **AWS CloudFormation**
- Add **AWS CloudTrail** and **AWS Config** for auditing and compliance

---

## Author

**Gokulan A**
Aspiring Cloud Engineer | B.Sc Information Technology, Madurai Kamaraj University
AWS Certified Solutions Architect (Coursera)

[LinkedIn](#) | [GitHub](#)
