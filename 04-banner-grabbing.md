# Chapter 4 — Banner Grabbing & Service/Version Detection

## What Is Banner Grabbing?

**Key points**
- "Hello, Server! Who are you?" — that's what banner grabbing is all about.
- A technique to gather information about a service running on an open port.
- Hacker's goal: extract software version, OS type, service name (Apache, Nginx, OpenSSH), and vulnerability indicators — all from the service's own self-announcement.

**Why:** Once you know exact software + version, you can search for **known, exploitable vulnerabilities** tied to that specific build.

---

## Types of Banner Grabbing

| Type | How | Tools | Trade-off |
|------|-----|-------|-----------|
| **Passive** | No direct interaction — sniffs traffic between the service and other clients | Wireshark, tcpdump | Safe, stealthy, slow |
| **Active** | Sends requests directly to the service and reads its response | telnet, nc, nmap, curl | Fast but detectable |

---

## Practical Tools & Commands

### 1. Using Telnet (Active)
```bash
telnet <IP> 80
GET / HTTP/1.1
Host: <IP>
```
**Output:**
```
Server: Apache/2.4.41 (Ubuntu)
```

### 2. Using Nmap (Active, automated)
```bash
nmap -sV <IP>
```
**Output:**
```
22/tcp open  ssh    OpenSSH 7.6p1 Ubuntu
80/tcp open  http   Apache httpd 2.4.29 ((Ubuntu))
```

**Hacker's tip:** Copy the exact version string → paste into Google (or an exploit database) with the word "exploit" to instantly find known CVEs.

---

## How Hackers Use This Info

| Service | Version | Vulnerability Example |
|---|---|---|
| Apache 2.4.29 | Known vuln | CVE-2019-0211 |
| OpenSSH 7.2p2 | Known vuln | User Enumeration flaw |
| FTP vsftpd 2.3.4 | Evil backdoor | Metasploit-ready exploit |

Hackers search for exploits based on the exact version, then load matching payloads in Metasploit, Searchsploit, or GitHub.

---

## How to Protect Against Banner Grabbing

- **Disable server banners:**
  - Apache → `ServerTokens Prod` & `ServerSignature Off`
  - Nginx → `server_tokens off;`
- Use firewalls to restrict which IPs can even reach the service.
- Deploy Intrusion Detection Systems (IDS) to flag repeated probing.
- Avoid running outdated/unpatched services — the version itself is the risk, not just its visibility.
