# Chapter 14 — SMB Enumeration

## What Is SMB?

**Key points**
- Server Message Block (SMB) is a network file-sharing protocol used in Windows environments to access files and printers.
- Runs on TCP port 445 (and legacy 139 via NetBIOS).
- One of the most historically exploited protocols (e.g. EternalBlue/WannaCry) — enumeration here is high-value.

**Analogy:** SMB is like someone leaving the front door to their house wide open. You're just walking in, checking out what snacks they left on the table.

---

## How Attackers Use It

**Key points**
- SMB enumeration reveals a list of shares, services, and even usernames — allowing further exploitation or direct access.
- Anonymous/guest access (if enabled) can expose share listings with **no credentials at all**.
- Combined with NetBIOS enumeration (Ch.12), SMB gives a very complete picture of a Windows host.

**Tools:** `smbclient`, `rpcclient`, `enum4linux`, `smbmap`, `crackmapexec`

**How it works:** `smbclient` connects to a target's SMB service and lists available shares just like an FTP client would — if anonymous access is permitted, no username/password is even needed to browse.

**Mini example:**
```bash
smbclient -L <target-ip> -U anonymous     # List available shares
smbclient //<target-ip>/SHARE_NAME -U anonymous   # Connect to a specific share
smbmap -H <target-ip>                     # Cleaner share + permission overview
```

## Defensive Note
Disable SMBv1 entirely (it's the version most exploited), disable anonymous/guest access to shares, and require SMB signing to prevent relay attacks.
