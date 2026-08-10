# Chapter 1 — Scanning Fundamentals

## 1. What Is Network Scanning?

**Key points**
- Network scanning identifies active devices, open ports, running services, and vulnerabilities on a network.
- Helps ethical hackers map the network and understand potential entry points.
- Critical for identifying the attack surface, security audits, and penetration testing.

**What a hacker wants to know (the 3 big questions):**
1. Which ports are open?
2. Which service is running?
3. Any weak point to attack?

**How it works:** Scanning sends crafted packets to a target and interprets the responses (or lack of them) to infer port state, service, and OS — unlike passive recon, the target can see this traffic in its logs.

---

## 2. Types of Scanning

| Type | What it finds | Example tool |
|------|---------------|---------------|
| **Port Scanning** | Open, closed, or filtered ports; services behind them | Nmap, Netcat, Masscan |
| **Network Scanning** | Live hosts/devices on a network, infrastructure map | Nmap (`-sn`), Angry IP Scanner, ARP Scan |
| **Vulnerability Scanning** | Known weaknesses tied to detected services/versions | Nessus, OpenVAS, Nikto |

**Mini example:**
```bash
nmap -sn 192.168.1.0/24     # Network scan: who's alive?
nmap -p 1-1000 192.168.1.10 # Port scan: what's open?
nikto -h http://target.com  # Vulnerability scan: what's weak?
```

---

## 3. TCP/IP Basics (What Scanning Runs On)

**Key points**
- TCP/IP is the protocol stack the entire internet runs on — no internet without it.
- **TCP (Transmission Control Protocol):** reliable, connection-oriented; packets get confirmed. Abused in attacks like TCP SYN Flood.
- **IP (Internet Protocol):** delivers each packet to its destination. IP spoofing is an attacker favorite.
- **ICMP:** the "doorbell" protocol — used for `ping` (is host alive?) and `traceroute` (which path did the packet take?).

**ICMP types to know:**
- Type 8 = Echo Request (the "knock")
- Type 0 = Echo Reply (the "answer")

**Mini example:**
```bash
ping target.com          # ICMP Echo Request → checks if host is alive
traceroute target.com    # Maps the path packets take to reach target
```

**Real trick:** Many IoT devices don't block ICMP — a hacker pings a whole IP range, finds a device (e.g. Smart TV) that responds, checks its open ports, and sometimes finds an admin panel with no password.

---

## 4. IP Addressing & Subnetting Refresher

**Key points**
- IP = your system's identity card on the network. Format (IPv4): `192.168.1.1`
- **Public IP** = visible to the world · **Private IP** = only inside your local network
- Subnetting = dividing a network into smaller "neighborhoods" — tells an attacker network size, how many machines might exist, and which IPs are likely alive.

**Subnet cheat sheet:**
```
/24 = 254 usable hosts
/30 = 2 usable hosts (used in router-to-router links)
```

**Mini example:**
```bash
ifconfig      # Linux: see your IP
ipconfig      # Windows: see your IP
curl ifconfig.me   # See your public IP
ip r          # Shows your subnet, e.g. 192.168.0.0/24
```

**Example attack scenario:** A hacker scans a company WiFi's `/24` range and finds `.1` is the router, `.10` is a printer, `.45` is a CCTV camera. Next step: try default credentials or exposed login portals on each.

**Defensive takeaways:**

| Element | Hacker Trick | Defense Strategy |
|---------|-------------|-------------------|
| IP | Spoofing, recon | Use VPNs, firewalls |
| Subnet | Scan smaller ranges | Split network, isolate VLANs |
| ICMP | Ping sweep | Disable ICMP externally |
