# Chapter 12 — NetBIOS Enumeration

## What Is NetBIOS?

**Key points**
- NetBIOS (Network Basic Input/Output System) is a protocol allowing applications on different computers to communicate within a LAN.
- Commonly used in Windows-based environments to share files, printers, and services.
- Operates over ports 137 (name service), 138 (datagram), 139 (session).

**Analogy:** Imagine your target is like a cool guy at a party (the network). NetBIOS enumeration is like asking everyone at the party, "Yo, what's his name? Where does he live?"

---

## How Attackers Use It

**Key points**
- Attackers use NetBIOS enumeration to gather info about a target network: machine name, IP address, usernames, domain/workgroup, and more.
- Older/unhardened systems may respond to these queries without any authentication at all.

**Tools:** `enum4linux`, `nbtstat` (Windows), `nbtscan`

**How it works:** `enum4linux` connects to the NetBIOS/SMB services on a target and gathers information from a Linux box that's talking to (or is) a Windows machine — pulling machine names, workgroup/domain info, and sometimes shares and users in one pass.

**Mini example:**
```bash
enum4linux -a <target-ip>       # Full automated NetBIOS + SMB enumeration
nbtscan 192.168.1.0/24          # Scan a subnet for NetBIOS names
nbtstat -A <target-ip>          # Windows-native NetBIOS query
```

## Defensive Note
Disable NetBIOS over TCP/IP where not needed (Windows: Network adapter → WINS tab → "Disable NetBIOS over TCP/IP"), and block ports 137–139 at the firewall from untrusted networks.
