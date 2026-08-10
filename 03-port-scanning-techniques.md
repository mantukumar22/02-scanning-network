# Chapter 3 — Port Scanning Techniques (SYN, ACK, XMAS, NULL)

## What Is Port Scanning?

**Key points**
- Port scanning discovers open, closed, or filtered ports on a target and the services behind them.
- **Analogy:** walking through a hotel corridor checking which doors are unlocked — you're not entering, just gently checking the handle.
- Common goals: discover open ports (21-FTP, 22-SSH, 80-HTTP), identify services (Apache, MySQL), detect firewall/filtering rules.

**Tools:** Nmap, Netcat, Masscan

**Real story:** During a pentest, a hacker port-scanned an office printer and found an unauthenticated web interface — printed "This printer has been hacked" on every department's printer. The office thought it was a ghost 👻 — a reminder that overlooked devices (printers, IoT, cameras) are often the weakest link.

---

## 1. SYN Scan (Half-Open Scan)

**Key points**
- Sends a SYN packet; if the port is open, the server replies SYN+ACK — the scanner then sends RST instead of completing the handshake ("half-open").
- Because the connection never fully completes, it's **less likely to be logged** by many applications and older firewalls.
- Requires raw socket privileges (root/admin) to craft the packet.

**Analogy:** Knocking on someone's door (SYN), they peek through the window (SYN-ACK), and you disappear (no ACK). They get suspicious but never know who it was.

**Tools:** `nmap -sS`

**How it works:**
```
Open port:   Hacker → SYN → Server
             Server → SYN+ACK → Hacker
             Hacker → RST → Server   (connection never completes)

Closed port: Hacker → SYN → Server
             Server → RST → Hacker   (immediate rejection)
```

**Mini example:**
```bash
nmap -sS <target-ip>
```

---

## 2. ACK Scan

**Key points**
- Doesn't determine open/closed — instead it's used to **map firewall rulesets**.
- Sends an ACK packet (no prior SYN) and reads how the target responds.
- Often run **before** a full scan to understand what a firewall is doing.

**Status → Response table:**

| Port Status | Response |
|---|---|
| Unfiltered | RST |
| Filtered | No reply or ICMP error |

**Tools:** `nmap -sA`

**How it works:** A **stateful firewall** recognizes the ACK doesn't belong to an established connection and drops it silently (no response). With **no firewall** present, the host just replies RST regardless of port state — telling the attacker the port is at least reachable (unfiltered), even though it says nothing about open/closed.

**Mini example:**
```bash
nmap -sA <target-ip>
```

---

## 3. XMAS Scan

**Key points**
- Sets multiple TCP flags at once: **FIN + PSH + URG** — like "decorating a Christmas tree" with flags.
- Relies on a quirk in the TCP RFC: closed ports must reply RST, but open/filtered ports often stay silent.
- Effective against older/Unix-like TCP stacks; **modern Windows systems ignore this rule** and won't respond predictably.

**Status → Response table:**

| Port Status | Response |
|---|---|
| Closed | RST |
| Open/Filtered | No response |

**Analogy:** Like ringing the doorbell + throwing pebbles + shouting all at once. If there's no reaction, the house might be open and empty.

**Tools:** `nmap -sX`

**Mini example:**
```bash
nmap -sX <target-ip>
```

---

## 4. NULL Scan (for comparison)

**Key points**
- Sends a TCP packet with **no flags set at all**.
- Same RFC quirk as XMAS: closed ports send RST, open/filtered ports stay silent.
- Very stealthy — rarely detected by older IDS/logging systems, but like XMAS, unreliable against modern OSes.

**Tools:** `nmap -sN`

**Mini example:**
```bash
nmap -sN <target-ip>
```

---

## Comparison Table

| Technique | Flags Used | Detects Open? | Stealth Level | Notes |
|---|---|---|---|---|
| SYN | SYN | ✅ Yes | 🔥 High | Half-open scan |
| ACK | ACK | ❌ No | ⚠️ Medium | Firewall rule detection |
| XMAS | FIN, PSH, URG | ✅ Sometimes | 🥷 Stealthy | Confuses detection systems |
| NULL | None | ✅ Sometimes | 👻 Very High | Rarely detected |
