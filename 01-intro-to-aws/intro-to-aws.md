# Day 1 — Intro to AWS & Cloud Computing

---

## What is Cloud?

Traditionally, companies needed physical servers to run applications.

**Problems with physical servers:**
- Had to get 15–20 racks of servers ready per application
- Teams needed to: create datacenters, procure servers, set up environments
- Huge complexity: costly, hard to configure, requires IT/ops/HR teams
- Need to manage them 24/7

**Solution → Virtualization → Cloud**

Using virtualization concepts (VMs), companies could now use resources more efficiently on a single server — this became the foundation of cloud.

---

## Why AWS?

AWS solved the physical server problem by:
- Offering virtual servers on demand
- No need to own or manage physical infrastructure
- Pay only for what you use

**AWS Advantages:**
- Fast provisioning (minutes vs weeks)
- Huge market share — most widely adopted cloud platform
- Large ecosystem of services

> AWS is the **#1 cloud provider** by market share — most DevOps jobs require AWS knowledge.

---

## Public vs Private Cloud

| | Public Cloud | Private Cloud |
|-|-------------|--------------|
| Who owns infra | Cloud provider (AWS, Azure) | The company itself |
| Examples | AWS, Azure, GCP | On-prem data centers |
| Cost | Pay-as-you-go | High upfront capital cost |
| Maintenance | Provider handles it | Company handles it |
| Control | Less control | Full control |

---

## Cloud Deployment Models

| Model | Description | Example |
|-------|-------------|---------|
| **Public Cloud** | Infra owned by provider, shared across customers | AWS, Azure, GCP |
| **Private Cloud** | Infra owned and managed by the company | On-prem data center |
| **Hybrid Cloud** | Mix of public + private | AWS + company DC |

---

## Why People Are Moving to Public Cloud

- No maintenance overhead
- Scale up/down instantly
- No need for dedicated IT/ops teams for infra
- Cost-efficient for startups — no upfront investment

**Especially for small startups:**
- Can't afford to buy servers and manage them
- Public cloud lets them focus on product, not infrastructure
- This is why public cloud **keeps growing**

---

## Why Some Companies Move Back to Private Cloud

Despite public cloud benefits, some companies move workloads back to private/on-prem:

### 1. Security & Compliance
- Sensitive data (banking, healthcare, government) must stay within controlled environments
- Regulatory requirements (GDPR, HIPAA) may restrict where data can be stored
- Public cloud = shared infrastructure → security concerns

### 2. Cost at Scale
- At large scale, public cloud bills get very expensive
- Companies with predictable, stable workloads find it cheaper to own hardware
- Example: Companies moving from AWS back to their own DCs to save millions

### 3. Data Sovereignty
- Some countries require data to stay within national borders
- Private cloud or local DCs are the only compliant option

### 4. Vendor Lock-in
- Over-dependence on one cloud provider is risky
- Moving back gives more control and flexibility

---

## The Reality — Public Cloud Is Here to Stay

Despite the "moving back" trend:
- **Most companies still use and grow on public cloud**
- Hybrid cloud is the dominant enterprise model
- Public cloud is still the default for startups and modern teams

The debate is not **public vs private** — it's about **which workloads go where**.

---

## AWS, Azure, GCP — Public Cloud Layer

All major cloud providers follow the same model:

```
Physical Data Centers (owned by AWS/Azure/GCP)
        ↓
Virtualization Layer (Hypervisor)
        ↓
Virtual Servers → rented to users as cloud services
```

They're all renting you **virtual machines** on their physical infra — what differs is:
- Services offered
- Pricing
- Geographic regions
- Ecosystem and tooling

---

## Key Terms

| Term | Meaning |
|------|---------|
| Public Cloud | Cloud infra owned by provider, available to anyone |
| Private Cloud | Infra owned and operated by the company itself |
| Hybrid Cloud | Combination of public + private cloud |
| On-premises (on-prem) | Company's own physical servers in their own data center |
| Data Sovereignty | Legal requirement for data to stay within a country/region |
| Vendor Lock-in | Over-dependence on a single cloud provider |
