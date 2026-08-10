# Chapter 2 — Discovering Live Hosts (Ping Sweep, ARP Scan)

## What Is Host Discovery?

**Key points**
- Before hacking, a hacker must ask: *"Is anyone alive in this network jungle?"*
- Host discovery = finding which devices are online and reachable.
- It's the **first step of scanning**, done before port scanning.

**Analogy:** Imagine shouting "Oye!" into a dark colony — whichever direction echoes back, someone's alive there.

---

## 1. Ping Sweep

**Key points**
- Sends ICMP Echo Requests to every IP in a range and listens for replies.
- Simple, fast, but easily blocked — many firewalls/hosts disable ICMP responses.
- Goal: find active IPs and devices, identify reachable systems, gather basic network info.

**Tools:** `nmap -sn`, `fping`, Angry IP Scanner

**How it works:** Each live host that hasn't blocked ICMP replies with an Echo Reply (Type 0), confirming it's online — without touching any specific port or service.

**Mini example:**
```bash
nmap -sn 192.168.1.0/24     # Ping sweep an entire subnet
fping -a -g 192.168.1.0/24  # Fast alternative ping sweep
```

---

## 2. ARP Scan

**Key points**
- ARP = Address Resolution Protocol — used inside a LAN to map IP addresses to MAC addresses.
- **Works even when ICMP is blocked** — ARP operates at Layer 2, below IP-level firewall rules.
- Gives very high accuracy for local network discovery since every device must respond to ARP to communicate at all.

**Tools:** `arp-scan`, `netdiscover`

**How it works:** ARP scan broadcasts "who has this IP?" to the whole local subnet; every live device on that segment must reply with its MAC address to function on the network, so replies reveal exactly who's present.

**Mini example:**
```bash
arp-scan 192.168.1.0/24
sudo netdiscover -r 192.168.1.0/24
```

---

## Homework / Practice
```bash
nmap -sn <your-subnet>            # Ping sweep your own network
sudo netdiscover -r <your-subnet> # ARP scan your own network
```
- Sniff ICMP Echo Requests in Wireshark while a ping sweep runs.
- Compare `arp-scan` results vs `ping-sweep` results on the same subnet — ARP usually finds more devices.

## Output of this chapter
A list of **live IPs** on the target network — this feeds directly into **Chapter 3: Port Scanning Techniques.**
