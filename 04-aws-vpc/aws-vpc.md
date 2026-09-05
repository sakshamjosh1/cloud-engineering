# AWS VPC — Virtual Private Cloud

---

## Why VPC Exists

**The 2013–2014 problem:**

Before VPC, all companies sharing AWS infrastructure in a region were on the **same network**.

```
Mumbai Region
└── AWS Shared Server
    ├── Company 1's resources
    ├── Company 2's resources
    └── Company 3's resources
```

A hacker breaches Company 1 → now on the same server → **resources of C2 and C3 are also in danger** → entire server can be compromised.

**AWS introduced VPC to fix this** — each company gets their own **isolated private network** inside AWS.

---

## What is a VPC?

A VPC is a **logically isolated network** within an AWS region that you fully control.

- Your resources (EC2, RDS, etc.) live inside your VPC
- No other AWS customer can see or access your VPC
- You define the IP range, subnets, routing, and security rules

---

## VPC Components

```
VPC (172.16.0.0/16)
├── Public Subnet (172.16.1.0/24)   ← accessible from internet
│   ├── Internet Gateway             ← entry/exit point to internet
│   ├── Route Table                  ← rules for where traffic goes
│   └── EC2 instances
├── Private Subnet
│   ├── EC2 / RDS instances          ← not directly accessible from internet
│   ├── Route Table
│   └── NAT Gateway                  ← lets private instances reach internet
└── Security controls
    ├── Security Groups              ← instance-level firewall
    └── NACLs                        ← subnet-level firewall
```

---

## CIDR & IP Addressing

| Your notes | What it means |
|-----------|--------------|
| `172.16.0.0/16` | VPC IP range — `/16` gives **65,536** IP addresses (255×255) |
| `172.16.1.0/24` | Subnet range — `/24` gives **256** IPs |
| Subnets carve out chunks of the VPC's IP range | Each subnet lives in one AZ |

> **Exam tip:** VPC CIDR range must be between `/16` (65,536 IPs) and `/28` (16 IPs).

---

## Key Components Explained

### Internet Gateway (IGW)
- Attached to the VPC
- Allows resources in **public subnets** to communicate with the internet
- Without IGW → no internet access at all

### Public Subnet
- Has a route in its Route Table pointing to the IGW
- Resources here **can be accessed from the internet** (e.g. web servers, load balancers)

### Private Subnet
- No direct route to IGW
- Resources here are **not reachable from the internet** (e.g. databases, app servers)

### Route Table
- A set of rules that decides **where network traffic goes**
- Every subnet is associated with a route table
- Public subnet route table → has route to IGW
- Private subnet route table → has route to NAT Gateway

### NAT Gateway
- Sits in the **public subnet**
- Lets private subnet instances **download from internet** (outbound only)
- **Masks the private IP** — internet sees NAT Gateway's IP, not the instance's
- Traffic cannot be initiated **from** the internet to private instances

```
Private EC2 → NAT Gateway → Internet Gateway → Internet
Internet    → Internet Gateway → BLOCKED (can't reach private EC2)
```

### Security Groups
- **Instance-level** firewall
- Stateful — if inbound is allowed, response is automatically allowed
- You define: which ports, which protocols, which IPs can connect

### NACLs (Network Access Control Lists)
- **Subnet-level** firewall
- Stateless — inbound and outbound rules evaluated separately
- Applied before traffic reaches instances

| | Security Group | NACL |
|-|---------------|------|
| Level | Instance | Subnet |
| Stateful | Yes | No |
| Default | Allow all outbound | Allow all |
| Use for | Fine-grained instance access | Broad subnet-level control |

---

## Elastic Load Balancer (ELB)

- Lives in the **public subnet**
- Receives traffic from the internet
- Distributes it across EC2 instances in **private subnets** via **Target Groups**

```
Internet → Internet Gateway → ELB (public subnet)
                                    ↓
                             Target Group
                                    ↓
                    EC2 instances (private subnet)
```

- You assign EC2 instances to a **Target Group**
- ELB routes traffic to healthy instances in the target group

---

## Real-World Example — TCS on AWS

TCS has multiple projects → each project has its own set of EC2 instances inside a VPC.

```
TCS VPC (Mumbai Region)
├── Project A → EC2s in private subnet
├── Project B → EC2s in private subnet
└── Shared services → public subnet (ELB, NAT)
```

Each project is isolated — a breach in one doesn't expose others.

---

## Key Terms

| Term | Meaning |
|------|---------|
| VPC | Isolated private network in AWS |
| CIDR | IP address range notation (e.g. `172.16.0.0/16`) |
| IGW | Internet Gateway — connects VPC to internet |
| NAT Gateway | Lets private instances access internet outbound only |
| Route Table | Rules for directing network traffic |
| Public Subnet | Subnet with IGW route — internet accessible |
| Private Subnet | Subnet without IGW route — internal only |
| Security Group | Instance-level stateful firewall |
| NACL | Subnet-level stateless firewall |
| ELB | Elastic Load Balancer — distributes traffic across instances |
| Target Group | Group of EC2s that ELB routes traffic to |
