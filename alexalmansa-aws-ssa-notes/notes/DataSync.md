## 1. What It Is
- Fully managed **online** data transfer service.
- Moves **millions of files** between:
  - On-prem (NFS, SMB)
  - AWS (S3, EFS, FSx family)
- Works over **Internet**, **Direct Connect**, or **PrivateLink**.
- Handles:
  - Scheduling (hourly/daily/weekly)
  - Filters (only move what you want)
  - Integrity checks (byte-for-byte)

---

## 2. Core Facts You Must Know
1. **Agent** required for on-prem sources — runs as:
   - VM (VMware, Hyper-V, KVM)
   - EC2 (AWS-to-AWS transfers)
2. **Speed** – one agent can saturate **10 Gbps**.
3. **Security**:
   - TLS encryption in-flight
   - Never stores your data in DataSync
   - PCI + HIPAA compliant
4. **Billing** – pay for GB moved, nothing else.
5. Can write **directly to S3 Glacier Deep Archive** (skip S3 → Glacier step).
6. Not for bandwidth-starved sites → use Snowball/Snowcone instead.

---

## 3. DataSync Transfer Types (Online + Offline Options)

### Online
- **AWS DataSync** → Automated over Internet, Direct Connect, or PrivateLink.  
- Best for active/ongoing transfers and when bandwidth allows.
### Offline – AWS Snow Family
| Device | Capacity | Extras | Use Case |
|--------|----------|--------|----------|
| **Snowcone** | 8TB HDD or 14TB SSD | Runs EC2/Lambda | Small, rugged, remote sites with little space/power |
| **Snowball** | 50TB / 80TB | Storage only | Bulk one-time transfers |
| **Snowball Edge** | 100TB | EC2/Lambda | Remote compute + storage |
| **Snowmobile** | 100PB | None | Entire datacenter migrations |

---

## 4. Network Options
**a) Internet**  
Public endpoints, simplest setup.

**b) Direct Connect** [[VPC#XVI. Direct Connect (DX)]]
- **Private VIF** → Access VPC private IPs (EFS mount targets, PrivateLink endpoints).
- **Public VIF** → Access AWS public endpoints (like S3).

**c) AWS PrivateLink (Interface VPC Endpoint)** [[VPC#XVIII. AWS PrivateLink]] 
- Private API access to EFS, FSx, or DataSync from:
  - On-prem over DX/VPN
  - Another VPC
- Keeps traffic off the internet.

**d) VPC Peering**  [[VPC#III. Peering (Do this after section XV)]]
Only for VPC-to-VPC, **NOT** from on-prem over DX.

---

## 5. Special Case – EFS Over Direct Connect
- EFS has **no public endpoint**.
- To hit EFS from on-prem:
  1. Use **Private VIF** to your VPC.
  2. Add **PrivateLink Interface Endpoint** for `efs.api`.
  3. Point DataSync at the mount target via the endpoint.
- Result → traffic stays inside AWS backbone.

---

## 6. Scheduling & Reliability
- Schedule jobs: hourly, daily, weekly.
- Incremental sync – only changed files moved.
- Auto-resume after interruptions.

---

## 7. DataSync vs. Others
| Service | When to Use |
|---------|-------------|
| **DataSync** | Online transfers, automated scheduling, AWS-native targets |
| **Storage Gateway** | Hybrid access (on-prem + cloud active usage) |
| **Snowball/Snowcone** | Offline/bandwidth-limited large migrations |
| **S3 Transfer Acceleration** | Already integrated with S3 API, need global speed boost |

---

## 8. Exam Traps & Tips
- If the target is **EFS/FSx from on-prem** → Private VIF + PrivateLink wins.
- If the target is **S3 from on-prem** → Public VIF or internet direct to S3.
- Avoid multi-step solutions (S3 → Lambda → EFS) unless absolutely necessary.
- VPC Peering will **never** solve on-prem → AWS over DX.
