## I. Introduction ![[Pasted image 20250514092946.png|100]]

1. RDBMS in AWS is called RDS (Relational Database Service)
2. Used in OLTP scenarios.
3. DB instance is the building block.
4. Only pay for the instances that are active.
5. RDS includes SQL Server, Oracle, MySQL, MariaDB, PgSQL and Amazon Aurora.
6. Key features of RDS include:
    - Automatic provisioning, OS patching
    - Continuous backups and restore to specific timestamp
    - Monitoring dashboards
    - Multi-AZ for disaster recovery
    - Read-replicas for performance
    - Storage backed by EBS (GP2 or IO1)
7. RDS runs on virtual machines but you can't log into them.
    - Patching is Amazon's responsibility
    - It is NOT serverless (except Aurora)
8. AWS also provides data warehousing with tools like Cognos, JasperSoft, SQL Server Reporting Services, Oracle, Hyperion, SAP NetWeaver.
    - These are used to pull in very large and complex data sets. Usually used by management to do queries on data
    - Redshift is AWS' data warehousing solution
9. It's important to identify what kind of database you need based on use case — OLAP vs OLTP.
10. ElastiCache is a web service that makes it easy to deploy, operate and scale in-memory cache in the cloud. The service improves the performance of web apps. ElastiCache supports two open-source in-memory caching engines:
    - redis
    - memcacheD
11. RDS supports encryption at rest and is done using [[KMS]]. Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted along with its automated backups, read-replicas etc
12. For MS SQL, the max size RDS volume is 16 TB
13. Regular metrics available by default including CPU utilization, database connections and freeable memory

## II. RDS for SA (Solutions Architect)

1. Operations
    1. Small downtime when failover/maintenance happens
    2. Scaling in read replicas/EC2 instance/restore EBS implies manual interventions
2. Security
    1. AWS responsible for OS security
    2. We are responsible for setting up KMS, security groups, IAM policies and user auth
3. Reliability: Multi AZ feature, failover in case of failures
4. Performance: Depends on EC2 instance type, EBS volume type, ability to add read replicas. Does not auto scale.
5. Cost: Pay per hour based on provisioned EC2 and EBS

## III. Enhanced Monitoring

1. You are charged for using enhanced monitoring
2. You can enable "Enhanced Metrics" to capture up to 50 metrics about your RDS instance
3. Supports all RDS engines
4. Supports all instance types except `t1.micro` and `m1.small`
5. You can get a precision of 1s and up to 1 hour back
6. The metrics are delivered into your CloudWatch account along with RDS dashboard
7. The metrics can be exported as JSON payloads
8. Supports metrics like
    1. RDS processes
    2. OS processes
    3. RDS child processes

### RDS PROXY
RDS proxy is a HA ( High Availability), fully managed database proxy for RDS that can improve scalability by pooling and sharing database connection, improve availability and enhance security. Can add an average latency of 5ms
## IV. RDS Tips

1. Instances are billed for DB instance hours (per second), storage (per GB per month), IO (per 1 million requests per month), provisioned IOPS, backup storage and data transfer.
2. RDS purchasing options — on demand and reserved instance
3. If your client app is caching the DNS of your db service, set a TTL < 30s
4. **You can query error logs (default), slow query logs and general logs**
5. Multi AZ = sync replication, highly durable
6. Read replicas = async replication, highly scalable
7. You can run RDS on-prem using RDS on Outpost or VMware
8. By default, customers are allowed up to 40 RDS instances
9. You can use AWS DMS to migrate data to RDS
10. If you don't specify a maintenance window, RDS would book a 30-min window by default
11. Not every db engine is available in every region
12. You can test your db patch update by creating a snapshot of your main instance and restoring from there
13. While your db instance is stopped, you pay for provisioned storage (including provisioned IOPS) and backup storage but not db instance hours
14. Backup storage cost more because the backup is replicated across multiple AZs to provide durability
15. You can use RDS RI to reserve a db instance for a 1-3 year term and get discounts. You can get up to 40 RDS RIs.
16. You can convert an existing db to RDS RI
17. You cannot cancel RI and the one-time payment is not refundable
18. You can modify the compute resources and storage capacity from the RDS console
19. A db subnet group is a collection of subnets that you may want to designate for your RDS db instances in a VPC. It is strongly recommended to use DNS name to connect to db.
20. Migration of db from VPC to outside is not supported but vice versa is true
21. You are responsible for modifying route tables and ACLs to make sure your apps can talk to RDS inside VPC
22. You can use AWS Config to continuously record configurations changes to RDS db instance
23. You can have Multi-AZ && Read Replicas simultaneously
24. You can have multi-AZ for read replicas
25. Supports push-button scaling (vertical scaling)
26. **The recommended storage engine for MySQL is InnoDB. However, in case you require intense, full-text search capability, use MyISAM storage engine instead.**
27. For creating short-lived tokens, you can use an **IAM database authentication** when connecting to a database instance.
28. Authentication is handled by `AWSAuthenticationPlugin` — an AWS-provided plugin that works seamlessly with IAM to authenticate your IAM users.
29. You CANNOT create a cross-region Multi-AZ deployment with RDS.
30. **By default, RDS forces SSL connections for all connections.**
31. To enable user-access with short-lived credentials, you can use `AWSAuthenticationPlugin`
32. **Security patching requires db to be offline for certain time.**
33. **To connect a RDS instance from on-prem server, you'd need to place it in a public subnet. You CANNOT put it in a private subnet and use NAT Gateway.**
34. To connect to RDS from an application hosted on-prem
    1. Make the RDS instance publicly accessible and place it in a public subnet
    2. Create a security group allowing access from the on-premise public IP to the RDS instance and assign to the RDS instance
35. If you want to be able to **customize database server host and OS, Amazon RDS Custom**


## RDS Main Features
### Multi AZ 

1. Multi AZ allows you to have an exact copy of your production database in another Availability Zone.
2. It is active-passive
3. AWS handles the replication for you, so when your production database is written to, it's automatically synced with the standby (same DNS name).
4. In the case of failure/maintenance, RDS would automatically switch to the standby without admin intervention in case of following events:
    1. AZ outage
    2. Primary DB instance fails
    3. DB instance server type is changed
    4. OS is going through patching
    5. Manual failover was initiated
    
5. Its main objective is disaster-recovery than performance improvements, for which you'd need read-replicas.
6. Read replicas can be set as multi-AZ. Aurora has Multi-AZ in-built by default.
7. **Multi-AZ cannot serve read requests, they are not meant to be used as read replicas. They are only meant to be used as stand-by (except Aurora which can be both multi-AZ and read replica at the same time).**
8. Failover is automatically handled by RDS and it will flip the CNAME record to point to the standby instead.
9. You can create a read replica as a Multi-AZ DB instance. Amazon RDS creates a standby of your replica in another Availability Zone for failover support for the replica. Creating your read replica as a Multi-AZ DB instance is independent of whether the source database is a Multi-AZ DB instance.
10. **Multi-AZ follows synchronous replication** and spans at least two Availability Zones (AZs) within a single region. **Read replicas follow asynchronous replication** and can be within an Availability Zone (AZ), Cross-AZ, or Cross-Region
11. If you do an upgrade, in multiAz setup. Both primary and standby are updated at the same time, and there's downtime.

### Read Replicas

1. They allow you to have read-only copy of your production database for read-heavy apps (eg Wikipedia)
2. This is achieved using **async** replication from the primary RDS instance to the read replica
3. Used for scaling, not for disaster recovery
4. **Must have automatic backups turned on in order to deploy a read replica**
5. You can have up to **5 read replica copies** of any database
6. You can have read replicas of read replicas (except Pgsql, Oracle and SQL Server)
7. **Each read replica will have its own DNS endpoint**
8. **They can be cross-AZ and cross-region**
9. If the master and Read Replica are in different regions, you encrypt using the encryption key for that region.
10. They can be promoted to master (breaks replication)
11. Not available for SQL Server instances
12. **If you want the read replica to be HA itself, create a read replica as a multi-AZ DB instance.**
13. You can track how far behind a replica is from master using CloudWatch's "Replica Lag" feature
14. A read replica will stay active even if the master is deleted
15. No extra charge for replication within same AZ
16. Pgsql requires physical replication. MySQL and MariaDB do logical replication.
17. They are _eventually_ consistent
18. You can ElastiCache in conjunction with read replicas
19. If you create Read Replica in another region, you are charged for data transfer. If it is in the same region you are not.
![[Pasted image 20250514094917.png|600]]
![[Pasted image 20250514094936.png]]
### RDS Backups

#### Automated Backups

- Allows you to recover your database to any point in time within the specified retention period (Max 35 days)
- Takes daily snapshots and stores transition logs
- When recovering AWS will choose the most recent backup
- Enabled by default
- Backup data is stored in S3
- May experience latency when backup is being taken
- Backups are deleted once you remove the original RDS instances

#### Database Snapshot

- User-initiated, must be manually done by yourself
- Stored until you explicitly delete them, even after you delete the original RDS instance they are still persisted. However this is not the case with automated backups.

## Multi-AZ, Multi-Region, and Read Replicas
| Feature              | Multi-AZ Deployments                            | Multi-Region Deployments              | Read Replicas                 |
| -------------------- | ----------------------------------------------- | ------------------------------------- | ----------------------------- |
| **Main purpose**     | High availability, durability                   | Disaster recovery & local performance | Scalability, performance      |
| **Replication type** | Non-Aurora: synchronous<br>Aurora: asynchronous | Asynchronous                          | Asynchronous                  |
| **Scope**            | At least 2 AZs in one region                    | Each region can have Multi-AZ         | AZ, Cross-AZ, or Cross-Region |
## V. Pages
[[RDS VS DynamoDB]]
[[Databases]]
[[Data Transfer & Migration Options]]
