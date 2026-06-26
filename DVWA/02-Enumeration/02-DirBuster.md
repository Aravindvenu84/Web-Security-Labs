
# DirBuster - Complete Guide

> A practical guide to using DirBuster for web content discovery during authorized penetration testing and security assessments.

---

# Table of Contents

1. Introduction
2. Installation
3. How DirBuster Works
4. User Interface Overview
5. Basic Scan
6. Configuration Options
7. Wordlists
8. Recursive Scanning
9. File Extension Discovery
10. Authentication
11. Status Codes
12. Scan Results
13. Common Workflows
14. DirBuster vs DIRB vs Gobuster vs Feroxbuster
15. Best Practices
16. Cheat Sheet

---

# Introduction

DirBuster is a **GUI-based** web content discovery tool developed by OWASP. It performs dictionary-based and brute-force attacks against web servers to discover hidden directories and files.

Unlike DIRB, which is command-line based, DirBuster provides an interactive graphical interface for configuring and monitoring scans.

It is commonly used during the **Enumeration** phase of a web application penetration test.

---

# Installation

## Kali Linux

```bash
sudo apt install dirbuster
```

Launch:

```bash
dirbuster
```

Or search for **DirBuster** in your desktop applications.

---

# How DirBuster Works

Suppose your wordlist contains:

```
admin
login
backup
config
images
uploads
```

DirBuster generates requests such as:

```
GET /admin
GET /login
GET /backup
GET /config
GET /images
GET /uploads
```

It records any responses indicating that the resource may exist.

---

# User Interface Overview

The main interface includes:

- Target URL
- Wordlist selection
- Thread configuration
- Extension list
- Recursive scanning options
- Start/Stop controls
- Results window

---

# Basic Scan

1. Launch DirBuster.
2. Enter the target URL.

Example:

```
http://192.168.24.226
```

3. Select a wordlist.
4. Click **Start**.

---

# Selecting a Wordlist

Common locations:

```
/usr/share/dirbuster/wordlists/

/usr/share/wordlists/

/usr/share/dirb/wordlists/
```

Popular choices:

```
directory-list-2.3-small.txt

directory-list-2.3-medium.txt

directory-list-2.3-big.txt

common.txt
```

---

# Thread Configuration

Threads determine how many requests are sent simultaneously.

Suggested values:

| Threads | Use Case |
|----------|----------|
| 10 | Slow networks |
| 50 | Normal labs |
| 100 | Fast local networks |
| 200+ | Only when the target can handle the load |

More threads increase scan speed but also increase server load.

---

# Recursive Scanning

Enable:

```
Recursive
```

Example:

```
/admin/

/admin/login/

/admin/images/

/admin/config/

/admin/uploads/
```

This allows DirBuster to continue searching inside newly discovered directories.

---

# Brute Force Mode

DirBuster supports two discovery methods:

## Dictionary Mode

Uses an existing wordlist.

Example:

```
admin

login

uploads

config
```

Recommended for most assessments.

---

## Pure Brute Force Mode

Generates names automatically.

Example:

```
aaa

aab

aac

aad
```

This is significantly slower and rarely used in practice.

---

# File Extension Discovery

Specify extensions to search.

Examples:

```
php

html

txt

bak

zip

old

sql

jsp

aspx
```

Generated requests:

```
admin.php

admin.html

admin.bak

config.zip

backup.old
```

---

# Authentication

If the application requires authentication, include the session information.

Methods include:

- HTTP Basic Authentication
- Cookies
- Custom Headers (if supported through a proxy)

A common workflow is:

1. Authenticate in the browser.
2. Capture the session cookie.
3. Configure DirBuster (or use a proxy like Burp Suite).

---

# Status Codes

Common responses:

| Status | Meaning |
|---------|---------|
| 200 | Resource exists |
| 201 | Created |
| 204 | No Content |
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 307 | Redirect |
| 401 | Authentication Required |
| 403 | Forbidden (resource exists but access is denied) |
| 404 | Not Found |
| 500 | Internal Server Error |

A **403** response often indicates that the resource exists but access is restricted.

---

# Scan Results

Example:

```
/admin               200

/login               200

/uploads             403

/phpmyadmin          301

/config.php          200

/backup.zip          200
```

Each finding should be reviewed manually.

---

# Common Workflow

### Step 1

Launch DirBuster.

---

### Step 2

Target:

```
http://TARGET
```

---

### Step 3

Choose:

```
directory-list-2.3-medium.txt
```

---

### Step 4

Extensions:

```
php

bak

zip

txt

old
```

---

### Step 5

Threads:

```
50
```

---

### Step 6

Enable recursion.

---

### Step 7

Start scan.

---

### Step 8

Investigate discovered resources manually.

---

# Practical Example

Target:

```
http://192.168.24.226
```

Results:

```
/admin

/dvwa

/login.php

/phpinfo.php

/uploads

/backup.zip
```

Next steps:

- Visit each page.
- Check authentication requirements.
- Look for exposed backups.
- Continue manual testing.

---

# DirBuster vs Other Tools

| Feature | DirBuster | DIRB | Gobuster | Feroxbuster |
|----------|-----------|------|-----------|-------------|
| Interface | GUI | CLI | CLI | CLI |
| Recursive | Yes | Yes | Limited | Excellent |
| Multi-threaded | Yes | No | Yes | Yes |
| Speed | Medium | Medium | Fast | Very Fast |
| Beginner Friendly | Excellent | Good | Good | Good |
| Active Development | No | Limited | Yes | Yes |

---

# DirBuster in the Web Attack Chain

```
Reconnaissance
        │
        ▼
Host Discovery (Nmap)
        │
        ▼
Port Scanning
        │
        ▼
Service Detection
        │
        ▼
Web Enumeration
        │
        ├── DirBuster
        ├── DIRB
        ├── Gobuster
        └── Feroxbuster
        ▼
Hidden Resources Found
        │
        ▼
Manual Analysis
        │
        ▼
Vulnerability Assessment
        │
        ▼
Exploitation
```

---

# Best Practices

- Scan only systems you own or are authorized to assess.
- Begin with a small or medium wordlist before using larger ones.
- Enable recursion to uncover nested directories.
- Include common backup extensions (`.bak`, `.zip`, `.old`).
- Review `301` and `403` responses carefully—they often point to valid resources.
- Save scan results for reporting and later analysis.

---

# Cheat Sheet

| Task | Action |
|------|--------|
| Start DirBuster | `dirbuster` |
| Target | `http://TARGET` |
| Default Threads | `50` |
| Enable Recursion | ✓ |
| Wordlist | `directory-list-2.3-medium.txt` |
| Extensions | `php,bak,zip,txt,old` |
| Review | 200, 301, 302, 403 responses |

---

# References

- OWASP DirBuster Project
- OWASP Web Security Testing Guide (WSTG)
- SecLists Wordlists

---

Happy Hunting! 🚀
````
