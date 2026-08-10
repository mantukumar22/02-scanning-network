# Chapter 15 — Active Directory Enumeration

## What Is Active Directory?

**Key points**
- Active Directory (AD) is Microsoft's directory service for storing information about network resources: users, computers, printers, and their relationships.
- The backbone of most corporate Windows networks — one AD compromise often means the entire domain is at risk.
- Enumeration here is usually the **first real step** of a Windows-domain-focused engagement.

**Analogy:** Think of AD as a giant phone book. When you enumerate it, you're flipping through pages, looking for that one contact you want to interact with.

---

## How Attackers Use It

**Key points**
- Enumerating AD reveals usernames, group memberships, and more — which fuels further attacks like password spraying (trying one common password against many accounts to avoid lockouts).
- Understanding the domain structure also reveals privilege paths — which users belong to Domain Admins, etc.

**Tools:** `enum4linux`, `rpcclient`, BloodHound (for attack-path graphing), `ldapsearch`

**How it works:** `enum4linux` can dump all kinds of useful info from a Windows server via its SMB/RPC interfaces — user lists, group lists, password policy, and more, often even with a low-privilege or null session.

**Mini example:**
```bash
enum4linux -U <target-ip>      # Enumerate users specifically
rpcclient -U "" -N <target-ip> # Null-session RPC connection
> enumdomusers                 # Inside rpcclient: list domain users
> enumdomgroups                # List domain groups
```

## The Active Directory Attack Chain (for context)

| Step | Objective |
|---|---|
| 1. Domain Enumeration | Finding as much information about the target as possible |
| 2. Privilege Escalation | Elevating privilege to carry out privileged tasks / access privileged resources |
| 3. Persistence | Maintaining the elevated privilege in the environment as long as possible |
| 4. Data Exfiltration / DoS / etc. | Carrying out the malicious action that was the initial target of the attack |

This module covers **Step 1** only — enumeration. Steps 2–4 belong to later phases (Gaining Access, Maintaining Access) of the overall pentest lifecycle.

## Defensive Note
Restrict null-session/anonymous RPC access (a common Windows misconfiguration), enforce least-privilege group membership reviews, and monitor for abnormal LDAP/RPC query volume — a sudden burst of `enumdomusers`-style queries is a strong enumeration indicator.
