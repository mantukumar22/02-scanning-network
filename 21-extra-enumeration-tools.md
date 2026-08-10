# Chapter 21 — Extra Enumeration Tools (Global & India Context)

Tools not covered in the original course slides but valuable in a modern (2026) enumeration toolkit.

## SMB / Windows Enumeration
- **CrackMapExec (CME) / NetExec** — the modern go-to for Windows/AD environments; combines SMB enumeration, credential testing, and lateral movement checks in one tool.
  ```bash
  netexec smb 192.168.1.0/24 -u '' -p '' --shares
  ```
- **BloodHound + SharpHound** — maps AD attack paths visually (who can reach Domain Admin and how) using graph theory; the standard tool for AD attack-path analysis after basic enumeration.
- **SMBMap** — quick, readable share + permission enumeration, good alternative to raw `smbclient`.

## DNS Enumeration (beyond zone transfer)
- **dnsenum** — automates subdomain brute-forcing, zone transfer attempts, and Google scraping for a target domain.
- **dnsrecon** — similar to dnsenum with more output format options; good for scripting into larger recon pipelines.
- **fierce** — lightweight, fast DNS reconnaissance tool, good for a first pass.

## LDAP / Active Directory
- **ldapdomaindump** — dumps AD data (users, groups, computers, policies) into readable HTML/JSON reports from an LDAP bind.
- **windapsearch** — LDAP enumeration tool built specifically for AD environments, with useful flags for finding privileged users.

## SNMP
- **snmp-check** — friendlier formatted output than raw `snmpwalk`, good for quick triage.
- **Braa** — mass SNMP scanner for querying many hosts/OIDs in parallel (faster than looping `snmpwalk`).

## General / Multi-Protocol
- **Nmap NSE enumeration scripts** — Nmap isn't just for scanning; its script engine covers most protocols in this module:
  ```bash
  nmap --script smb-enum-shares,smb-enum-users -p 445 <target-ip>
  nmap --script ldap-search -p 389 <target-ip>
  nmap --script snmp-info -p 161 <target-ip>
  ```
- **Metasploit auxiliary modules** — `auxiliary/scanner/smb/smb_enumusers`, `auxiliary/scanner/snmp/snmp_enum`, and similar modules wrap many of the above techniques inside the Metasploit Framework for engagement consistency.

## India-Relevant Notes
- **IT Act, 2000 (Sections 43 & 66)** — enumeration is active, unauthorized-access-adjacent activity; the same authorization requirements from the Scanning module (Ch.1–10) apply here, arguably more strictly since enumeration often extracts actual usernames and personal data.
- **DPDP Act, 2023** — if enumeration surfaces personal data (usernames tied to real employees, email addresses, etc.), handling and storing that data during an engagement falls under India's data protection law — scope documents should explicitly cover data handling/retention for anything enumeration turns up.
- **CERT-In** — for engagements involving Indian critical infrastructure or government-linked domains, check current CERT-In empanelment/reporting requirements before running AD or DNS enumeration against such targets.

## Practical Tip
A strong Module 04 workflow: **Nmap NSE scripts for a fast multi-protocol pass → enum4linux/CrackMapExec for SMB+AD depth → ldapsearch/windapsearch for directory detail → BloodHound for attack-path visualization.** This mirrors how real internal pentests move from broad enumeration to a specific privilege-escalation plan.
