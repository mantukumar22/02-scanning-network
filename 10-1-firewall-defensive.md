# Firewall Fundamentals with `iptables`
### A Practical & Ethical Hacker's Guide

This repo documents how to build a basic Linux firewall using `iptables`, and how to reason about it from an offensive-security (ethical hacking) point of view — i.e., how an attacker would probe it, and how a defender closes those gaps.

> ⚠️ **Scope note:** Everything here is defensive/educational. Only run scans or attacks against systems you own or are explicitly authorized to test.

---

## Table of Contents

1. [Introduction](#chapter-1-introduction)
2. [Prerequisites & Checking Rules](#chapter-2-prerequisites--checking-rules)
3. [Basic Allow/Deny Rules](#chapter-3-basic-allowdeny-rules)
4. [Blocking Port Scans](#chapter-4-blocking-port-scans)
5. [Rate Limiting Connections](#chapter-5-rate-limiting-connections)
6. [Persisting Rules After Reboot](#chapter-6-persisting-rules-after-reboot)
7. [Ethical Hacker Perspective: Testing Your Own Firewall](#chapter-7-ethical-hacker-perspective-testing-your-own-firewall)
8. [Common Weaknesses & Hardening Checklist](#chapter-8-common-weaknesses--hardening-checklist)

---

## Chapter 1: Introduction

`iptables` is a command-line firewall utility built into Linux. It works by evaluating packets against **policy chains** — ordered lists of rules. When a packet matches a rule, it's handed a **target**: `ACCEPT`, `DROP`, or `REJECT`.

From a security standpoint, a firewall is your first filtering layer — it decides *what even gets a chance* to reach a service. An ethical hacker cares about firewalls in two ways:
- **As an attacker (during authorized testing):** identifying what's open, what's filtered, and what rules can be bypassed.
- **As a defender:** writing rules that don't just block obvious traffic, but also resist reconnaissance techniques like port scanning.

---

## Chapter 2: Prerequisites & Checking Rules

Run all commands as root or with `sudo`. To review your current ruleset with line numbers (useful for editing/deleting specific rules later):

```bash
sudo iptables -L -v -n --line-numbers
```

- `-L` — list rules
- `-v` — verbose (packet/byte counters)
- `-n` — numeric output (no DNS resolution, faster and avoids leaking your lookups)
- `--line-numbers` — shows rule position, needed for `iptables -D INPUT <line>`

---

## Chapter 3: Basic Allow/Deny Rules

By default, new rules are appended to the `INPUT` chain (incoming traffic).

**Allow a specific port** (e.g., SSH on 22):
```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

**Block a specific port** (e.g., HTTP on 80):
```bash
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

**Block a specific IP address:**
```bash
sudo iptables -A INPUT -s 192.168.1.50 -j DROP
```

| Flag | Meaning |
|---|---|
| `-A INPUT` | Append rule to the INPUT chain |
| `-p tcp` | Match TCP protocol |
| `--dport` | Destination port to match |
| `-s` | Source IP/CIDR to match |
| `-j ACCEPT/DROP/REJECT` | Action ("jump") to take |

**DROP vs REJECT:** `DROP` silently discards the packet — the sender gets no response, so from the attacker's side the port just looks "dead" (harder to distinguish filtered vs. non-existent). `REJECT` sends back an error, which is more polite but reveals that something is actively filtering.

---

## Chapter 4: Blocking Port Scans

Port scanners (Nmap and similar tools) probe for open ports using unusual TCP flag combinations designed to slip past naive rules or fingerprint the OS. Recognizing these patterns is core to both offensive scanning knowledge and defensive rule-writing.

**Common stealth scan signatures:**

```bash
# NULL scan — no flags set at all
sudo iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP

# Xmas scan — FIN, PSH, URG all set ("lit up like a Christmas tree")
sudo iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP

# SYN-FIN scan — contradictory flags, never legitimate
sudo iptables -A INPUT -p tcp --tcp-flags ALL SYN,FIN -j DROP
```

**Why these work as detection signals:** legitimate TCP connections always start with a lone SYN flag. NULL, Xmas, and SYN-FIN combinations don't occur in normal traffic — they exist specifically to evade simple stateful filters or elicit RFC-defined responses that reveal open/closed ports without completing a handshake. Any packet matching these patterns is scan traffic by definition, so dropping them outright is safe.

---

## Chapter 5: Rate Limiting Connections

Even a scanner using "normal" SYN packets can be slowed down by capping how many new connections a single source can open per second — this blunts both scanning and basic SYN-flood-style DoS attempts.

```bash
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
sudo iptables -A INPUT -p tcp --syn -j DROP
```

- `-m limit` — loads the rate-limiting match module
- `--limit 1/s` — allow at most 1 new SYN per second on average
- `--limit-burst 3` — but allow short bursts up to 3 before the limit kicks in
- The second line drops anything exceeding that rate

This is a lightweight defense — for serious DoS mitigation you'd pair it with `fail2ban`, connection tracking (`conntrack`), or an upstream service, but it materially raises the cost of a naive scan.

---

## Chapter 6: Persisting Rules After Reboot

`iptables` rules live in memory and are wiped on reboot unless saved.

**Ubuntu / Debian:**
```bash
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

**CentOS / RHEL:**
```bash
sudo service iptables save
```

---

## Chapter 7: Ethical Hacker Perspective — Testing Your Own Firewall

Once rules are in place, the ethical-hacking mindset is to verify them the way an attacker would, **against systems you're authorized to test**:

1. **Confirm silent drops behave as expected.** Scan a DROP-protected port from another host with `nmap -sS <target>` and confirm it shows as `filtered` rather than `closed` (closed implies a RST was sent, which DROP never does).
2. **Test the stealth-scan rules.** Run `nmap -sN` (NULL), `nmap -sX` (Xmas), and `nmap -sF` (FIN) against the host and confirm your `--tcp-flags` rules catch them — you should see no meaningful response, not port-state leakage.
3. **Validate rate limiting.** A rapid `nmap -T4` or `-T5` scan should get throttled — later probes should show `filtered` once the burst allowance is exhausted, confirming the limit rule is actually engaging.
4. **Check rule order.** `iptables` evaluates top-down and stops at the first match. A broad `ACCEPT` placed above your DROP/reject rules will silently defeat them — always list your ruleset (`-L -v -n --line-numbers`) and confirm restrictive rules aren't shadowed by earlier permissive ones.
5. **Verify persistence.** Reboot (in a test environment) and re-check that rules survived — a firewall that resets on every restart is a common real-world gap that penetration testers flag.

---

## Chapter 8: Common Weaknesses & Hardening Checklist

Things an attacker looks for — and a defender should close:

- [ ] **No default-deny policy.** Set `iptables -P INPUT DROP` as a baseline, then explicitly allow what's needed, rather than relying only on scattered DROP rules.
- [ ] **Missing `ESTABLISHED,RELATED` rule**, which forces you to write bidirectional rules for every service instead of just allowing return traffic:
  ```bash
  sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  ```
- [ ] **Loopback interface not explicitly allowed**, which can break local services:
  ```bash
  sudo iptables -A INPUT -i lo -j ACCEPT
  ```
- [ ] **Rules not persisted** — see Chapter 6.
- [ ] **Only IPv4 covered.** If IPv6 is enabled, mirror your rules with `ip6tables` or disable IPv6 if unused.
- [ ] **No logging before drop**, making it hard to investigate incidents:
  ```bash
  sudo iptables -A INPUT -j LOG --log-prefix "iptables-dropped: "
  ```
- [ ] **Overly broad allow rules** (e.g., allowing an entire `/16` instead of the specific hosts that need access).

---

## Sources / Further Reading

This document synthesizes general `iptables` firewall practices; consult your distribution's official documentation and the `man iptables` page for the authoritative, up-to-date syntax reference.
