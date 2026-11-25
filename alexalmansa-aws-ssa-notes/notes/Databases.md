# **🧠 AWS Database Decision Guide**
## **1. 🧭 Choosing the Right Database – Key Questions**

| **Question**                     | **What to Consider & AWS Fit**                                                                              |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Workload type**                | Read-heavy → Aurora, RDS with Read Replicas<br>Write-heavy → DynamoDB, DocumentDB<br>Balanced → RDS, Aurora |
| **Throughput & scaling**         | Spiky → DynamoDB (on-demand), ElastiCache<br>Predictable → RDS (provisioned IOPS)                           |
| **Data volume & growth**         | Growing → DynamoDB auto-scaling<br>Fixed or bursty →  S3,  ElastiCache, RDS auto-scaling                    |
| **Access pattern**               | Key-value → DynamoDB, ElastiCache<br>Relational/joins → RDS Aurora<br>Blob → S3                             |
| **Durability & source of truth** | Mission-critical → Aurora (6 copies, 3 AZs), DynamoDB with PITR, RDS Multi-AZ                               |
| **Latency & concurrency**        | Sub-ms → ElastiCache<br>Low-latency → DynamoDB Aurora Serverless v2                                         |
| **Data model**                   | Structured → RDS, Aurora<br>Semi-structured → DynamoDB, DocumentDB<br>Unstructured → S3                     |
| **Schema requirements**          | Fixed schema → RDS, Aurora<br>Flexible → DynamoDB, S3 + Athena                                              |
| **Reporting/Search needs**       | Reporting → Redshift, Athena<br>Search → OpenSearch, DynamoDB Streams + Lambda                              |
| **Cost & licensing**             | Cloud-native & cost-effective → Aurora, DynamoDB, Timestream                                                |

## **2. 🗃️ Database Types in AWS**
![[Pasted image 20250515143729.png]]
### **🧩 RDBMS (Relational, OLTP)**

| **Service**            | **Notes**                                                                                                                                                            |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Amazon [[RDS]] **    | Managed MySQL, PostgreSQL, Oracle, SQL Server, MariaDB, **Customs Oracle and SQL Server** for legacy complex apps . Supports Multi-AZ, read replicas, IAM, KMS, PITR |
| **Amazon [[Aurora]] ** | Cloud-native, MySQL/PostgreSQL compatibleAuto-scaling storage, 6-way replicated, Serverless & Global                                                                 |


### **🔗 NoSQL (Non-Relational)**

| **Service**     | **Type**           | **Notes**                                                       |
| --------------- | ------------------ | --------------------------------------------------------------- |
| [[DynamoDb]]    | Key-value/document | Fully serverless, global tables, DAX for caching, PITR          |
| **DocumentDB**  | Document           | MongoDB-compatible, managed and scalable                        |
| **Keyspaces**   | Wide-column        | Apache Cassandra-compatible, serverless, scalable               |
| [[ElastiCache]] | In-memory          | Redis or Memcached, sub-ms latency, read replicas, cluster mode |
| **Neptune**     | Graph              | Social graphs, fraud detection, recommendation engines          |

### **🪣 Object Storage**

| **Service**                | **Notes**                                           |
| -------------------------- | --------------------------------------------------- |
| [[S3]]                     | Scalable object store with 99.999999999% durability |
| **Glacier / Deep Archive** | For long-term backups, archival with slow retrieval |

### **📊 Data Warehousing & Analytics (OLAP)**

| **Service**  | **Notes**                                                    |
| ------------ | ------------------------------------------------------------ |
| [[Redshift]] | High-performance analytics, BI integration, columnar storage |
| [[Athena]]   | Serverless SQL on S3 data                                    |
| **EMR**      | Big Data analytics with Hadoop, Spark, Hive                  |

### **🔍 Search & Logs**

| **Service**                    | **Notes**                                                |
| ------------------------------ | -------------------------------------------------------- |
| **OpenSearch (ES)**            | Full-text search, log indexing, dashboarding with Kibana |
| **CloudWatch Logs + Insights** | Basic log query and metrics                              |

### **🧱 Specialized Databases**

| **Use Case**                      | **Service**            | **Notes**                                                |
| --------------------------------- | ---------------------- | -------------------------------------------------------- |
| **Ledger (immutable, auditable)** | **QLDB**               | Cryptographically verifiable transaction log             |
| **Time series data**              | **Timestream**         | Real-time IoT/metrics, SQL, in-memory + historical tiers |
| **Blockchain**                    | **Managed Blockchain** | For decentralized use cases                              |

## **3. 🔧 Feature Comparison Table (Quick Summary)**

| **Feature**        | **RDS**     | **Aurora**    | **DynamoDB**         | **ElastiCache** | **Redshift** | **DocumentDB** | **Neptune**   | **Timestream** | **QLDB**          |
| ------------------ | ----------- | ------------- | -------------------- | --------------- | ------------ | -------------- | ------------- | -------------- | ----------------- |
| Serverless         | 🚫/✅        | ✅ (v2)        | ✅                    | 🚫              | 🚫           | 🚫             | 🚫            | ✅              | ✅                 |
| Fully Managed      | ✅           | ✅             | ✅                    | ✅               | ✅            | ✅              | ✅             | ✅              | ✅                 |
| Global Replication | ✅ (via RDS) | ✅ (Global DB) | ✅                    | 🚫              | 🚫           | 🚫             | 🚫            | 🚫             | 🚫                |
| ACID Transactions  | ✅           | ✅             | ✅                    | 🚫              | 🚫           | 🚫             | 🚫            | 🚫             | ✅                 |
| SQL Compatible     | ✅           | ✅             | 🚫 (Partially)       | 🚫              | ✅            | 🚫             | 🚫            | ✅              | ✅                 |
| Use Case Fit       | OLTP        | Scalable OLTP | Serverless apps, IoT | Real-time cache | Analytics/BI | Mongo apps     | Social graphs | Time-series    | Financial ledgers |
### ⚙️ EBS volume cheat-sheet 
[[EBS & EFS]]

| **Type (API name)**                      | **Baseline / burst rules**                                                  | **Max IOPS**.        | **Max through-put**          | **When to choose it**                                                                                 |
| ---------------------------------------- | --------------------------------------------------------------------------- | -------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------- |
| **General Purpose SSD (gp2)**            | 3 IOPS per GiB (credits)                                                    | 16 000               | 250 MiB/s (size-scaled)      | legacy default; OK for small DBs or boot disks that need ≤ 16 k IOPS but you don’t want to tune knobs |
| **General Purpose SSD (gp3)**            | Fixed 3 000 IOPS baseline, tunable to 16 000 IOPS & 1 GiB/s                 | 16 000               | 1 000 MiB/s                  | cheapest path to predictable mid-range IOPS; perfect for staging or moderate OLTP                     |
| **Provisioned IOPS SSD (io1)**           | Provision 100 – 64 000 IOPS (≤ 50 IOPS/GiB; need ≥ 1 280 GiB for full 64 k) | **64 000**           | 1 000 MiB/s (Nitro required) | production SQL/NoSQL needing sustained latency; no RAID 0 needed up to 64 k IOPS                      |
| **Provisioned IOPS SSD (io2)**           | Same model as io1 but 99.999 % durability; up to 64 k IOPS                  | 64 000               | 1 000 MiB/s                  | higher durability replacement for io1                                                                 |
| **io2 Block Express**                    | Nitro-only; 4-TiB volumes or larger recommended                             | **256 000**          | 4 000 MiB/s                  | ultra-high OLTP, SAP HANA, HFT; biggest single-volume numbers on EBS                                  |
| **Throughput-optimised HDD (st1)**       | Credit bucket model (MiB/s)                                                 | 500 IOPS (1 MiB I/O) | **500 MiB/s**                | streaming analytics, big-data, log pipelines where $/GB beats IOPS                                    |
| **Cold HDD (sc1)**                       | Similar credit model                                                        | 250 IOPS             | 250 MiB/s                    | infrequently-accessed, large sequential datasets (e.g., archives)                                     |
| **Magnetic (standard)** _(previous gen)_ | ~100 IOPS avg (burst hundreds)                                              | 40–200               | 40–90 MiB/s                  | only for very small, rarely accessed data; consider S3 first                                          |

> **io1/io2 & RAID 0:** one properly sized io1/io2 volume already delivers 64 k IOPS; striping is only required if you need more than that, >1 GiB/s, or you’re combining cheaper gp3 volumes for cost / perf tuning. 

---

### **🚦 Quick decision ladder by peak IOPS**

| **Peak need**                            | **Recommended first stop**                              | **Why**                                                                |
| ---------------------------------------- | ------------------------------------------------------- | ---------------------------------------------------------------------- |
| **≤ 3 k IOPS**                           | gp3 at baseline                                         | Cheaper & steadier than gp2 credits                                    |
| **3 k – 16 k IOPS**                      | gp3 (provisioned) or large gp2                          | Same ceiling, gp3 lets you tune IOPS/throughput independently          |
| **16 k – 64 k IOPS**                     | io1/io2 single volume                                   | Direct provisioned IOPS; no RAID until you exceed 64 k                 |
| **64 k – 256 k IOPS**                    | io2 Block Express (Nitro) or **Aurora I/O-Optimized**   | Highest single-disk numbers; Aurora wraps Block Express under the hood |
| **> 256 k IOPS** or unpredictable spikes | **[[DynamoDB]] on-demand** or **ElastiCache [[Redis]]** | Horizontal scale, micro-latency, no per-volume limits                  |

Keep the network in mind: Nitro instances expose the EBS bandwidth you need—without them, the volume can’t hit its advertised numbers.