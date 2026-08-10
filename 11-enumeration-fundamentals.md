# Chapter 11 — Enumeration Fundamentals

## What Is Enumeration?

**Key points**
- Enumeration = **active information gathering** — one step deeper than scanning.
- Extracts detailed data about a system: usernames, group names, network resources, shares, and system banners.
- Usually follows the scanning phase (Module 03) — scanning tells you *what's* open, enumeration tells you *who/what* is actually behind it.
- Used by both attackers and ethical hackers/pentesters — the technique is identical, only authorization and intent differ.

**Why it's important**
- Helps identify system weaknesses that scanning alone can't reveal.
- Gathers info like: usernames, group memberships, shared resources, and configuration details — exactly what's needed to plan credential attacks (password spraying, brute force) or find misconfigured shares.

**How it works:** Instead of just probing for open ports, enumeration tools actively query the *services themselves* (SMB, LDAP, SNMP, DNS, RPC) using the protocol's own legitimate query features — many of these protocols were designed to share information within a trusted network, so enumeration abuses that trust when access controls are weak or default.

---

## What You'll Cover in This Module

| Protocol/Service | What It Leaks |
|---|---|
| NetBIOS | Machine names, IP addresses, usernames |
| SNMP | Connected devices, sensitive configs, sometimes credentials |
| SMB | Shares, services, usernames |
| Active Directory | Usernames, group memberships, domain structure |
| DNS (Zone Transfer) | Every DNS record: subdomains, mail servers |
| LDAP | User/group directory data, permissions |
| NFS & RPC | Shared files, remote procedure services |
| BGP | Internet routing information (used differently — network-level, not host-level) |
| OS Fingerprinting | Target's exact operating system |

**Tools used throughout this module:** `enum4linux`, `ldapsearch`, `rpcclient`, `snmpwalk`, `smbclient`, Nmap NSE scripts

---

## Mini Guide: The Enumeration Mindset
1. You already know a port is open (from Module 03 scanning) — e.g. 445 (SMB), 389 (LDAP), 161 (SNMP).
2. Pick the enumeration tool matching that service.
3. Query it for what it's willing to share by design (share lists, user lists, device configs).
4. Cross-reference findings (usernames + share names + versions) to plan the next phase: **Step 3 — Gaining Access.**
