# Chapter 13 — SNMP Enumeration

## What Is SNMP?

**Key points**
- Simple Network Management Protocol (SNMP) is used for managing and monitoring network devices — routers, switches, servers, printers.
- Works in a client-server model over UDP port 161 (queries) and 162 (traps).
- Authenticated by a "community string" — historically often left at the default `public` (read) or `private` (write).

**Analogy:** SNMP is like sneaking into an office and finding the secretary's list of everyone's phone numbers without them knowing.

---

## How Attackers Use It

**Key points**
- An attacker can query SNMP for a list of connected devices and sensitive info like usernames, running processes, and even hardware configurations.
- Because many devices still ship with default community strings, this is often a **zero-authentication** information leak.
- SNMP v1/v2c send the community string in plaintext — trivially sniffable on the wire too.

**Tools:** `snmpwalk`, `onesixtyone` (community string brute-forcer), `snmp-check`

**How it works:** `snmpwalk` is like the hacker's version of a metal detector when looking for data in SNMP — it walks the entire MIB (Management Information Base) tree that the device exposes and dumps every value it can read with the given community string.

**Mini example:**
```bash
snmpwalk -v2c -c public <target-ip>              # Walk the whole MIB tree
onesixtyone -c community.txt <target-ip>          # Brute-force community strings
snmp-check <target-ip> -c public                  # Cleaner formatted output
```

## Defensive Note
Change default community strings immediately, prefer SNMPv3 (which adds authentication + encryption), and restrict SNMP access to management VLANs only — never expose port 161 to the internet.
