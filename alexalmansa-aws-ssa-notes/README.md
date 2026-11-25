# ☁️ AWS Solutions Architect Associate — Study Notes

## 📖 About

These notes were written while studying for the AWS SAA-C03 certification. They are structured to mirror the official exam domains and are optimized for **quick review and active recall** — not passive reading.

**What's inside:**
- ~50 topic files covering every exam domain
- A [CheatSheet](notes/CheatSheet.md) with key numbers, comparisons, and decision tables
- A [master index](notes/AWS%20Solutions%20Architect%20Index.md) linking every subtopic

> **Best viewed in [Obsidian](https://obsidian.md/)** — notes use wiki-links (`[[Service]]`) that render as clickable navigation inside Obsidian. They also work as plain Markdown in any viewer.

---

## 🗂️ Notes Index

### I. Core Infrastructure

| Topic | File |
|-------|------|
| EC2 (Instance Types, Launch Types, Placement Groups, ENI, Hibernate) | [EC2.md](notes/EC2.md) |
| Auto Scaling Groups (ASG) | [ASG.md](notes/ASG.md) |
| Elastic Load Balancing (ALB, NLB, CLB) | [ELB.md](notes/ELB.md) |
| VPC (Subnets, Route Tables, Peering, Endpoints, Flow Logs) | [VPC.md](notes/VPC.md) |
| Creating a VPC (Step-by-step) | [Creating a VPC.md](notes/Creating%20a%20VPC.md) |
| Networking Basics (IPs, CIDR, Subnets) | [Networking.md](notes/Networking.md) |
| Security Groups | [Security Groups.md](notes/Security%20Groups.md) |
| AWS Regions & Availability Zones | [AWS Regions & AZ.md](notes/AWS%20Regions%20%26%20AZ.md) |
| VPC Architecture Patterns | [Architecutres.md](notes/Architecutres.md) |
| VPC & Networking Exam Notes | [AWS VPC & Networking Exam Notes.md](notes/AWS%20VPC%20%26%20Networking%20Exam%20Notes.md) |

### II. Storage

| Topic | File |
|-------|------|
| S3 (Encryption, Security, Storage Classes, Lifecycle, Performance) | [S3.md](notes/S3.md) |
| EBS & EFS (Volume Types, Snapshots, Encryption, RAID) | [EBS & EFS.md](notes/EBS%20%26%20EFS.md) |
| FSx (Windows, Lustre, ONTAP, OpenZFS) | [FSx.md](notes/FSx.md) |
| Storage Gateway & Snowball Family | [Storage Extras.md](notes/Storage%20Extras.md) |
| DataSync | [DataSync.md](notes/DataSync.md) |
| Data Transfer & Migration Options | [Data Transfer & Migration Options.md](notes/Data%20Transfer%20%26%20Migration%20Options.md) |

### III. Databases

| Topic | File |
|-------|------|
| AWS Database Decision Guide | [Databases.md](notes/Databases.md) |
| RDS (Multi-AZ, Read Replicas, Backups) | [RDS.md](notes/RDS.md) |
| Aurora (Cluster, Serverless, Global) | [Aurora.md](notes/Aurora.md) |
| DynamoDB (DAX, Streams, Global Tables) | [DynamoDb.md](notes/DynamoDb.md) |
| ElastiCache (Redis vs Memcached) | [ElastiCache.md](notes/ElastiCache.md) |
| Redshift (OLAP, Spectrum) | [Redshift.md](notes/Redshift.md) |
| RDS vs DynamoDB Comparison | [RDS VS DynamoDB.md](notes/RDS%20VS%20DynamoDB.md) |

### IV. Serverless & Application Integration

| Topic | File |
|-------|------|
| Lambda (Event-driven, Lambda@Edge) | [Lambda.md](notes/Lambda.md) |
| API Gateway (REST, HTTP, WebSocket) | [Api Gateway.md](notes/Api%20Gateway.md) |
| Cognito (User Pools, Identity Pools) | [Cognito.md](notes/Cognito.md) |
| Kinesis (Streams, Firehose, Analytics) | [Kinesis.md](notes/Kinesis.md) |
| SQS (Queues, FIFO, Amazon MQ) | [SQS.md](notes/SQS.md) |
| SNS (Pub/Sub, Fan-out pattern) | [SNS.md](notes/SNS.md) |
| SQS & SNS Summary | [SQS & SNS Summary.md](notes/SQS%20%26%20SNS%20Summary.md) |
| Step Functions & SWF | [SWF & Step Functions.md](notes/SWF%20%26%20Step%20Functions.md) |

### V. Security, Identity & Compliance

| Topic | File |
|-------|------|
| IAM (Users, Roles, STS, Organizations, AD) | [IAM.md](notes/IAM.md) |
| KMS (CMK, Key Policies, CloudHSM) | [KMS.md](notes/KMS.md) |
| ACM (Certificate Manager) | [ACM.md](notes/ACM.md) |
| Shield (DDoS) & WAF | [Shield & WAF.md](notes/Shield%20%26%20WAF.md) |
| CloudTrail & AWS Config | [Cloudtrail.md](notes/Cloudtrail.md) |
| SSM Parameter Store & Secrets Manager | [SSM & Secrets Manager.md](notes/SSM%20%26%20Secrets%20Manager.md) |

### VI. Management & Governance

| Topic | File |
|-------|------|
| CloudWatch (Metrics, Logs, Alarms, Events) | [CloudWatch.md](notes/CloudWatch.md) |
| CloudFormation (IaC, StackSets, Drift) | [CloudFormation.md](notes/CloudFormation.md) |
| CLI & SDK | [CLI & SDK.md](notes/CLI%20%26%20SDK.md) |
| Other Services (CI/CD, Glue, EMR, Batch, AppSync, SAM…) | [Other Services.md](notes/Other%20Services.md) |

### VII. Deployment & Containers

| Topic | File |
|-------|------|
| Elastic Beanstalk (PaaS) | [Beanstalk.md](notes/Beanstalk.md) |
| ECS, Fargate & EKS | [ECS.md](notes/ECS.md) |

### VIII. Analytics & Specialized

| Topic | File |
|-------|------|
| Athena (Serverless SQL on S3) | [Athena.md](notes/Athena.md) |
| High Performance Computing (HPC, EFA) | [HPC.md](notes/HPC.md) |

### IX. Foundational Concepts

| Topic | File |
|-------|------|
| AWS Well-Architected Framework (5 Pillars) | [Five Pillars.md](notes/Five%20Pillars.md) |
| Disaster Recovery (RPO, RTO, Strategies) | [Disaster recovery.md](notes/Disaster%20recovery.md) |
| Route 53 (DNS, Record Types, Health Checks) | [Route53.md](notes/Route53.md) |
| Route 53 Routing Policies | [Routings.md](notes/Routings.md) |
| CloudFront & Global Accelerator | [CloudFront & Global Accelerator.md](notes/CloudFront%20%26%20Global%20Accelerator.md) |

---

## ⚡ Quick Access

| Resource | Description |
|----------|-------------|
| 📋 [CheatSheet](notes/CheatSheet.md) | Key numbers, comparison tables, and exam traps — start here for last-minute review |
| 🗺️ [Master Index](notes/AWS%20Solutions%20Architect%20Index.md) | Full topic tree linking every subtopic across all notes |


*Made while grinding for the AWS SAA-C03. Good luck on your exam!* ☁️
