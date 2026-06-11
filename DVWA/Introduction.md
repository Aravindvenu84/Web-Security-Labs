# Damn Vulnerable Web Application (DVWA)

## Overview

Damn Vulnerable Web Application (DVWA) is a deliberately insecure PHP/MySQL web application designed for security professionals, students, and enthusiasts to learn and practice web application security in a safe environment.

DVWA contains intentionally vulnerable modules that allow users to understand how common web vulnerabilities work, how they can be exploited, and how they can be mitigated.

The platform is widely used for learning:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- File Inclusion
- File Upload Vulnerabilities
- CSRF
- Authentication Weaknesses
- Brute Force Attacks
- Security Misconfigurations

---

## Purpose of This Lab

The goal of this project is to document the process of learning web application security through practical testing of DVWA using various security tools and manual techniques.

Each tool and methodology will be documented separately throughout this repository.

Topics covered include:

- Enumeration
- Information Gathering
- Web Content Discovery
- Traffic Interception
- Vulnerability Assessment
- Exploitation
- Security Analysis

---

## Environment

### Attacker Machine

```text
Ubuntu Linux
```

### Target Machine

```text
Kali Linux
```

### Web Server

```text
Apache 2.4.67
```

### Target Application

```text
DVWA
```

### Access Method

```text
http://<target-ip>/DVWA
```

---

# DVWA Login Page

The login page is the entry point to the application.

Default credentials are typically:

```text
Username: admin
Password: password
```

## Screenshot

![DVWA Login Page](DVWA-login-page.png)

---

# DVWA Interface

After authentication, DVWA provides access to multiple intentionally vulnerable modules.

These modules are grouped according to vulnerability category and security level.

Examples include:

- SQL Injection
- XSS (Reflected)
- XSS (Stored)
- Command Injection
- File Upload
- File Inclusion
- Brute Force
- CSRF

## Screenshot

![DVWA Dashboard](DVWA-dashboard.png)

---

# Security Levels

DVWA allows testing vulnerabilities under different security configurations.

Available levels:

```text
Low
Medium
High
Impossible
```

This helps demonstrate how security controls affect attack success.

---

# Learning Objectives

This DVWA lab will be used to:

- Learn web application reconnaissance
- Understand HTTP communication
- Practice using security tools
- Identify vulnerabilities
- Study application behavior
- Improve web penetration testing methodology

---

# Tools Planned

The following tools will be explored during this project:

- Nmap
- Gobuster
- Feroxbuster
- Burp Suite
- WhatWeb
- Nikto
- SQLMap
- Hydra
- Curl
- Manual Testing Techniques

Additional tools may be added as the project progresses.

---

# Disclaimer

This project is conducted entirely within a controlled lab environment using DVWA, an intentionally vulnerable application created for educational purposes.

No testing is performed against systems without authorization.
