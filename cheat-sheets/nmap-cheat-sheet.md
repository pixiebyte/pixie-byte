# Nmap Cheat Sheet

The Swiss Army knife of network scanning. If you're in cybersecurity, you'll use Nmap more than you use your coffee machine.

## Basic scans

```bash
# Scan a single host
nmap 192.168.1.1

# Scan a range
nmap 192.168.1.1-254

# Scan a subnet
nmap 192.168.1.0/24

# Scan from a file of targets
nmap -iL targets.txt

# Scan specific ports
nmap -p 22,80,443 192.168.1.1

# Scan a port range
nmap -p 1-1000 192.168.1.1

# Scan all 65535 ports
nmap -p- 192.168.1.1
```

## Scan types

```bash
# TCP SYN scan (default, fast, stealthy-ish)
nmap -sS 192.168.1.1

# TCP Connect scan (full connection, more visible)
nmap -sT 192.168.1.1

# UDP scan (slow but important — many services use UDP)
nmap -sU 192.168.1.1

# Ping scan — just find live hosts, no port scan
nmap -sn 192.168.1.0/24

# Skip host discovery — scan even if host seems down
nmap -Pn 192.168.1.1
```

## Service and version detection

```bash
# Detect service versions
nmap -sV 192.168.1.1

# Aggressive version detection
nmap -sV --version-intensity 5 192.168.1.1

# OS detection
nmap -O 192.168.1.1

# Everything — OS, versions, scripts, traceroute
nmap -A 192.168.1.1
```

## NSE scripts (Nmap Scripting Engine)

```bash
# Run default scripts
nmap -sC 192.168.1.1

# Run a specific script
nmap --script=http-title 192.168.1.1

# Run a category of scripts
nmap --script=vuln 192.168.1.1

# Common combo — version detection + default scripts
nmap -sV -sC 192.168.1.1

# List available scripts
ls /usr/share/nmap/scripts/

# Search for scripts by name
ls /usr/share/nmap/scripts/ | grep smb
```

## Useful script examples

```bash
# Check for SMB vulnerabilities
nmap --script=smb-vuln* -p 445 192.168.1.1

# Enumerate HTTP methods
nmap --script=http-methods -p 80,443 192.168.1.1

# Brute force SSH (use responsibly — lab only!)
nmap --script=ssh-brute -p 22 192.168.1.1

# DNS zone transfer check
nmap --script=dns-zone-transfer -p 53 192.168.1.1

# SSL/TLS info and vulnerabilities
nmap --script=ssl-enum-ciphers,ssl-heartbleed -p 443 192.168.1.1
```

## Speed and timing

```bash
# Timing templates (0=paranoid, 5=insane)
nmap -T0 192.168.1.1    # Slowest — IDS evasion
nmap -T3 192.168.1.1    # Default — balanced
nmap -T4 192.168.1.1    # Fast — good for CTFs and labs
nmap -T5 192.168.1.1    # Fastest — noisy, may miss results

# Custom rate limiting
nmap --min-rate 1000 192.168.1.1
nmap --max-rate 100 192.168.1.1
```

## Output formats

```bash
# Normal output to file
nmap -oN scan_results.txt 192.168.1.1

# XML output (good for parsing)
nmap -oX scan_results.xml 192.168.1.1

# Grepable output
nmap -oG scan_results.gnmap 192.168.1.1

# All formats at once
nmap -oA scan_results 192.168.1.1
```

## My go-to commands

```bash
# Quick network discovery
nmap -sn -T4 192.168.1.0/24

# Standard full scan for a single target
nmap -sV -sC -O -T4 -p- 192.168.1.1 -oA full_scan

# Quick top-ports scan
nmap -sV -sC -T4 --top-ports 1000 192.168.1.1

# Stealthy scan with version detection
nmap -sS -sV -T2 -p- 192.168.1.1

# Web server focused
nmap -sV -sC -p 80,443,8080,8443 --script=http-* 192.168.1.1
```

## Pro tips

- Always save your output (`-oA`) — you'll want to reference it later
- Start with a quick scan (`--top-ports 1000`), then do a full port scan
- Use `-T4` in labs and CTFs, `-T2` or lower in real engagements
- Combine `-sV -sC` for the most useful default output
- UDP scans are slow but don't skip them — SNMP, DNS, TFTP hide there
- On large networks, discover hosts first (`-sn`), then scan live hosts

## Legal reminder

Only scan networks you own or have explicit written permission to test. Unauthorised scanning is illegal in most jurisdictions. Use a home lab or platforms like TryHackMe and Hack The Box for practice.

---

*Nmap: because "have you tried turning it off and on again" isn't a security strategy. 💜*
