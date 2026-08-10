# Chapter 22 — End-to-End Enumeration Workflow Checklist

Ties together Chapters 11–21 into a practical order of operations, picking up right where the Scanning checklist (Ch.10) leaves off.

## Phase 0 — Prerequisites
- [ ] Scanning phase (Ch.1–10) complete — you have a live-host list and open-port map
- [ ] Written authorization still covers active enumeration (usernames/data extraction, not just port state)
- [ ] Identify which enumeration-relevant ports are open: 53 (DNS), 111 (RPC), 137-139/445 (NetBIOS/SMB), 161 (SNMP), 389 (LDAP), 2049 (NFS)

## Phase 1 — NetBIOS & SMB (Ch.12, Ch.14)
- [ ] `enum4linux -a <target-ip>` for a full automated pass
- [ ] `smbclient -L <target-ip> -U anonymous` — check for anonymous share access
- [ ] `smbmap -H <target-ip>` — confirm share permissions
- [ ] Note any writable shares — high-priority finding

## Phase 2 — SNMP (Ch.13)
- [ ] `onesixtyone` against the subnet to find default community strings
- [ ] `snmpwalk -v2c -c public <target-ip>` on any responsive host
- [ ] Flag any device still using `public`/`private` — near-instant compromise path

## Phase 3 — DNS (Ch.16)
- [ ] Attempt zone transfer: `dig axfr @<nameserver> <domain>`
- [ ] If blocked (expected on hardened targets), fall back to `dnsenum`/`dnsrecon` for brute-force subdomain discovery

## Phase 4 — LDAP & Active Directory (Ch.15, Ch.17)
- [ ] Test anonymous LDAP bind: `ldapsearch -x -h <target-ip> -b "dc=domain,dc=com"`
- [ ] `rpcclient -U "" -N <target-ip>` → try `enumdomusers`, `enumdomgroups`
- [ ] If credentials obtained anywhere in this phase, run BloodHound/SharpHound for attack-path mapping

## Phase 5 — NFS & RPC (Ch.18)
- [ ] `rpcinfo -p <target-ip>` — list registered RPC services
- [ ] `showmount -e <target-ip>` — list NFS exports
- [ ] Attempt mount only if scope explicitly allows filesystem interaction

## Phase 6 — OS Fingerprinting (Ch.20)
- [ ] `nmap -O <target-ip>` for active fingerprinting
- [ ] Cross-check against banner-grabbed service versions (Module 03, Ch.4) for consistency

## Phase 7 — Network-Level Context (Ch.19, large scope only)
- [ ] For engagements covering an organization's public ASN: check bgp.he.net / RIPEstat for announced prefixes
- [ ] Note this is informational/context-gathering only — not an active attack technique in scope for most engagements

## Phase 8 — Consolidate & Report
- [ ] Compile a master list: usernames found, shares found (with permissions), community strings that worked, DNS records exposed, OS per host
- [ ] Prioritize: writable shares > default SNMP strings > anonymous LDAP/zone transfer > informational OS/version data
- [ ] This consolidated output becomes the direct input for **Step 3: Gaining Access**

## Golden Rule
Enumeration should always answer one question per protocol: *"What is this service willing to tell me without a fight?"* The answer to that question — not a raw port list — is what actually drives the next phase of the engagement.
