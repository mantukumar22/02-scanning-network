# Chapter 5 — Nmap Mastery (Basic to Advanced)

> "Nmap is the Swiss Army Knife for hackers." — every hacker ever.

## What Is Nmap?

**Key points**
- Nmap (Network Mapper) is a free, open-source tool to: discover live hosts, scan ports, detect services, fingerprint OS, and find vulnerabilities (via scripts).
- It's usually a hacker's (and defender's) very first tool in any engagement.

**Tools:** `nmap` (CLI), Zenmap (its GUI — see Chapter 6)

---

## Command Reference

### Host & Port Discovery
| Command | Purpose |
|---|---|
| `nmap <IP>` | Default scan (top 1000 ports) |
| `nmap -sP <subnet>` / `nmap -sn <subnet>` | Ping sweep — find live hosts only |
| `nmap -F <IP>` | Fast scan (100 most common ports) |
| `nmap -p 80 <IP>` | Scan one specific port |
| `nmap -p 1-1000 <IP>` | Scan a port range |
| `nmap -p- <IP>` | Scan all 65,535 ports |

### Service & Version Detection
| Command | Purpose |
|---|---|
| `nmap -sV <IP>` | Detect running services + version numbers |
| `nmap -O <IP>` | OS detection / fingerprinting |

### Scan Types (Stealth & Aggressive)
| Scan Type | Command | Usage |
|---|---|---|
| TCP SYN (Stealth) | `nmap -sS <IP>` | Undetected by many firewalls |
| TCP Connect | `nmap -sT <IP>` | Full connection scan (no raw socket needed) |
| Aggressive | `nmap -A <IP>` | Full detection: OS, version, scripts, traceroute |
| OS Detection | `nmap -O <IP>` | Detect the target's operating system |

### The "All-in-One Recon Punch"
```bash
nmap -sS -sV -O -A <target-ip>
```
Combines stealth scanning, version detection, OS fingerprinting, and aggressive scripts into a single comprehensive scan.

---

## Real-Life Scenario: Wi-Fi Network Recon

You're on college or café Wi-Fi and curious what's around you:
```bash
ifconfig                          # Get your own IP
nmap -sn 192.168.1.0/24           # Who's online?
nmap -sV 192.168.1.10             # Deep dive on one target
```
Next step: check any open ports for FTP/SMB/HTTP and search for known exploits on the detected versions.

---

## Bonus Hacker Tips

> "Never scan blindly on the internet. Always test on your lab or target systems with permission."

- Use `--top-ports 1000` for a wide but efficient port scan.
- Use `-T4` for faster scan timing (use `-T2` to be stealthier/slower).
- Use `-oN <filename>` to save output to a file for later reference/reporting.
- Use `-v` or `-vv` for verbose output while a scan runs.
