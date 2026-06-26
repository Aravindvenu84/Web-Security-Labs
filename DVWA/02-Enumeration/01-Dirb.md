````markdown
# DIRB - Complete Guide

> A practical guide to using DIRB for web content discovery during authorized penetration testing and security assessments.

---

# Table of Contents

1. Introduction
2. Installation
3. Basic Syntax
4. How DIRB Works
5. Default Wordlists
6. Basic Usage
7. Common Options
8. Status Codes
9. Custom Wordlists
10. File Extension Discovery
11. Authentication
12. HTTPS Scanning
13. Output Options
14. Common Workflows
15. Comparison with Other Tools
16. Best Practices
17. Cheat Sheet

---

# Introduction

DIRB is a command-line web content scanner that performs dictionary-based attacks against web servers to discover hidden resources.

DIRB works by requesting paths generated from a wordlist and analyzing HTTP responses.

It helps discover:

- Hidden directories
- Hidden files
- Backup files
- Administrative panels
- Login pages
- API endpoints
- Configuration files

DIRB is commonly used during the **Enumeration** phase of a penetration test, after initial reconnaissance has identified a web service.

---

# Installation

## Kali Linux

```bash
sudo apt install dirb
```

## Ubuntu

```bash
sudo apt install dirb
```

Verify installation

```bash
dirb
```

---

# Basic Syntax

```bash
dirb <URL> [WORDLIST] [OPTIONS]
```

Example

```bash
dirb http://192.168.24.226
```

---

# How DIRB Works

Example:

Suppose the wordlist contains:

```
admin
login
backup
images
uploads
config
```

DIRB sends requests like:

```
GET /admin
GET /login
GET /backup
GET /images
GET /uploads
GET /config
```

If the server returns:

- 200 OK
- 301 Redirect
- 302 Redirect
- 403 Forbidden

DIRB reports the resource.

---

# Default Wordlists

DIRB includes several built-in wordlists.

Location:

```bash
ls /usr/share/dirb/wordlists/
```

Common wordlists:

```
common.txt
big.txt
small.txt
vulns/
stress/
extensions_common.txt
```

---

# Basic Scan

```bash
dirb http://TARGET
```

DIRB automatically uses:

```
/usr/share/dirb/wordlists/common.txt
```

---

# Specify a Custom Wordlist

```bash
dirb http://TARGET /path/to/wordlist.txt
```

Example

```bash
dirb http://TARGET /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

---

# Common Options

## Ignore SSL Certificate Errors

```bash
dirb https://TARGET -k
```

---

## Silent Mode

```bash
dirb http://TARGET -S
```

---

## Show Only Existing Pages

```bash
dirb http://TARGET -f
```

---

## Do Not Stop on Warnings

```bash
dirb http://TARGET -w
```

---

## Save Results

```bash
dirb http://TARGET -o results.txt
```

---

## Add a Delay Between Requests

```bash
dirb http://TARGET -z 100
```

100 milliseconds between requests.

Useful to reduce load on the server.

---

## Recursive Scan

DIRB recursively scans discovered directories by default.

Example:

```
/admin/

/admin/login/

/admin/images/

/admin/config/
```

---

# File Extension Discovery

Search for PHP files

```bash
dirb http://TARGET -X .php
```

Multiple extensions

```bash
dirb http://TARGET -X .php,.bak,.txt
```

Common extensions

```
.php
.html
.asp
.aspx
.jsp
.bak
.old
.zip
.tar
.sql
.txt
```

---

# Authentication

HTTP Basic Authentication

```bash
dirb http://TARGET -u admin:password
```

---

# Custom Headers

Add a header

```bash
dirb http://TARGET -H "User-Agent: Mozilla/5.0"
```

Add cookies

```bash
dirb http://TARGET -H "Cookie: PHPSESSID=SESSIONID"
```

Useful for authenticated testing.

---

# HTTPS

Scan HTTPS site

```bash
dirb https://TARGET
```

Ignore certificate validation

```bash
dirb https://TARGET -k
```

---

# HTTP Status Codes

DIRB commonly reports:

| Code | Meaning |
|------|---------|
| 200 | Resource exists |
| 201 | Created |
| 204 | No Content |
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 307 | Redirect |
| 401 | Authentication Required |
| 403 | Forbidden (resource exists but access denied) |
| 500 | Internal Server Error |

A **403 Forbidden** response is often valuable because it indicates the path exists.

---

# Output Formats

Save results

```bash
dirb http://TARGET -o results.txt
```

Example output

```
+ http://TARGET/admin (CODE:200)

+ http://TARGET/login (CODE:200)

+ http://TARGET/uploads (CODE:403)

+ http://TARGET/config (CODE:301)
```

---

# Common Workflows

## Initial Enumeration

```bash
dirb http://TARGET
```

---

## Use a Larger Wordlist

```bash
dirb http://TARGET /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

---

## Search for PHP Files

```bash
dirb http://TARGET -X .php
```

---

## Search for Backup Files

```bash
dirb http://TARGET -X .bak,.zip,.old
```

---

## Authenticated Scan

```bash
dirb http://TARGET -H "Cookie: PHPSESSID=YOUR_SESSION"
```

---

## Save Results

```bash
dirb http://TARGET -o dirb_scan.txt
```

---

# DIRB vs Gobuster vs Feroxbuster

| Feature | DIRB | Gobuster | Feroxbuster |
|----------|------|-----------|-------------|
| Language | C | Go | Rust |
| Speed | Medium | Fast | Very Fast |
| Recursive | Yes | Limited | Excellent |
| Extensions | Yes | Yes | Yes |
| Multi-threaded | No | Yes | Yes |
| Modern Development | No | Yes | Yes |

---

# DIRB in the Web Attack Chain

```
Reconnaissance
        │
        ▼
Nmap
        │
        ▼
Service Detection
        │
        ▼
Web Enumeration
        │
        ├── DIRB
        ├── Gobuster
        ├── Feroxbuster
        ▼
Hidden Directories Found
        │
        ▼
Manual Analysis
        │
        ▼
Vulnerability Discovery
        │
        ▼
Exploitation
```

---

# Best Practices

- Scan only systems you own or have explicit permission to test.
- Start with the default wordlist before trying larger ones.
- Investigate HTTP 403 responses—they often indicate real resources with restricted access.
- Test for common backup file extensions such as `.bak`, `.old`, and `.zip`.
- Save scan results for documentation.
- Validate interesting findings manually in a browser or proxy.

---

# Cheat Sheet

| Purpose | Command |
|----------|---------|
| Basic Scan | `dirb http://TARGET` |
| Custom Wordlist | `dirb http://TARGET WORDLIST` |
| HTTPS | `dirb https://TARGET` |
| Ignore SSL Errors | `dirb https://TARGET -k` |
| PHP Files | `dirb http://TARGET -X .php` |
| Backup Files | `dirb http://TARGET -X .bak,.zip,.old` |
| Add Cookie | `dirb http://TARGET -H "Cookie: PHPSESSID=..."` |
| Add Header | `dirb http://TARGET -H "User-Agent: Mozilla/5.0"` |
| Delay Requests | `dirb http://TARGET -z 100` |
| Save Output | `dirb http://TARGET -o results.txt` |

---

# References

- Official DIRB documentation
- Kali Linux package documentation
- OWASP Web Security Testing Guide (WSTG)

---

Happy Hunting! 🚀
````
