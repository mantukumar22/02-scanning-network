# Chapter 7 — Firewall Evasion Techniques

## Why Evasion Matters

**Key points**
- Firewalls are like the bouncers at a club — they don't let suspicious traffic in or out.
- Goal of evasion: trick the firewall so a scan or payload sneaks past without triggering alarms.
- For hackers, evasion enables: bypassing detection, scanning stealthily, reaching hidden services, and gaining access in restricted environments.

**Analogy:** Sneaking into a movie theatre without buying a ticket — except as an *ethical* hacker, you're testing whether the theatre's security actually catches you, with permission to do so.

---

## Top Firewall Evasion Techniques

| Technique | Description |
|---|---|
| **Fragmentation** | Break a packet into small pieces to confuse the firewall's inspection |
| **Timing** | Send packets slowly ("low and slow") to avoid triggering rate-based alerts |
| **Decoy Scans** | Add fake source IPs alongside the real one to hide which is the actual attacker |
| **Spoofed IP** | Send packets pretending to be from another IP entirely |
| **Custom Packets** | Use Nmap/Nikto with unusual flag combinations to slip past signature-based rules |
| **Source Port Manipulation** | Use commonly-allowed ports (53-DNS, 443-HTTPS) as the scan's source port |

---

## Practical Commands

### Fragmented Scan
```bash
nmap -f scanme.nmap.org
```
Splits probe packets into smaller fragments, which some older firewalls/IDS fail to reassemble and inspect correctly.

### Decoy Scan
```bash
nmap -D 192.168.1.1,192.168.1.2,ME scanme.nmap.org
```
Sends scan packets that appear to originate from multiple decoy IPs plus your own (`ME`), making it harder for a defender to identify the real attacker in logs.

### Source Port Bypass
```bash
nmap --source-port 443 scanme.nmap.org
```
Many firewalls trust traffic from "well-known" ports like 443 (HTTPS) or 53 (DNS) — using one as the scan's source port can let probes through rules that only inspect destination ports.

---

## Defensive Counter-Notes
- Fragmentation reassembly should be enabled on modern firewalls/IDS — most current systems already handle this correctly, which is why `-f` is less effective against up-to-date defenses.
- Decoys don't hide you from a defender who checks *all* source IPs against known-good lists — they add noise, not invisibility.
- Source-port trust rules are a misconfiguration, not a firewall feature — proper stateful firewalls validate the full connection, not just the source port number.
- Practice all of the above only against `scanme.nmap.org` (Nmap's official public test target) or your own lab systems.
