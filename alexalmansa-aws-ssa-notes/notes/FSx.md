## I. FSx for Windows File Server

- Fully managed, native **Windows NTFS** + SMB shares.
- Integrates with **Active Directory** (Kerberos, ACLs).
- Use case: lift-and-shift Windows apps needing shared file storage.
- Scale: up to **64 TB per volume**, multi-AZ, daily S3 backups via VSS.
- Migration tool: **RoboCopy**.
- Best option for shared access between Windows and Linux

---

## II.  FSx for Lustre

- **Linux + Cluster = Lustre** → HPC workloads.    
- Parallel distributed FS, <1 ms latency.
- **100s of GB/s throughput, millions of IOPS.**
- Integrates with S3 (“hot” Lustre tier + “cold” S3).
- Common in ML, genomics, finance sims.

---

## III.  FSx for NetApp ONTAP

- Basically **managed NetApp filer in AWS**.
- Features you’d expect from ONTAP:
    - **NFS, SMB, iSCSI** access.
    - Snapshots, cloning, thin provisioning.
    - Tiering to S3 for cold data.
    - Multi-protocol support (Linux + Windows mixed envs).
- Great if your on-prem shop is already NetApp and you want **hybrid workflows** without retraining people.
- Scales from **1 TiB up to multiple PBs**.
- Still fully managed, integrated with AD + KMS encryption.

---

## IV. FSx for OpenZFS

- **Managed ZFS in AWS.**
- LinuxOnly NFS-based, but with ZFS goodies: snapshots, clones, compression.
- Low-latency, single-digit ms, good for dev/test or apps already on ZFS.
- Open-source leaning customers, HPC, analytics.
- Cheaper, simpler than Lustre if you don’t need extreme HPC scale.

---

## 🔑 FSx Exam Traps

- **EFS vs FSx**:
    - **EFS** = simple, POSIX-compliant, **Linux only**, NFS protocol.
    - **FSx** = “bring your own enterprise FS” → AWS runs Windows, Lustre, NetApp ONTAP, or OpenZFS for you.
    
- **FSx for Windows**:
    - Speaks **SMB (CIFS)**.
    - Integrates with **Active Directory (AD/LDAP)** → ACLs & identity-based access.
    - Target: Windows shops, user home dirs, shared drives.
    
- **FSx for Lustre**:
    - Built for **HPC & ML** workloads.
    - Ultra-low latency, massive throughput.
    - **Native S3 integration** → mount S3 as a file system (hot tier in FSx, cold tier in S3).
    
- **FSx for ONTAP**:
    - Full **NetApp experience** in AWS.
    - **Multi-protocol**: NFS, SMB, iSCSI.
    - Snapshots, cloning, deduplication, thin provisioning — all the NetApp goodies.
    - Target: enterprises migrating NetApp workloads without refactoring.
    
- **FSx for OpenZFS**:
    - **NFS only** (v3, v4.1, v4.2).
    - Classic **ZFS features**: snapshots, clones, compression, deduplication.
    - Great for Linux workloads, dev/test, analytics, or ZFS-native orgs.
	
- **Common to all FSx flavors**:
    - **Encrypted** at rest (KMS) + in transit.
    - **Backed up daily to S3** (Volume Shadow Copy Service for Windows).
    - **PCI & HIPAA compliant.**
    - **Billing** = storage capacity + throughput capacity + backup storage (all hourly).