## 🖥️ Compute
### [[EC2]]
- **Launch Types**: On-Demand, Reserved (1–3 yrs), Spot (90% off but interruptible), Dedicated (hosts/instances).
- **Placement Groups**:
  - Cluster → low latency, single AZ.
  - Spread → max 7 instances per AZ, HA.
  - Partition → racks isolated, big distributed systems.
- **Networking**: ENI = extra NIC, Elastic IP = static IPv4.
- **Metadata**: `http://169.254.169.254/latest/meta-data/`
- **Hibernate** = RAM preserved to EBS.
- **Spot Fleet** = mix of instance types/AZs with scaling policy.
- **Instance Store**: Ephemeral storage, lost on stop/terminate, faster than EBS.

### Autoscaling [[ASG]]
- Policies: Target Tracking, Step, Scheduled.
- Health checks: EC2 + ELB.

### Elastic Load Balancer [[ELB]] 
- **Types**: 
  - ALB (HTTP/HTTPS, WAF, path-based, host-based routing).  
  - NLB (TCP/UDP, static IP, millions of RPS).  
  - CLB (legacy).  
- **Features**: Sticky sessions, Cross-zone LB, SSL termination, Connection draining.

---

## 🌐 Networking
### VPC Basics [[VPC]]
- One VPC spans all AZs in a region.
- Max 5 CIDR blocks per VPC
- Subnets = AZ-scoped.
- Route tables decide where traffic goes.
- SGs = stateful, NACLs = stateless.

### Internet Connectivity [[AWS VPC & Networking Exam Notes]]
- **IGW** → 2-way internet (public subnets). [[VPC#VI. Internet Gateways (IGW aka Modem)]]
- **NAT Gateway** → outbound only for private subnets, managed, 45Gbps. Place it in a public subnet and private subnet traffic must be routed towards it)[[VPC#IX. NAT Gateways (Modern, AWS-Managed, Scalable)]]
- **NAT Instance** → legacy, uses Security Groups, manual scaling. You manage everything [[VPC#VII. NAT Instances (Legacy, DIY, but Flexible)]]
- **Egress-only IGW** → IPv6-only outbound. [[VPC#XVII. Egress only IGW]]

### VPC Add-Ons
- **VPC Peering**: Simple, no transitive routing. [[VPC#III. Peering (Do this after section XV)]]
- **Transit Gateway**: Hub-and-spoke for multi-VPC + on-prem, scalable. [[VPC#XXI. Transit Gateway]]
- **Bastion Hosts**: Jump box for SSH/RDP into private subnets.
- **VPC Flow Logs**: Capture metadata of accepted/rejected traffic (no packet data).

### VPC Endpoints [[VPC#XII. VPC Endpoints]]
- **Gateway** → S3, DynamoDB.
- **Interface (PrivateLink)** → most services, ENI in subnet.
- Keeps traffic off internet.

### DNS (Route 53 Resolver) [[Route53]]
- **Inbound Endpoint** → on-prem → AWS VPC DNS.
- **Outbound Endpoint** → AWS VPC → on-prem DNS.
- Routing policies: Simple, Weighted, Latency, Failover, Geo, Multi-Value.

### DX & VPN
- **DX VIFs**: Public (AWS services via public IPs), Private (VPC), Transit.
- **VPN CloudHub**: Connect multiple sites to a VGW.  [[VPC#XV. Virtual Private Gateway (aka AWS VPN)]]
- VPN over DX or internet = IPsec tunnels.

---

## 📦 Storage & Migration
### [[S3]]
- **Classes**: Standard, IA, 1Z-IA, Glacier, Glacier DA.
- Versioning + MFA delete.
- Lifecycle → transition/expire.
- Cross-region replication.
- **Consistency**: Strong for new PUTs; eventual for overwrite/delete.
- Max object size: **5 TB**
- Prefix scaling: **3,500 PUT/s**, **5,500 GET/s**
- Big Files: Multipart + S3TA

### EBS [[EBS & EFS]]
- gp3 (cheap), io1/io2 (high IOPS), st1/sc1 (throughput).
	- gp3: 3,000 IOPS, 125 MB/s baseline → **16,000 IOPS, 1,000 MB/s max**
	- io1/io2: Up to **64,000 IOPS**
	- Volume size: **1 GB – 16 TiB**
- Snapshots = incremental, stored in S3.
- **RAID**: RAID 0 (performance), RAID 1 (High Abailability).

### EFS [[EBS & EFS#EFS volumes]]
- Max I/O throughput: **1 GiB/s** (provisioned)
- NFS, multi-AZ, elastic scaling.
- Perf modes: General Purpose vs Max I/O.
- Only Linux

### [[FSx]]
- Windows (SMB), Lustre (HPC), ONTAP, OpenZFS.

### [[DataSync]]
- Online transfers: NFS/SMB/S3/EFS/FSx.
- 10Gbps per agent.
- Paths: Internet, DX (Private VIF), PrivateLink.
- Exam trap: EFS = needs PrivateLink + Private VIF.

### Storage Gateway [[Storage Extras]]
- **File Gateway** → Lets you store and access files in S3 using standard file protocols like NFS/SMB.
- **Volume Gateway** → Makes cloud-backed block storage (like virtual hard drives) available on-prem via iSCSI.
- **Tape Gateway** → Replaces physical tape backups with virtual tapes stored in S3 and Glacier, no software change needed.
- For hybrid live  access, not bulk transfer.

### Snow Family (Offline) [[Data Transfer & Migration Options]] 
- **Snowcone**: 8TB HDD / 14TB SSD, tiny + EC2/Lambda capable.
- **Snowball**: 50–80TB.
- **Snowball Edge**: 100TB + compute.
- **Snowmobile**: 100PB truck.

---

## 🛢️ Databases
### [[RDS]]
- Multi-AZ = HA, sync.
- Read Replicas = scale, async (can be cross-region).
- Backups: automated (daily), snapshots (manual).
- Max size: up to **64 TB** (Aurora), ~**32 TB** (others)

### [[Aurora]]
- 6 copies across 3 AZs.
- Endpoints: Cluster, Reader, Custom.
- Aurora Global = cross-region <1s lag.
- Aurora Serverless = auto-scale, can pause to $0.

### [[DynamoDb]]
- NoSQL, auto-scaling.
- Global Tables = multi-region.
- DAX = in-memory cache.
- Streams = event log ( can be fed to lambda) 24h retention default.

### [[Redshift]]
- Columnar OLAP DB.
- Spectrum = query S3 directly.
- RA3 = decoupled storage/compute.

### [[ElastiCache]]
- Redis = persistence + pub/sub.
- Memcached = simple, multi-threaded, no persistent.

### Other [[Databases]]
- DocumentDB (MongoDB-compatible).
- Keyspaces (Cassandra-compatible).
- Neptune (Graph DB).
- QLDB (Ledger, immutable).
- Timestream (Time series).

---

## 🔐 Security 
- **[[IAM]]**: Users, Groups, Roles. Policies = JSON. 
	- **Instance Profiles:**  [[IAM#Instance Profiles (for EC2 Roles)]] Attaching a role to an EC2
	- **STS**: temp creds. [[IAM#II. STS]]

	- **Permission Boundaries**: max allowed permissions.  [[IAM#VIII. IAM Permission Boundaries]]
	- **SCPs (Service Control Policies)**: Org-level restrictions.  [[IAM#V. Organizational Units (OU) ]]
	- **AD options**: Managed AD, AD Connector, Simple AD. [[IAM#III. AWS AD(Active Directory)]]
- **[[KMS]]**: envelope encryption, CMK vs AWS-managed keys. Default AES-256
	- **CloudHSM**: dedicated HSM cluster. [[KMS#IV. CloudHSM]]
- **Secrets Manager vs Parameter Store**: [[SSM & Secrets Manager]]
	- Secrets: automatic rotation + secrets ( newer)
	- Param store: config, free tier 
- **Shield**: DDoS Layer 4; Standard = free, Advanced = $ + 24/7 SOC.  [[Shield & WAF]]
- **WAF**: Deployed on ALB, CloudFront and API Gateway; DDos Leyer 7, IPs, geo, headers, SQLinjection /XSS. [[Shield & WAF#II. WAF → XSS, CSRF, SQL Injection]]

---

## 📊 Monitoring & Ops
- **[[CloudWatch]]**: Metrics, Logs, Alarms, Dashboards.
- **[[CloudTrail]]**: API call auditing, S3 bucket storage.
- **AWS Config**: compliance snapshots of resources. [[Cloudtrail#II. AWS Config]]
- **CloudWatch vs CloudTrail vs Config**: [[Cloudtrail#III. CloudWatch vs CloudTrail vs Config]] 
  - CW = performance metrics.  
  - CT = API auditing.  
  - Config = resource state/compliance.  
- **X-Ray**: Distributed tracing.
- **Trusted Advisor**: Cost, Performance, Security, Fault Tolerance, Limits. [[Other Services#X. Trusted Advisor]]

---

## ⚡ Serverless & Integration
- **[[Lambda]]**: Event-driven, 15 min max, concurrency controls.
- **Lambda@Edge**: run code at CloudFront edge locations. [[Lambda#II. Lambda@Edge]]
- **Step Functions**: Standard: Long running vs Express: Short fast cheap (duration/cost). [[SWF & Step Functions#II. Step Functions]]
- **API Gateway**: REST, HTTP, WebSocket APIs.
- **[[SNS]]**: pub/sub.
	- Max payload: **256 KB**
	- 30k TPS per topic
	- Up to 12.5M subscribers
- **[[SQS]]**: queue decoupling; 
	- - Max message: **256 KB**
	- Retention: **1m – 14d**
	- Standard: unlimited throughput
	- FIFO: **300 msg/s (3,000 with batching of 10)**
	- Visibility timeout: up to **12 hours**
	- Visibility Timeout must > job duration.
- **SNS + SQS Fan-out**: common pattern.
- **Amazon MQ**: managed ActiveMQ/RabbitMQ, migration path. [[SQS#IV. Amazon MQ ("Migration")]]
- **[[Cognito]]**: User Pools (auth) + Identity Pools (federated access).
- **[[Kinesis]]**: 
	- **Kinesis Streams** collects and processes real-time data streams manually with full control.
	- **Kinesis Firehose**  delivers streaming data to destinations like S3 or Redshift with no coding.
	- **Kinesis Data Analytics** lets you run real-time SQL queries on streaming data for insights on the fly.

---

## 🌍 Content Delivery
[[CloudFront & Global Accelerator]]
- **CloudFront**: CDN + caching. Signed URLs/Cookies for restrictions.
- **Global Accelerator**: Anycast IPs, TCP optimizations (not caching).
- **Transfer Accelerator**:
Client in Australia trying to upload to S3 in Ohio? Transfer Acceleration. 
Want to **serve content fast**? CloudFront. 
Need **TCP/UDP app acceleration** with static IPs? Global Accelerator

---

## 🧩 [[HPC]]
- Placement groups: Cluster, Spread, Partition.
- FSx for Lustre = shared fast FS.
- EFA = HPC networking.
- AWS Batch = HPC batch job orchestration.

---
## 🚀 Deployment & Migration

### **[[CloudFormation]] (IaC)**
- Templates (JSON/YAML) → infra automation.
- **StackSets** → deploy stacks across **multiple accounts/regions**.
- Drift detection = check if manual changes differ from template.
### [[Beanstalk]] (PaaS)
- Deploy apps (Java, Python, Node.js, etc.) without managing infra.
- Handles provisioning (EC2, ELB, ASG, RDS).
- **You still pay** for underlying resources.
- Customization → via .ebextensions.
### ECS / Fargate / EKS
- **[[ECS]]**: AWS container orchestration (Docker).
    - Launch types: **EC2** (you manage cluster) or **Fargate** (serverless, AWS runs infra).
- [[ECS#III. EKS]]: Managed Kubernetes.
- [[ECS#II. Fargate]]: Serverless container compute (pay per task).
  
### SAM (Serverless Application Model) [[Other Services#VIII. SAM]]
- Extension of CloudFormation for **serverless apps**.
- Simplifies creation of **Lambda + API Gateway + DynamoDB**.
- CLI supports **local testing** of Lambdas.

---
## ☠️ [[Disaster Recovery]]
- **RTO** = time to recover.  
- **RPO** = data loss tolerated.  
- Strategies:
  - Backup/Restore (cheap, slow).
  - Pilot Light (core services always on).
  - Warm Standby (reduced size live env).
  - Multi-site (active-active, $).

---

# 🔑 AWS Comparison Tables

## Internet Access
| Feature | Internet Gateway | NAT Gateway | NAT Instance | Egress-Only IGW |
|---------|-----------------|-------------|--------------|-----------------|
| Direction | Inbound + Outbound | Outbound only | Outbound only | Outbound only (IPv6) |
| Managed? | Yes | Yes | No (you manage EC2) | Yes |
| Scaling | Fully | Auto (to 45 Gbps) | Manual (instance size) | Fully |
| High Availability | Multi-AZ resilient | Per AZ (deploy >1) | You manage | Multi-AZ resilient |
| Security Groups | ❌ | ❌ | ✅ | ❌ |
| Cost | Free | Hourly + data | EC2 price | Free |
| Use Case | Public subnets | Private subnets → internet | Custom NAT logic | IPv6 private → internet |

---

## VPC Endpoints
| Type | Works With | Scaling | Cost | Notes |
|------|------------|---------|------|-------|
| Gateway | S3, DynamoDB | Scales auto | Free | Routes in RT |
| Interface (PrivateLink) | Most AWS services, SaaS | ENI in subnet | $$$/hour + data | Keeps traffic in AWS backbone |

---

## RDS vs Aurora vs DynamoDB
| Feature | RDS | Aurora | DynamoDB |
|---------|-----|--------|----------|
| Type | Managed SQL (MySQL, Postgres, etc.) | Cloud-native SQL (MySQL/Postgres compatible) | Managed NoSQL |
| Replication | Multi-AZ, Read Replicas | 6 copies, 3 AZs, Aurora Replicas | Global Tables (multi-region) |
| Scaling | Vertical + Read Replicas | Horizontal read scaling, writer endpoint | Auto scale R/W throughput |
| Backup | Snapshots, auto backups | Continuous backups to S3 | Point-in-time restore |
| Global | Cross-region replicas | Aurora Global (<1s lag) | Global Tables |
| Use Case | Traditional apps | Cloud-optimized SQL | Serverless scale apps |

---

## S3 Storage Classes
| Class | Durability | Availability | Retrieval | Cost |
|-------|------------|--------------|-----------|------|
| Standard | 11 9s | 99.99% | ms | $ |
| IA | 11 9s | 99.9% | ms | $$ (retrieval fee) |
| 1Z-IA | 11 9s | 99.5% | ms | $$ |
| Glacier | 11 9s | Variable (mins–hrs) | cheap storage | $$$ retrieval |
| Glacier DA | 11 9s | Variable (12–48h) | cheapest storage | $$$ retrieval |

---

## Snow Family
| Device | Capacity | Compute | Use Case |
|--------|----------|---------|----------|
| Snowcone | 8TB HDD / 14TB SSD | EC2/Lambda | Rugged, portable, edge/remote |
| Snowball | 50TB / 80TB | No | Bulk transfer |
| Snowball Edge | 100TB | EC2/Lambda | Edge compute + storage |
| Snowmobile | 100PB | No | Datacenter migration |

---

## CloudFront vs Global Accelerator
| Feature | CloudFront | Global Accelerator |
|---------|------------|--------------------|
| Type | CDN (caching) | Network accelerator (TCP/UDP) |
| Improves | Latency + caching content | Latency + routing packets |
| IPs | DNS-based | Anycast static IPs |
| Use Case | Distribute static/dynamic content | Gaming, VoIP, global apps |

---

## Data Migration
| Service | Online/Offline | Destinations | Notes |
|---------|----------------|--------------|-------|
| DataSync | Online | S3, EFS, FSx | 10Gbps/agent, supports DX + PrivateLink |
| Storage Gateway | Hybrid | S3 | File/Volume/Tape modes, live access |
| Snowcone | Offline | S3, EBS | Small, rugged |
| Snowball | Offline | S3, EBS | Medium, no compute |
| Snowball Edge | Offline | S3, EBS | Has compute |
| Snowmobile | Offline | S3 | 100PB per truck |
Missing EBS Types and boot volumes
Missing SQS, SNS caracter

Direct connect NO = Private Link

ElacticCahe Memcached repasar


Repassar security groups, bloquejar ips, api gw SG, etc.