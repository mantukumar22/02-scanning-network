# Chapter 19 — Border Gateway Protocol (BGP) Basics

## What Is BGP?

**Key points**
- BGP is the protocol that makes decisions on how data is routed across the internet — the "traffic controller" of the internet's highways.
- Operates between **Autonomous Systems (AS)** — large networks (ISPs, big companies, cloud providers) each with their own AS number, sharing routing information with each other's routers.
- Unlike the other topics in this module, BGP enumeration/attacks work at the **internet routing level**, not against a single host.

**Analogy:** Imagine redirecting traffic on the internet highways and sending everyone to the wrong exit.

---

## How Attackers Use It

**Key points**
- By manipulating BGP, attackers can hijack internet traffic — redirecting it to malicious servers or causing Denial of Service (DoS).
- This is known as a **BGP hijack**: announcing IP ranges you don't actually own, tricking other routers into sending traffic your way.
- Real-world impact has included large-scale traffic redirection incidents affecting major cloud/crypto services — this is a nation-state/ISP-level attack surface, not something in scope for typical pentests.

**How it works (simplified):**
```
1st Autonomous System (Router 1) ←── Sharing of routing information ──→ 2nd Autonomous System (Router 2)
```
Routers exchange "I can reach this IP range" announcements; if a malicious or misconfigured AS announces a range it doesn't own, other routers may start sending that range's traffic to the wrong place.

**Tools (for observation/research, not offense):** `bgpq3`, RIPEstat, Hurricane Electric's BGP Toolkit (bgp.he.net), `whois` for ASN lookups

**Mini example (defensive/research use):**
```bash
whois -h whois.radb.net AS15169         # Look up a known AS (e.g. Google) 
# bgp.he.net — check current announced prefixes for an ASN
```

## Why This Is In an Enumeration Module
Understanding BGP lets a pentester/researcher map an organization's actual internet-facing infrastructure at the ISP/routing level — useful for large-scope engagements — and understand a very high-impact (if rare) attack class.

## Defensive Note
Organizations should implement **RPKI (Resource Public Key Infrastructure)** to cryptographically validate route announcements, and monitor BGP announcements for their own IP ranges via services like BGPmon or Cloudflare Radar.
