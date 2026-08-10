# 📡 Scanning & Enumeration — Nmap, Zenmap & Module 04

Chapter-wise notes on network scanning and enumeration, built from course slides (Module 03: Scanning Networks, Module 04: Enumeration), reorganized for quick reference, with extra tools added from global practice.

> ⚠️ **Legal note:** Everything in this repo is **active** and touches the target's systems directly — port scans, service detection, firewall evasion, OS fingerprinting, and every enumeration technique in Module 04 (which additionally extracts real usernames/data). Only run these against systems you own or have **written authorization** to test. Unauthorized scanning/enumeration can be prosecuted under India's IT Act 2000 (Sections 43, 66) and the DPDP Act 2023 where personal data is involved, and equivalent laws elsewhere (CFAA in the US, Computer Misuse Act in the UK). `scanme.nmap.org` is Nmap's own public test target — safe to practice scanning against; use your own lab VMs for enumeration practice.

## 📚 Module — Scanning Networks

| # | File | Topic |
|---|------|-------|
| 1 | [01-scanning-fundamentals.md](01-scanning-fundamentals.md) | What is scanning, types (port/network/vulnerability), TCP/IP basics |
| 2 | [02-host-discovery.md](02-host-discovery.md) | Ping sweep, ARP scan — finding live hosts |
| 3 | [03-port-scanning-techniques.md](03-port-scanning-techniques.md) | SYN, ACK, XMAS, NULL scans — how each works |
| 4 | [04-banner-grabbing.md](04-banner-grabbing.md) | Service & version detection, banner grabbing |
| 5 | [05-nmap-mastery.md](05-nmap-mastery.md) | Nmap command reference — basic to advanced |
| 6 | [06-zenmap-gui.md](06-zenmap-gui.md) | Zenmap — the GUI for Nmap |
| 7 | [07-firewall-evasion.md](07-firewall-evasion.md) | Fragmentation, decoys, spoofing, timing |
| 8 | [08-vulnerability-scanning.md](08-vulnerability-scanning.md) | Nessus, OpenVAS, Nikto |
| 9 | [09-extra-tools-global-india.md](09-extra-tools-global-india.md) | Masscan, RustScan, Shodan/Censys, and India-relevant notes |
| 10 | [10-scanning-checklist.md](10-scanning-checklist.md) | End-to-end scanning workflow checklist |

## 📚 Module — Enumeration

| # | File | Topic |
|---|------|-------|
| 11 | [11-enumeration-fundamentals.md](11-enumeration-fundamentals.md) | What is enumeration, why it matters, the enumeration mindset |
| 12 | [12-netbios-enumeration.md](12-netbios-enumeration.md) | NetBIOS — machine names, users, workgroups |
| 13 | [13-snmp-enumeration.md](13-snmp-enumeration.md) | SNMP — device configs via community strings |
| 14 | [14-smb-enumeration.md](14-smb-enumeration.md) | SMB — shares, services, usernames |
| 15 | [15-active-directory-enumeration.md](15-active-directory-enumeration.md) | Active Directory users/groups, the AD attack chain |
| 16 | [16-dns-zone-transfer.md](16-dns-zone-transfer.md) | DNS Zone Transfer (AXFR) misconfigurations |
| 17 | [17-ldap-enumeration.md](17-ldap-enumeration.md) | LDAP — directory data via ldapsearch |
| 18 | [18-nfs-rpc-enumeration.md](18-nfs-rpc-enumeration.md) | NFS & RPC — file shares, portmapper |
| 19 | [19-bgp-basics.md](19-bgp-basics.md) | BGP — internet routing, hijacking basics |
| 20 | [20-os-fingerprinting.md](20-os-fingerprinting.md) | OS Fingerprinting — active vs passive |
| 21 | [21-extra-enumeration-tools.md](21-extra-enumeration-tools.md) | CrackMapExec, BloodHound, dnsenum, and India-relevant notes |
| 22 | [22-enumeration-checklist.md](22-enumeration-checklist.md) | End-to-end enumeration workflow checklist |

## 🧭 How to use this
Each chapter follows the same structure: **Key Points → Tools → How It Works → Mini Guide/Example.** Work through Module 03 (1→7 build on each other, 8–9 are reference material, 10 is the workflow checklist), then Module 04 the same way (11 is foundational, 12–20 are protocol-by-protocol, 21 is reference material, 22 is the workflow checklist). Module 04 assumes Module 03 scanning is already done — enumeration targets the ports scanning found open.

## 🗂️ Source
Notes consolidated from course slides: *"Module 03: Scanning Networks with Nmap & Zenmap 🔥 | Practical Ethical Hacking Course | Hindi Free"* and *"Module 04: Enumeration in Ethical Hacking | Active Directory, NetBIOS, DNS, LDAP, SNMP | Hindi 2025"*.
