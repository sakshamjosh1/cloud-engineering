# AWS EC2 — Elastic Cloud Compute

---

## What is EC2?

**EC2 = Elastic Cloud Compute** — AWS's virtual machine service.

- **Elastic** = flexible, scale up or down on demand
- **Cloud** = runs on AWS public cloud
- **Compute** = CPU + RAM + Disk = virtual server

Before EC2, you bought physical servers from HP or IBM — managed upgrades, security, and costs yourself. AWS said *"I'll manage all of that"* → pay-as-you-go model.

---

## EC2 Instance Types

Choose based on your workload:

| Type | Optimized for | Example use case | Instance family |
|------|-------------|-----------------|----------------|
| **General Purpose** | Balanced CPU/RAM | Web servers, small DBs | `t3`, `m5` |
| **Compute Optimized** | High CPU | Batch processing, gaming servers | `c5`, `c6g` |
| **Memory Optimized** | High RAM | In-memory DBs, Redis, SAP | `r5`, `x1` |
| **Storage Optimized** | High disk I/O | Data warehouses, Kafka | `i3`, `d2` |
| **Accelerated Compute** | GPU/FPGA | ML training, video rendering | `p3`, `g4` |

> **Exam tip:** Know which family letter maps to which type — `t`/`m` = general, `c` = compute, `r` = memory, `i` = storage, `p`/`g` = GPU.

---

## Regions & Availability Zones

| Concept | What it is |
|---------|-----------|
| **Region** | Geographic location (e.g. `us-east-1`, `ap-south-1` Mumbai) |
| **Availability Zone (AZ)** | Isolated data center within a region (e.g. `us-east-1a`, `1b`, `1c`) |

```
Region (ap-south-1 — Mumbai)
├── AZ: ap-south-1a
├── AZ: ap-south-1b
└── AZ: ap-south-1c
```

- Deploy across multiple AZs for **high availability**
- If one AZ goes down, others keep running
- Each region has **minimum 2 AZs**, most have 3+

> **Exam tip:** EC2 instances live in a specific AZ. For fault tolerance, always deploy in **multiple AZs** behind a Load Balancer.

---

## EC2 Purchasing Options

| Option | How it works | Save vs On-Demand | Use when |
|--------|-------------|------------------|---------|
| **On-Demand** | Pay per hour/second, no commitment | — | Unpredictable workloads |
| **Reserved** | 1 or 3 year commitment | Up to 72% | Steady, predictable workloads |
| **Spot** | Bid for unused capacity | Up to 90% | Fault-tolerant, flexible jobs |
| **Savings Plans** | Flexible commitment ($/hour) | Up to 66% | Mix of instance types |
| **Dedicated Host** | Physical server reserved for you | — | Compliance, licensing requirements |

> **Exam tip:** Spot instances can be **terminated by AWS with 2-minute warning** — never use for critical workloads.

---

## Key Terms

| Term | Meaning |
|------|---------|
| AMI | OS image used to launch an instance (Ubuntu, Amazon Linux) |
| Instance type | CPU + RAM configuration (e.g. `t3.micro`) |
| EBS | Elastic Block Store — persistent disk attached to EC2 |
| Security Group | Virtual firewall — controls inbound/outbound traffic |
| Key pair | SSH credentials to access your EC2 instance |
| Elastic IP | Static public IP that persists across instance restarts |
