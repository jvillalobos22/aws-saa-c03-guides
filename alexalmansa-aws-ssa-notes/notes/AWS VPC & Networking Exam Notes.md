  
## I. VPC Fundamentals (Foundation)
### 1. What is a VPC
- Logically isolated virtual network in AWS.
- Full control over:
  - IP ranges (CIDR)
  - Subnets
  - Route tables
  - Gateways
- Better security and isolation than classic AWS networking.
### 2. Uses of a VPC
- Launch instances into chosen subnets.
- Assign custom IP ranges.
- Configure routing between subnets.
- Attach Internet Gateways.
- Apply security groups and NACLs.

---
## II. CIDR, IP Addressing & Limits

### 1. VPC CIDR rules
- Private IP ranges only:
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- VPC CIDR must **not overlap** with:
  - Other VPCs
  - Corporate networks
### 2. CIDR limits
- Max VPC CIDRs: **5**
- CIDR size:
  - Min `/28` → 16 IPs
  - Max `/16` → 65,536 IPs
### 3. Reserved IPs (per subnet)

AWS always reserves **5 IPs**:
1. Network address
2. VPC router
3. AWS DNS
4. Reserved for future use
5. Broadcast address (not usable)
---
## III. Subnets

### 1. Subnet basics
- Each subnet belongs to **one AZ**.
- Each subnet must be associated with **one route table**.
- Route table ↔ Subnet = **1:Many**.

### 2. Public vs Private subnets
- **Public subnet** → route to an Internet Gateway.
- **Private subnet** → no route to an Internet Gateway.  
### 3. Default subnet behavior
- Default subnets are **public**.
- Auto-assign public IPv4 = `true`.
- Non-default subnets:
  - No public IP.
  - No public DNS (unless enabled).
---

## IV. Route Tables
### 1. Route table basics
- Set of rules to direct traffic.
- Main route table exists by default.
- Cannot delete main route table (but can replace association).
### 2. Routing behavior
- **Longest Prefix Match**.
- Can associate with:
  - Subnets
  - Gateways (IGW / VGW)

---
## V. Internet Gateway (IGW)

### 1. IGW fundamentals
- Enables internet access.
- Horizontally scalable, HA, redundant.
- One IGW per VPC.
- Unlimited bandwidth.
### 2. Important exam rules
- IGW alone does nothing → route table needed.
- Required for:
  - Public EC2 inbound traffic
  - NAT Gateway
  - Public Load Balancers

---
## VI. Security Layers (VERY IMPORTANT)

### 1. Security Groups
- Firewall for **instances**.
- **Stateful**.
- Union of all rules.
- Can reference other security groups.
### 2. Network ACLs
- Firewall for **subnets**.
- **Stateless**.
- Rules evaluated by number (lowest first).
- Newly created NACL = deny all.
### 3. Ephemeral ports
- Range: `1024–65535`.
- Must allow return traffic (especially for NACLs).

---
## VII. Internet Access Patterns

### 1. Public EC2 access (Inbound)
Requires:
- Internet Gateway.
- Route `0.0.0.0/0 → IGW`.
- Public or Elastic IP.
- Security Group + NACL allow traffic.
- **NAT Gateway NOT needed**.

### 2. NAT Instance (Legacy)
- EC2 acting as router.
- Disable source/destination check.
- Requires Elastic IP.
- You manage HA, scaling, patching.
### 3. NAT Gateway (Modern)
- AWS-managed.
- Scales automatically.
- Single AZ.
- No Security Groups (uses NACLs)
- **One NAT Gateway per AZ for HA**.
- Requires IGW.

### 4. Egress-only IGW (IPv6)
- IPv6 outbound-only internet access.
- Prevents inbound internet traffic.
---
## VIII. DNS in VPC
### 1. DNS attributes
- `enableDnsSupport`
- `enableDnsHostnames`
### 2. Important exam behavior
- Interface endpoints rely on DNS override.
- Private hosted zones require both enabled.

---

## IX. Private Access to AWS Services
### 1. VPC Endpoints
- No IGW / NAT required.
- Traffic stays on AWS backbone.
#### Interface Endpoints
- ENI + Security Group.
- Most AWS services.
- Private DNS override.
- Same region only.
#### Gateway Endpoints
- Route table entry.
- **Only S3 and DynamoDB**.
### 2. AWS PrivateLink
- Secure service exposure to other VPCs.
- Uses NLB + ENIs.
- No peering, IGW, or NAT.
- Scales to thousands of VPCs.
---
## X. VPC-to-VPC Connectivity (East–West)

### 1. VPC Peering
- Non-overlapping CIDRs.
- No transitive routing.
- No edge-to-edge routing.
- Route tables must be updated manually.
### 2. Transit VPC (Architecture)
- DIY hub-and-spoke using EC2.
- High operational overhead.
### 3. Transit Gateway (Managed)
- Regional virtual router.
- Star topology.
- Supports:
  - VPCs
  - VPN
  - Direct Connect
- Share via RAM.
- Supports multicast.
---
## XI. Hybrid Connectivity (On-Prem ↔ AWS)

### 1. Site-to-Site VPN (Quick, Encrypted, Internet-based)

- Uses **Virtual Private Gateway (AWS)** + **Customer Gateway (on-prem)**.
- Traffic goes over the **public internet**.
- **IPSec encrypted by default**.
- Fast to set up, low upfront cost.
- Less predictable performance (latency depends on internet).
**Best for:**  
- Getting started quickly  
- Low to medium traffic  
- Backup / failover for Direct Connect
### 2. VPN CloudHub (Multi-Site VPN)

- **Hub-and-spoke VPN model**.
- Connects **multiple on-prem sites** together via AWS.
- Uses Site-to-Site VPN connections.
- Encrypted, low cost.
- Useful when sites need to talk **to each other**, not just AWS.
**Best for:**  
- Multiple branch offices  
- Simple, low-cost multi-site connectivity
### 3. Direct Connect (DX) (Private, Predictable, Expensive)

- **Dedicated private connection** from on-prem to AWS.
- Does **NOT use the public internet**.
- **Not encrypted by default**.
- Setup takes time (weeks) and costs money.
- Very **stable latency and throughput**.
**Best for:**  
- Large, constant data transfers  
- Latency-sensitive workloads  
- Long-term hybrid environments
> 🔐 **Encryption tip:**  
> Use **VPN over Direct Connect** to add IPSec encryption.

### 4. Direct Connect Gateway (Multi-VPC / Multi-Region)

- Allows **one Direct Connect** to connect to:
  - Multiple VPCs
  - Multiple regions
- Simplifies hybrid architectures at scale.
- Connected VPC CIDRs **must not overlap**.
**Best for:**  
- Large AWS environments  
- Centralized on-prem connectivity  
- Multi-region architectures

---
## XII. Observability & Troubleshooting

### 1. VPC Flow Logs
- Capture traffic metadata.
- VPC / Subnet / ENI level.
- Deliver to S3 or CloudWatch.
- Query with Athena or Logs Insights.
---
## XIII. Cost & Exam Tips

- Same AZ traffic is cheapest.
- NAT Gateways and Elastic IPs cost money.
- Cross-AZ traffic costs more.
- Peering does not scale.
- Transit Gateway > Peering at scale.

---
## XIV. Legacy & Edge Cases

### 1. EC2 Classic & ClassicLink (deprecated)
Previously, EC2 instances used to run on a single network which was shared with other customers. Then came VPC that made logical isolation of services within accounts.

AWS Classic Link is a way of connecting Classic EC2s to VPC in your account.
### 2. AWS Outposts
**AWS hardware installed in your own data center**.

You get the same AWS services (EC2, EBS, RDS, etc.) running **locally**, managed through the AWS console.

Used when you need **low latency**, **local data storage**, or **compliance** but still want full AWS integration.

---

## 🔑 Final Mental Model (Exam Gold)

- Public subnet = route to IGW.
- Security Group = stateful, instance-level.
- NACL = stateless, subnet-level.
- NAT ≠ IGW.
- Peering ≠ transitive.
- Transit Gateway for scale.
- VPC Endpoints replace NAT for AWS services.