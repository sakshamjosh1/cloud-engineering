# AWS IAM — Identity and Access Management

---

## What is IAM?

IAM is how AWS controls **who can access what** inside your AWS account.

Every action in AWS — launching EC2, reading S3, deleting a database — goes through IAM first.

IAM handles two things:

| Concept | Question it answers | Example |
|---------|-------------------|---------|
| **Authentication** | Can you enter? | Is this a valid AWS user? |
| **Authorization** | What can you do? | Can this user delete EC2 instances? |

### Bank Analogy (from your notes)
```
Enter the bank         →  Authentication  (can you enter?)
Employee desk / Service desk  →  Authorization   (what can you do here?)
```
- An **employee** can access the vault
- A **customer** can only access the service desk
- Same building, different permissions

---

## Core IAM Components

| Component | What it is |
|-----------|-----------|
| **Users** | Individual identities (a person, a service account) |
| **Groups** | Collection of users — attach policies to a group, all users inherit them |
| **Policies** | JSON documents that define what actions are allowed or denied |
| **Roles** | Temporary permissions — for AWS services or cross-account access |

---

## Users

- Represents a **person or application** that needs AWS access
- Created with authentication credentials (password or access keys)
- Policies are attached to define what they can do

**DevOps use case:**
> A new employee joins — employee #5001. You don't give them full access.
> You ask: what do they need?
> - Read access to DB ✅
> - No access to K8s ❌
>
> You create a user and attach only the required policies → **least privilege principle**

---

## Groups

- A group is a **collection of users**
- Instead of attaching policies to each user individually, attach once to the group
- Users inherit all permissions of the group

```
Groups:
├── Dev   → permissions for dev tools, EC2, S3
├── QA    → permissions for testing environments
└── DB    → permissions for RDS, DynamoDB
```

**Why groups matter:**
- New developer joins → add them to the `Dev` group → done
- Easy, fast, consistent
- Remove from group when they leave → access revoked instantly

---

## Policies

- A **JSON document** that defines allowed or denied actions
- Attached to users, groups, or roles

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

**Two types:**
| Type | Description |
|------|-------------|
| **AWS Managed** | Pre-built by AWS (e.g. `AmazonS3ReadOnlyAccess`) |
| **Customer Managed** | Custom policies you write for your specific needs |

---

## Roles

Roles are for **AWS service-to-service communication** — not for humans.

### The Problem
> A developer builds an application running on EC2.
> The app needs to read data from a DB service (RDS).
> You **can't create a regular user** for this particular app.
> Hardcoding credentials in code is a security risk.

### The Solution → IAM Role

```
Developer → Application (on EC2 / on-premises / private cloud)
                ↓
        needs data from DB Service (AWS)
                ↓
        attach an IAM Role to EC2
                ↓
        EC2 gets TEMPORARY credentials automatically
                ↓
        EC2 ↔ DB Service communication works securely
```

**Key properties of Roles:**
- **Temporary** — credentials are short-lived and auto-rotated
- **No hardcoded secrets** — AWS handles credential delivery
- **AWS-to-AWS** — designed for service-to-service communication
- Can also be used for cross-account access

**Common examples:**
| Scenario | Role used |
|----------|-----------|
| EC2 reading from S3 | Attach S3 read role to EC2 |
| Lambda writing to DynamoDB | Attach DynamoDB write role to Lambda |
| GitHub Actions deploying to AWS | OIDC role assumed by GitHub |

---

## The Big Picture — How IAM Fits Together

```
DevOps Engineer (you)
        ↓
  creates AWS Account
        ↓
  via IAM → creates 1000s of users
        ↓
  root access  ←→  scoped access
```

> As a DevOps engineer, **you use IAM to provide access only to the services that are required** — nothing more.

If someone deletes a service (knowingly or unknowingly), IAM is the first place you check:
- Did they have permission to do that?
- Should they have had that permission?

---

## IAM Best Practices

- **Never use root account** for day-to-day work — create IAM users
- **Least privilege** — give only the permissions actually needed
- **Use groups** for team-level access, not individual policy attachments
- **Use roles** for applications and services — never hardcode credentials
- **Enable MFA** on root and all admin accounts
- **Rotate access keys** regularly

---

## Key Terms

| Term | Meaning |
|------|---------|
| Authentication | Verifying identity — who are you? |
| Authorization | Verifying permissions — what can you do? |
| Least privilege | Give only the minimum permissions required |
| IAM Policy | JSON document defining allowed/denied actions |
| IAM Role | Temporary permissions for AWS services |
| Root account | The master AWS account — should be locked away |
| MFA | Multi-Factor Authentication — extra security layer |
| Access keys | Programmatic credentials for CLI/SDK access |
