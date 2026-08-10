# Chapter 16 — DNS Zone Transfer

## What Is a DNS Zone Transfer?

**Key points**
- A DNS Zone Transfer allows pulling down **all** the DNS records of a domain in one request.
- It's typically meant for legitimate DNS servers to replicate data between a primary and secondary nameserver.
- When misconfigured to allow **any** requester, it becomes a massive information leak — every subdomain, mail server, and internal hostname in one shot.

---

## How Attackers Use It

**Key points**
- If a DNS server allows unauthenticated zone transfers, an attacker can grab every single DNS record: subdomains, mail servers, internal hostnames, etc.
- This instantly maps a huge portion of an organization's infrastructure without any active port scanning at all.
- A single misconfigured secondary/authoritative nameserver is enough — even if the primary is locked down correctly.

**Tools:** `dig axfr`, `host -t axfr`, `nmap --script dns-zone-transfer`

**How it works:** The AXFR (full zone transfer) request asks a nameserver "give me your entire zone file." A properly configured server checks the requester against an allow-list of known secondary servers; a misconfigured one just sends the data to anyone who asks.

**Mini example:**
```bash
dig axfr @ns1.target.com target.com
host -t axfr target.com ns1.target.com
nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=target.com -p 53 ns1.target.com
```

## Defensive Note
Restrict zone transfers (AXFR) to explicitly authorized secondary nameserver IPs only, and consider DNSSEC to add integrity checks. This is a simple, high-impact fix that's still frequently missed in real audits.
