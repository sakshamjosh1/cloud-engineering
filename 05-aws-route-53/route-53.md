# AWS Route 53 — DNS Service

---

## What is Route 53?

Route 53 is AWS's **DNS (Domain Name System)** service.

DNS translates domain names → IP addresses.

```
amazon.com  →  3.6.10.171
flipkart.com →  (some IP)
```

**Why not just give users the IP directly?**
1. IPs are hard to remember
2. IPs can change anytime (e.g. Load Balancer IPs change)

→ Solution: always use a **domain name**, Route 53 handles the translation.

---

## Why Route 53 in a VPC context?

```
User
 ↓
Cannot give user the IP of the Load Balancer directly
 ↓
Use domain name → Route 53 resolves it → LB IP
 ↓
IGW → ELB (public subnet) → EC2s (private subnet)
```

Even if the LB's IP changes, the domain name stays the same — Route 53 always points to the current IP via DNS records.

---

## How Route 53 Works

```
App deployed on AWS
 ↓
Gets an IP address
 ↓
AWS DNS → DNS Records created
 ↓
Any request on the domain → goes to Route 53
 ↓
Route 53 checks DNS records → returns correct IP
```

---

## Domain Registration

You can register a domain:
- **On AWS** via Route 53 (e.g. `saksham.in`)
- **Outside AWS** via GoDaddy, Hostinger, etc. → then point DNS to Route 53

Either way, once registered → create a **Hosted Zone** on AWS → add DNS records there.

---

## Hosted Zones

A Hosted Zone is a **container for DNS records** for a domain.

| Type | Use |
|------|-----|
| **Public Hosted Zone** | Resolves domain for internet traffic |
| **Private Hosted Zone** | Resolves domain within a VPC only (internal services) |

---

## DNS Record Types (exam essential)

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Domain → IPv4 address | `api.saksham.in` → `3.6.10.171` |
| **AAAA** | Domain → IPv6 address | `api.saksham.in` → IPv6 |
| **CNAME** | Domain → another domain | `www.saksham.in` → `saksham.in` |
| **Alias** | AWS-specific — domain → AWS resource | `saksham.in` → ELB DNS name |
| **MX** | Mail server records | Email routing |
| **TXT** | Verification / SPF records | Domain ownership proof |

> **Exam tip:** Use **Alias records** (not CNAME) to point a root domain (`saksham.in`) to an AWS resource like ELB, CloudFront, or S3. CNAME can't be used on root domains.

---

## Health Checks

Route 53 performs **health checks** on your endpoints across AZs.

- Monitors if your app/server is responding
- If unhealthy → Route 53 stops routing traffic to that endpoint
- Used with **routing policies** for failover

---

## Routing Policies (exam essential)

| Policy | Behaviour | Use case |
|--------|-----------|---------|
| **Simple** | Single endpoint | Basic setup |
| **Weighted** | Split traffic by % | A/B testing, canary deployments |
| **Latency** | Route to lowest-latency region | Global apps |
| **Failover** | Primary → Secondary if primary unhealthy | Disaster recovery |
| **Geolocation** | Route based on user's country/region | Serve region-specific content |
| **Multivalue** | Returns multiple IPs, health-checked | Simple load balancing |

---

## Key Terms

| Term | Meaning |
|------|---------|
| DNS | Translates domain names to IP addresses |
| Hosted Zone | Container for DNS records on Route 53 |
| A Record | Maps domain to IPv4 |
| CNAME | Maps domain to another domain |
| Alias Record | AWS-specific — maps to AWS resource (ELB, CloudFront) |
| Health Check | Route 53 monitors endpoint availability |
| TTL | Time To Live — how long DNS response is cached |
