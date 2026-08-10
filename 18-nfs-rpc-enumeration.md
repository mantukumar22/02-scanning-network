# Chapter 18 — NFS & RPC Enumeration

## What Is NFS?

**Key points**
- Network File System (NFS) allows the sharing of files between Linux/UNIX systems.
- Its Linux/UNIX-world equivalent of what SMB (Ch.14) is for Windows.
- Runs over RPC rather than a single fixed port, which makes it dependent on portmapper (below).

## What Is RPC?

**Key points**
- Remote Procedure Call (RPC) allows programs to execute commands on remote systems, often used in NFS setups.
- Portmapper (port 111) is the "directory service" for RPC — it tells a client which port a given RPC service is actually listening on.

---

## How Attackers Use It

**Key points**
- By enumerating NFS and RPC, an attacker can check for vulnerable file shares or services that could lead to an exploit.
- Misconfigured NFS exports (`/etc/exports` with weak restrictions) can allow **any** client to mount and read/write a share.

**How it works (the portmapper flow):**
```
Client program → (2) → Portmapper (port 111) → asks which port a service is on
Portmapper → (1) → tells the client the actual service port
Client program → (3) → connects directly to the Server program on that port
```

**Tools:** `rpcinfo`, `showmount`, `nmap --script nfs-*`

**Mini example:**
```bash
rpcinfo -p <target-ip>              # List all RPC services registered with portmapper
showmount -e <target-ip>            # List NFS exported shares
mount -t nfs <target-ip>:/share /mnt/target   # Mount an exported share (if permissions allow)
```

## Defensive Note
Restrict NFS exports to specific trusted client IPs only (never `*` or `0.0.0.0/0`), disable portmapper/RPC services on systems that don't need them, and firewall port 111 from untrusted networks.
