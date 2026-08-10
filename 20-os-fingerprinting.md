# Chapter 20 — OS Fingerprinting Techniques

## What Is OS Fingerprinting?

**Key points**
- OS Fingerprinting is the technique used to determine the operating system of a target system based on its responses to different network requests.
- Complements enumeration by narrowing down which exploits, default paths, and known misconfigurations are even relevant.
- Two flavors: **active** (sends probes directly) and **passive** (observes existing traffic without sending anything).

**Analogy:** OS Fingerprinting is like a detective using clues (responses) to figure out what kind of car (OS) someone is driving — without ever seeing the car itself.

---

## How Attackers Use It

**Key points**
- Once the OS is known, an attacker can tailor attacks to exploit OS-specific vulnerabilities (e.g. a Windows-only exploit vs. a Linux kernel bug).
- Also useful for planning payload compatibility (Windows vs. Linux binaries) ahead of the "Gaining Access" phase.

**Tools:** `Nmap` (`-O`), `xprobe2`, `p0f` (passive fingerprinting)

**How it works:** Active tools like Nmap's `-O` send a series of crafted TCP/ICMP probes and compare subtle response quirks (TTL values, TCP window sizes, option ordering) against a large fingerprint database. Passive tools like `p0f` just listen to existing traffic and infer the OS from natural packet characteristics — completely undetectable since nothing is sent.

**Mini example:**
```bash
nmap -O <target-ip>              # Active OS detection
sudo p0f -i eth0                 # Passive OS fingerprinting from live traffic
```

## Defensive Note
OS fingerprinting can't be fully prevented (TCP/IP stack behavior is inherent to the OS), but normalizing TTL values and using a properly configured firewall/IPS to drop malformed or unusual probe packets can reduce reliability of active fingerprinting attempts.
