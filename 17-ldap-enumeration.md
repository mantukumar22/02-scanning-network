# Chapter 17 — LDAP Enumeration

## What Is LDAP?

**Key points**
- Lightweight Directory Access Protocol (LDAP) is used to access and maintain directory services — managing user and group information on a network.
- Runs on TCP port 389 (or 636 for LDAPS, the encrypted version).
- Active Directory (Ch.15) uses LDAP under the hood as one of its core query interfaces.

**Analogy:** LDAP enumeration is like going to a library and pulling out the index card catalog — only it's all user and group data instead of books.

---

## How Attackers Use It

**Key points**
- LDAP can reveal sensitive information about users, groups, and permissions if not properly secured.
- Anonymous LDAP binds (no credentials required) are still found in real-world misconfigured environments.
- Findings here directly complement AD enumeration (Ch.15) — LDAP is often the raw query layer behind those higher-level tools.

**Tools:** `ldapsearch`

**How it works:** `ldapsearch` connects to the LDAP service and performs a search against the directory tree starting from a "base DN" (distinguished name) — with anonymous bind enabled, this returns full user/group entries with no authentication.

**Mini example:**
```bash
ldapsearch -x -h <target-ip> -b "dc=target,dc=com"
```
- `-x` → use simple (unauthenticated) bind
- `-h` → target host
- `-b` → base DN to search from (the "root" of the directory tree to query)

## Defensive Note
Disable anonymous LDAP binds, enforce LDAPS (encrypted) instead of plaintext LDAP, and apply the principle of least privilege to what any given account can query in the directory.
