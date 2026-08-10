# 📡 Scanning Networks (Nmap & Zenmap)

Chapter-wise notes on network scanning, built from course slides on Nmap & Zenmap, reorganized for quick reference, with extra tools added from global scanning practice.

> ⚠️ **Legal note:** Everything past the *passive* recon stage — port scans, service detection, firewall evasion, OS fingerprinting — is **active** and touches the target's systems directly. Only run these against systems you own or have **written authorization** to test. Unauthorized scanning can be prosecuted under India's IT Act 2000 (Sections 43, 66) and equivalent laws elsewhere (CFAA in the US, Computer Misuse Act in the UK). `scanme.nmap.org` is Nmap's own public test target — safe to practice against.

## 📚 Chapters

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

## 🧭 How to use this
Each chapter follows the same structure: **Key Points → Tools → How It Works → Mini Guide/Example.** Work through 1→7 in order (they build on each other); 8–9 are reference/extension material; 10 ties it all into one workflow you can run start to finish.

## 🗂️ Source
Notes consolidated from course slides: *"Module 03: Scanning Networks with Nmap & Zenmap 🔥 | Practical Ethical Hacking Course | Hindi Free"*.
