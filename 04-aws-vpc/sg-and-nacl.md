# AWS VPC — Security Groups & NACLs

---

## Security Layers in a VPC

AWS uses a **shared responsibility model** — AWS secures the infrastructure, you secure what's inside it.

As a DevOps/AWS Admin, you add security at multiple levels:

```
Internet
    ↓
Internet Gateway
    ↓
NACL (subnet-level firewall)        ← your first security layer
    ↓
Security Group (instance-level)     ← your second security layer
    ↓
EC2 / App
```

VPC → SG → NACL = last point of security before traffic hits your instance.

---

## Security Groups (SG)

- Applied at the **EC2 instance level**
- Controls **inbound and outbound traffic** for that instance
- **Stateful** — if inbound is allowed, the response is automatically allowed outbound

### AWS Default SG Behaviour

| Traffic | Default |
|---------|---------|
| Inbound | **All denied** |
| Outbound | **All allowed** (except port 25 — mailing service, to prevent spam) |

### Practical Example

EC2 running Jenkins — not reachable from internet by default.

```
EC2 (Jenkins) → no SG rule → NOT reachable ❌

EC2 (Jenkins) → SG allows port 8080 → accessible over internet ✅
```

You add an inbound rule: allow TCP port 8080 → Jenkins is now reachable.

### SG + NACL Together

```
User → [NACL check] → [SG check] → App (EC2)
```

Both must allow traffic for it to reach the instance.

---

## NACLs — Network Access Control List

- Applied at the **subnet level**
- Controls traffic **entering and leaving the subnet**
- **Stateless** — inbound and outbound rules are evaluated independently

### Why NACL on top of SG?

**Scenario:**
- Dev Team 1 has 50 EC2 instances in a private subnet
- Port 8080 is exposed on each EC2 (SG allows all inbound)
- Anyone in the world can access these instances

**Problem:** You want to block certain IPs or automate security at scale — you can't update 50 SGs individually.

**Solution → NACL at subnet level:**

```
Private Subnet (50 EC2s, all allow port 8080 via SG)
    ↑
  NACL → deny incoming traffic from specific IP/range
    ↑
Even if EC2's SG allows it → NACL blocks it first
→ EC2 is NOT accessible ✅
```

NACL acts as another security layer that **controls access from the world** before traffic even reaches your instances.

### NACL Use Cases
- Block a specific IP range (e.g. after detecting an attack)
- Automate security — deny/allow by subnet, not instance-by-instance
- Add an extra layer even when SG is misconfigured

---

## SG vs NACL — Key Differences

| | Security Group | NACL |
|-|---------------|------|
| Applied at | EC2 instance level | Subnet level |
| Stateful | ✅ Yes | ❌ No (stateless) |
| Default inbound | Deny all | Allow all |
| Default outbound | Allow all (except port 25) | Allow all |
| Rules | Allow only | Allow + Deny |
| Use for | Per-instance access control | Subnet-wide access control |

> **Exam tip:** SG = stateful (instance), NACL = stateless (subnet). This distinction is tested constantly.

---

## Key Terms

| Term | Meaning |
|------|---------|
| Security Group | Instance-level stateful firewall |
| NACL | Subnet-level stateless firewall |
| Stateful | Response traffic automatically allowed |
| Stateless | Inbound and outbound rules evaluated separately |
| Inbound rule | Controls traffic coming INTO the instance/subnet |
| Outbound rule | Controls traffic going OUT of the instance/subnet |
| Port 25 | SMTP — blocked by default in AWS outbound to prevent spam |
