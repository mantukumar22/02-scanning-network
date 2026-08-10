# Chapter 6 — Zenmap GUI

## What Is Zenmap?

**Key points**
- Zenmap is the **official GUI for Nmap** — no need to remember all those long Nmap commands.
- Built for: beginners who prefer a visual interface, advanced users who want to save scan profiles, and anyone visualizing scan results graphically (network topology maps).

**Tools:** Zenmap (bundled with Nmap installers on Windows/macOS; separate package on Linux)

**How it works:** Zenmap wraps the Nmap CLI — you pick a target and a scan "profile" (e.g. Intense Scan, Quick Scan, Ping Scan) from a dropdown, and it builds and runs the equivalent Nmap command behind the scenes, then renders the results in tabs (Nmap Output, Ports/Hosts, Topology, Host Details, Scans).

## Why use it over the CLI
- **Topology tab** draws a live visual map of discovered hosts and their relationships — useful for presenting findings to non-technical stakeholders.
- **Saved profiles** let you re-run a specific scan configuration without retyping flags.
- **Compare Results** feature lets you diff two scans of the same network taken at different times — great for spotting new/changed devices.

## Mini Guide
1. Open Zenmap → enter target (IP, range, or hostname) in the **Target** field.
2. Choose a **Profile** (e.g. "Intense scan, all TCP ports" ≈ `nmap -p 1-65535 -T4 -A -v`).
3. Click **Scan** → results populate the Nmap Output tab in real time.
4. Switch to the **Ports/Hosts** tab for a sortable table of open ports per host.
5. Switch to **Topology** for a visual map of the scanned network.
6. Save the profile if you'll repeat this exact scan later.

## When to use Zenmap vs. raw Nmap
- **Zenmap:** learning, one-off exploratory scans, visual reporting for a client/stakeholder.
- **Raw Nmap CLI:** scripting, automation, remote/headless servers (Zenmap needs a desktop environment), fine-grained control over flags.
