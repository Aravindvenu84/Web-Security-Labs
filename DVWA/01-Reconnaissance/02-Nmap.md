
# Nmap - Complete Guide

> A practical guide to using Nmap for reconnaissance, enumeration, and service discovery during penetration testing and cybersecurity labs.

---

# Table of Contents

1. Introduction
2. Installation
3. Basic Syntax
4. Host Discovery
5. Port Scanning
6. Service & Version Detection
7. OS Detection
8. Timing Options
9. NSE (Nmap Scripting Engine)
10. Useful HTTP Scripts
11. Vulnerability Scanning
12. Output Formats
13. Firewall Evasion
14. Common Workflows
15. Command Cheat Sheet
16. Best Practices

---

# Introduction

Nmap (Network Mapper) is an open-source network scanner used for:

- Host Discovery
- Port Scanning
- Service Enumeration
- Operating System Detection
- Version Detection
- Vulnerability Assessment (using NSE)
- Network Auditing

It is one of the most important tools used in:

- Penetration Testing
- Red Teaming
- Blue Teaming
- System Administration
- CTF Challenges

---

# Installation

## Kali Linux

```bash
sudo apt install nmap
```

## Ubuntu

```bash
sudo apt install nmap
```

## Verify Installation

```bash
nmap --version
```

---

# Basic Syntax

```bash
nmap [OPTIONS] <TARGET>
```

Example:

```bash
nmap 192.168.1.10
```

---

# Target Specification

Single Host

```bash
nmap 192.168.1.15
```

Multiple Hosts

```bash
nmap 192.168.1.10 192.168.1.15
```

Subnet

```bash
nmap 192.168.1.0/24
```

Range

```bash
nmap 192.168.1.10-100
```

Hostname

```bash
nmap example.com
```

---

# Host Discovery

Ping Scan

```bash
nmap -sn 192.168.1.0/24
```

Skip Host Discovery

```bash
nmap -Pn 192.168.1.10
```

ARP Scan (Local Network)

```bash
nmap -PR 192.168.1.0/24
```

---

# Port Scanning

Default Scan

```bash
nmap 192.168.1.10
```

Specific Port

```bash
nmap -p80 192.168.1.10
```

Multiple Ports

```bash
nmap -p22,80,443 192.168.1.10
```

Port Range

```bash
nmap -p1-1000 192.168.1.10
```

All TCP Ports

```bash
nmap -p- 192.168.1.10
```

Top 100 Ports

```bash
nmap --top-ports 100 192.168.1.10
```

Fast Scan

```bash
nmap -F 192.168.1.10
```

---

# Scan Types

TCP SYN Scan (Stealth)

```bash
sudo nmap -sS 192.168.1.10
```

TCP Connect Scan

```bash
nmap -sT 192.168.1.10
```

UDP Scan

```bash
sudo nmap -sU 192.168.1.10
```

TCP + UDP

```bash
sudo nmap -sS -sU 192.168.1.10
```

---

# Service Detection

```bash
nmap -sV 192.168.1.10
```

More Aggressive Version Detection

```bash
nmap -sV --version-all
```

---

# Operating System Detection

```bash
sudo nmap -O 192.168.1.10
```

---

# Aggressive Scan

```bash
sudo nmap -A 192.168.1.10
```

Includes:

* OS Detection
* Version Detection
* Default NSE Scripts
* Traceroute

---

# Timing Templates

| Option | Speed      |
| ------ | ---------- |
| T0     | Paranoid   |
| T1     | Sneaky     |
| T2     | Polite     |
| T3     | Normal     |
| T4     | Aggressive |
| T5     | Insane     |

Example

```bash
nmap -T4 192.168.1.10
```

---

# Useful Performance Options

Increase Packet Rate

```bash
nmap --min-rate 5000
```

Maximum Retries

```bash
nmap --max-retries 2
```

Host Timeout

```bash
nmap --host-timeout 5m
```

---

# NSE (Nmap Scripting Engine)

Run Default Scripts

```bash
nmap -sC 192.168.1.10
```

Run a Specific Script

```bash
nmap --script=http-title
```

Run Multiple Scripts

```bash
nmap --script=http-title,http-headers,http-enum
```

Run an Entire Category

```bash
nmap --script vuln
```

---

# Common NSE Categories

* auth
* broadcast
* brute
* default
* discovery
* dos
* exploit
* external
* fuzzer
* intrusive
* malware
* safe
* version
* vuln

---

# Useful HTTP Scripts

HTTP Title

```bash
nmap --script http-title -p80 TARGET
```

HTTP Headers

```bash
nmap --script http-headers -p80 TARGET
```

HTTP Methods

```bash
nmap --script http-methods -p80 TARGET
```

Directory Enumeration

```bash
nmap --script http-enum -p80 TARGET
```

robots.txt

```bash
nmap --script http-robots.txt -p80 TARGET
```

Server Status

```bash
nmap --script http-server-header -p80 TARGET
```

---

# Vulnerability Scanning

```bash
nmap --script vuln TARGET
```

Safe Vulnerability Scan

```bash
nmap --script "safe and vuln"
```

---

# Output Formats

Normal

```bash
nmap -oN scan.txt TARGET
```

XML

```bash
nmap -oX scan.xml TARGET
```

Grepable

```bash
nmap -oG scan.gnmap TARGET
```

All Formats

```bash
nmap -oA full_scan TARGET
```

---

# Firewall Evasion

Fragment Packets

```bash
nmap -f TARGET
```

Decoys

```bash
nmap -D RND:5 TARGET
```

Spoof Source Port

```bash
nmap --source-port 53 TARGET
```

Random Data Length

```bash
nmap --data-length 32 TARGET
```

---

# Common Workflows

## Initial Recon

```bash
nmap TARGET
```

## Full TCP Scan

```bash
nmap -p- --min-rate 5000 TARGET
```

## Version Detection

```bash
nmap -sV TARGET
```

## Default Enumeration

```bash
nmap -sC -sV TARGET
```

## Aggressive Enumeration

```bash
sudo nmap -A TARGET
```

## HTTP Enumeration

```bash
nmap --script=http-title,http-headers,http-methods,http-enum -p80 TARGET
```

## Vulnerability Scan

```bash
nmap --script vuln TARGET
```

---

# Cheat Sheet

| Purpose            | Command                          |
| ------------------ | -------------------------------- |
| Ping Scan          | `nmap -sn TARGET`                |
| All TCP Ports      | `nmap -p- TARGET`                |
| SYN Scan           | `sudo nmap -sS TARGET`           |
| UDP Scan           | `sudo nmap -sU TARGET`           |
| Version Detection  | `nmap -sV TARGET`                |
| OS Detection       | `sudo nmap -O TARGET`            |
| Default Scripts    | `nmap -sC TARGET`                |
| Aggressive Scan    | `sudo nmap -A TARGET`            |
| HTTP Enumeration   | `nmap --script=http-enum TARGET` |
| Vulnerability Scan | `nmap --script vuln TARGET`      |

---

# Best Practices

* Scan only systems you own or are explicitly authorized to test.
* Start with host discovery before full port scans.
* Use `-p-` when thoroughness is more important than speed.
* Combine `-sC` and `-sV` for efficient initial enumeration.
* Save results with `-oA` for later analysis.
* Verify NSE findings manually; scripts can produce false positives.

---

# References

* Official Nmap Documentation
* Nmap NSE Documentation
* Nmap Network Scanning by Gordon "Fyodor" Lyon

---

Happy Hacking! 🚀

