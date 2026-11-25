## High Performance Computing

1. To transfer data, you can use
    1. Direct Connect — GB/s connection to transfer to cloud over a private secure network
    2. Snowball and Snowmobile — transfer PB/EB scale data
    3. DataSync — Install agent on prem to move data to S3, EFS, FSx
    
2. Compute    
    1. CPU and GPU optimized
    2. Spot instances / Spot fleets for cost savings + Auto scaling
    3. Cluster placement group for max network performance
 3.  **Placement Groups**
	- **Cluster** → Best for **HPC**, low-latency network, same AZ.
	- **Partition** → Best for **distributed systems**, fault isolation.
	- **Spread** → Best for **critical small workloads**, across hardware.
	
4. **EC2 enhanced networking (ENA or SR-IOV)**
    1. Very high network throughput
    2. Available on Windows
    3. Low inter-instance latency
    4. Higher bandwidth, higher PPS (packets per second), lower latency
    5. Can be done via Intel 82599 VF (legacy, 10Gbps) or ENA (new, 100Gbps)
    
5. **Elastic Fabric Adapter (EFA)**
    1. Improved ENA for HPC, **only works for linux**
    2. EFA devices provide all ENA devices functionalities plus a new OS bypass hardware interface that allows user-space applications to communicate directly with the hardware-provided reliable transport functionality.
    3. Great for inter-node communications, tightly coupled workloads
    4. Leverages **Message Passing Interface** (MPI) which can bypass underlying OS calls to improve latency
    
6. Storage
    1. Instance-attached storage
        1. EBS can scale up to 64000 IOPS with IO1 provisioned IOPS
        2. Instance store: scale to millions of IOPS, linked to EC2 memory, low latency
    2. Network storage
        1. S3
        2. EFS
        3. FSx for Lustre
            1. HPC-optimized distributed file system
            2. Millions of IOPS
            3. Backed by S3
	
7. Automation and orchestration
    1. AWS Batch: Supports multi-node parallel jobs over EC2s, schedule jobs and launch easily.
    2. AWS Parallel Cluster
        1. Open source cluster management tool to deploy HPC on AWS
        2. Configure with text files
        3. Automate creation of VPC, subnet, cluster type etc