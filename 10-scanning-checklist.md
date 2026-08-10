# Chapter 10 — End-to-End Scanning Workflow Checklist

Ties together Chapters 1–9 into a practical order of operations for an authorized scanning engagement.

## Phase 0 — Scope & Authorization
- [ ] Written authorization / signed scope in hand, confirming exactly which IPs/ranges are in scope
- [ ] Confirm timing windows (some orgs require scans during off-peak hours)
- [ ] Note legal framework (India: IT Act Sections 43/66; elsewhere: local law)

## Phase 1 — Host Discovery (Ch.2)
- [ ] Ping sweep: `nmap -sn <subnet>`
- [ ] ARP scan (if on the same LAN): `arp-scan <subnet>` or `netdiscover -r <subnet>`
- [ ] Compile a confirmed list of live hosts

## Phase 2 — Port Scanning (Ch.3, Ch.5)
- [ ] Fast scan first: `nmap -F <IP>` (top 100 ports) for a quick overview
- [ ] Full scan on priority hosts: `nmap -p- <IP>`
- [ ] Stealth scan if evasion needed: `nmap -sS <IP>`
- [ ] Firewall rule check: `nmap -sA <IP>`

## Phase 3 — Service & OS Fingerprinting (Ch.1, Ch.4, Ch.5)
- [ ] Version detection: `nmap -sV <IP>`
- [ ] OS detection: `nmap -O <IP>`
- [ ] Manual banner grab on interesting ports: `telnet <IP> <port>` or `nc -v <IP> <port>`
- [ ] Combined deep scan: `nmap -sS -sV -O -A <IP>`

## Phase 4 — Broad + Fast Coverage (large ranges only) (Ch.9)
- [ ] Masscan/RustScan sweep across the full range
- [ ] Feed interesting hosts back into Nmap for deep-dive scanning

## Phase 5 — Vulnerability Correlation (Ch.8)
- [ ] Cross-reference each service+version against known CVEs (manually or via Nessus/OpenVAS)
- [ ] Run Nikto/Nuclei against any discovered web servers
- [ ] Flag outdated/EOL software (e.g. Windows 7, old Apache/OpenSSH builds) as high priority

## Phase 6 — Evasion (only if testing detection capability) (Ch.7)
- [ ] Re-run key scans with `-f` (fragmentation) to test IDS/firewall packet reassembly
- [ ] Test decoy scans (`-D`) if evaluating log/attribution defenses
- [ ] Test source-port bypass (`--source-port 53/443`) against the firewall ruleset

## Phase 7 — Document & Report
- [ ] Save all scan output: `nmap ... -oN scan_results.txt` (or `-oX` for XML/tooling ingestion)
- [ ] Build a Zenmap topology view for stakeholder-facing reporting (Ch.6)
- [ ] Prioritize findings: exposed/default-credential services > known-CVE services > outdated software > informational findings
- [ ] Write remediation guidance alongside each finding — not just a list of open ports

## Golden Rule
Discovery → Ports → Services/OS → Vulnerabilities → (Evasion testing, if relevant) → Report. Never skip straight to vulnerability scanning without first understanding what's actually alive and listening — it wastes scanner time and produces noisier results.
