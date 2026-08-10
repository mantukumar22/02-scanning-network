# Chapter 9 — Extra Scanning Tools (Global & India Context)

Tools not covered in the original course slides but essential in a modern (2026) scanning toolkit.

## Faster / Large-Scale Port Scanners
- **Masscan** — the fastest port scanner available; can scan the entire IPv4 address space in under 10 minutes. Use for very large ranges, then hand off interesting hosts to Nmap for deep service detection.
  ```bash
  masscan -p1-65535 192.168.1.0/24 --rate 1000
  ```
- **RustScan** — modern, extremely fast port scanner written in Rust; auto-pipes results into Nmap for service detection.
  ```bash
  rustscan -a 192.168.1.10 -- -sV
  ```
- **Unicornscan** — another high-speed async scanner, good for UDP scanning where Nmap can be slow.

## Packet Crafting & Manual Probing
- **hping3** — craft custom TCP/IP/ICMP packets for manual scan technique testing, firewall rule testing, and basic DoS-testing in authorized labs.
- **Netcat (`nc`)** — the classic "Swiss Army Knife" for manual banner grabbing, port checking, and simple file transfer over TCP/UDP.
  ```bash
  nc -zv target.com 1-100    # Quick port check
  ```
- **Scapy** (Python) — build fully custom packets for advanced scan technique research and protocol testing.

## Internet-Wide Search (Complementary to Active Scanning)
- **Shodan.io / Censys.io** — instead of scanning yourself, query their pre-scanned index of the entire internet for exposed devices/services tied to a target (zero packets sent by you). Covered in depth in the Footprinting module — very complementary to active Nmap scans.
- **ZoomEye / FOFA** — Chinese equivalents with strong APAC coverage.

## Web-Specific Scanning
- **Nuclei** (ProjectDiscovery) — fast, template-based vulnerability scanner for web apps; huge community template library covering CVEs, misconfigurations, and exposed panels.
- **WhatWeb** — fingerprints web technologies (CMS, frameworks, JS libraries) faster and lighter than a full Nikto run.
- **Httpx** (ProjectDiscovery) — fast HTTP probing across large lists of hosts/subdomains.

## India-Relevant Notes
- **CERT-In (cert-in.org.in)** — under Indian law, certain categories of security testing and vulnerability reporting have specific procedures; CERT-In publishes advisories and, as of recent years, has had reporting-window requirements for some incident types. Check current guidance before formal engagements involving Indian entities.
- **IT Act, 2000 (Sections 43 & 66)** — unauthorized "access or attempt to access" a computer resource is a punishable offence in India; active scanning (this entire module) falls squarely under "access," so written scope authorization is not optional.
- Government and critical-infrastructure targets in India often fall under **additional restricted-scanning rules** (e.g. RBI-regulated entities, NIC-hosted domains) — always confirm sector-specific rules before scanning even with general authorization.

## Practical Tip
For large external engagements: **Masscan/RustScan for breadth → Nmap for depth on interesting hosts → Nuclei/Nikto for web-specific findings → Nessus/OpenVAS for a full vulnerability sweep.** This layered approach is both faster and more thorough than running one tool alone.
