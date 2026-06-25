# DNS and WHOIS Reconnaissance Analysis

## Objective

The objective of this exercise is to learn how to perform passive reconnaissance using WHOIS, DNS queries, and subdomain enumeration tools. These techniques are commonly used during the reconnaissance phase of penetration testing, bug bounty hunting, and security assessments.

---

# Tool 1: WHOIS

## Introduction

WHOIS is a protocol used to retrieve information about a registered domain name. It provides details about:

- Domain owner (Registrant)
- Registrar
- Creation date
- Expiration date
- Name servers
- Domain status

### Command Used

```bash
whois google.com
```

---

## Screenshot

> <img width="1126" height="573" alt="Screenshot from 2026-06-25 12-35-22" src="https://github.com/user-attachments/assets/a5330bd2-81f0-4afa-9a89-b9da10045594" />


---

## Analysis of Output

### Domain Name

```text
Domain Name: GOOGLE.COM
```

This indicates that the WHOIS query was performed against the domain **google.com**.

---

### Registrar

```text
Registrar: MarkMonitor Inc.
```

A registrar is a company authorized to manage domain registrations.

Google uses **MarkMonitor**, a registrar that specializes in protecting large enterprise brands from domain hijacking and abuse.

---

### Creation Date

```text
Creation Date: 1997-09-15
```

This is the date when Google registered the domain.

Observation:

- Google.com has existed since 1997.
- The domain is almost three decades old.
- Older domains generally have higher trust and reputation.

---

### Expiration Date

```text
Registry Expiry Date: 2028-09-14
```

This indicates when the current registration period ends.

Observation:

- The domain is registered several years into the future.
- Large organizations often renew domains for multiple years.

---

### Updated Date

```text
Updated Date: 2024-08-02
```

This shows when registration information was last modified.

Possible reasons:

- Contact updates
- Registrar changes
- DNS updates
- Security modifications

---

### Registrant Organization

```text
Registrant Organization: Google LLC
Registrant Country: US
```

This confirms that the domain belongs to Google LLC and is registered in the United States.

---

### Name Servers

```text
NS1.GOOGLE.COM
NS2.GOOGLE.COM
NS3.GOOGLE.COM
NS4.GOOGLE.COM
```

Name servers are responsible for answering DNS queries for a domain.

Google operates its own DNS infrastructure rather than relying on third-party providers.

---

### Domain Status

```text
clientTransferProhibited
clientDeleteProhibited
clientUpdateProhibited

serverTransferProhibited
serverDeleteProhibited
serverUpdateProhibited
```

These status codes are security controls that protect the domain.

#### clientTransferProhibited

Prevents unauthorized transfer of the domain to another registrar.

#### clientDeleteProhibited

Prevents accidental or malicious deletion.

#### clientUpdateProhibited

Prevents unauthorized modifications.

#### serverTransferProhibited

Additional protection enforced by the registry.

---

### DNSSEC

```text
DNSSEC: unsigned
```

DNSSEC adds cryptographic validation to DNS records.

Observation:

- Google's WHOIS currently reports DNSSEC as unsigned.
- DNSSEC status should always be verified directly from DNS records.

---

## Security Importance of WHOIS

WHOIS information can help security analysts:

- Identify domain ownership
- Investigate phishing domains
- Determine domain age
- Identify hosting and DNS providers
- Support OSINT investigations

---

# Tool 2: DIG (Domain Information Groper)

## Introduction

DIG is a DNS lookup utility used to query DNS servers and retrieve DNS records.

It is one of the most important tools for:

- DNS troubleshooting
- Security assessments
- Infrastructure mapping
- Reconnaissance

---

# A Record Lookup

## Command

```bash
dig www.google.com A
```

---

## Screenshot

> <img width="1126" height="573" alt="image" src="https://github.com/user-attachments/assets/4dbad5f5-83fa-4d9c-8be6-624b849953d3" />



---

## Analysis

### Question Section

```text
www.google.com IN A
```

The DNS resolver is requesting IPv4 addresses for www.google.com.

---

### Answer Section

```text
142.251.156.119
142.251.151.119
142.251.157.119
142.251.153.119
142.251.152.119
142.251.155.119
142.251.150.119
142.251.154.119
```

Google returns multiple IPv4 addresses.

This is called:

### Load Balancing

Traffic is distributed across multiple servers.

Benefits:

- High availability
- Improved performance
- Redundancy
- Global scalability

---

### TTL Value

```text
58
```

TTL = Time To Live

Meaning:

DNS resolvers may cache this response for 58 seconds before requesting fresh data.

---

### Query Time

```text
19 msec
```

The DNS server responded within 19 milliseconds.

This indicates excellent DNS performance.

---

# AAAA Record Lookup

## Command

```bash
dig www.google.com AAAA
```

---

## Screenshot

> <img width="1126" height="596" alt="Screenshot from 2026-06-25 12-32-47" src="https://github.com/user-attachments/assets/8994063f-3e28-4625-ac5a-f2894bf8a16a" />


---

## Analysis

AAAA records contain IPv6 addresses.

Example:

```text
2001:4860:4829:7700::
2001:4860:4827:7700::
```

Observation:

- Google fully supports IPv6.
- Multiple IPv6 addresses provide redundancy and load balancing.

---

# Name Server Lookup

## Command

```bash
dig google.com NS
```

---

## Screenshot

> <img width="1100" height="700" alt="Screenshot from 2026-06-25 12-38-55" src="https://github.com/user-attachments/assets/e0e11db1-0659-47e6-b1f1-9415eeead112" />


---

## Analysis

Output:

```text
ns1.google.com
ns2.google.com
ns3.google.com
ns4.google.com
```

These servers manage Google's DNS records.

Importance:

If DNS infrastructure becomes unavailable, users cannot resolve the domain.

Google uses multiple authoritative name servers for fault tolerance.

---

# Mail Exchange (MX) Record Lookup

## Command

```bash
dig google.com MX
```

---

## Screenshot

> <img width="1099" height="429" alt="Screenshot from 2026-06-25 12-39-36" src="https://github.com/user-attachments/assets/03c0ec61-3aa4-4a22-9acc-ad4addbce386" />


---

## Analysis

Output:

```text
10 smtp.google.com
```

MX records specify mail servers responsible for receiving email.

### Priority

```text
10
```

Lower values indicate higher priority.

Since only one MX record exists:

```text
smtp.google.com
```

all mail delivery is directed there.

---

## Security Importance

MX records can reveal:

- Email infrastructure
- Email providers
- Potential phishing targets

They are frequently used during OSINT investigations.

---

# Incorrect DIG Usage

## Command

```bash
dig https://www.google.com
```

---

## Screenshot

> <img width="1099" height="429" alt="Screenshot from 2026-06-25 12-40-11" src="https://github.com/user-attachments/assets/f017310f-8a0c-4ed6-89c4-69b438adb26a" />


---

## Result

```text
status: NXDOMAIN
```

---

## Why It Failed

DNS only resolves hostnames.

Correct:

```text
www.google.com
```

Incorrect:

```text
https://www.google.com
```

The protocol (`https://`) is not part of the hostname.

DIG attempted to resolve:

```text
https://www.google.com
```

as a domain name, which does not exist.

Therefore:

```text
NXDOMAIN
```

means:

**Non-Existent Domain**

---

# Understanding DIG Output

## Status Codes

### NOERROR

```text
status: NOERROR
```

The query succeeded.

---

### NXDOMAIN

```text
status: NXDOMAIN
```

The requested DNS record does not exist.

---

## DNS Server Used

```text
SERVER: 127.0.0.53
```

This is Ubuntu's local DNS resolver service:

```text
systemd-resolved
```

It forwards DNS requests to upstream DNS servers.

---

# Tool 3: Subfinder

## Introduction

Subfinder is a passive subdomain enumeration tool developed by ProjectDiscovery.

It gathers subdomains from:

- Certificate Transparency logs
- Search engines
- OSINT databases
- DNS datasets

---

## Command

```bash
subfinder -d facebook.com
```

---

## Screenshot

> Insert Screenshot Here

---

## Analysis

Subfinder discovered:

```text
51891 subdomains
```

This demonstrates the massive infrastructure used by Facebook.

Examples:

```text
whois.dns.facebook.com
origami.facebook.com
cg.facebook.com
int.c.facebook.com
```

---

## Interesting Findings

### whois.dns.facebook.com

Likely related to DNS infrastructure.

---

### origami.facebook.com

Associated with Facebook's internal engineering projects.

---

### int.c.facebook.com

Appears to be an internal service endpoint.

---

### edge-* Hosts

Examples:

```text
edge-fblite-tcp-p1-shv-02-fra3.facebook.com
edge-star-mini6-shv-01-ber1.facebook.com
```

These are edge servers used for:

- Content delivery
- API traffic
- Mobile application support

---

## Security Importance of Subdomain Enumeration

Subdomain enumeration helps identify:

- Hidden services
- Development environments
- Internal portals
- Cloud resources
- Forgotten assets

Example:

```text
dev.company.com
staging.company.com
vpn.company.com
```

These systems often become attack surfaces.

---

# Conclusion

This exercise demonstrated three critical reconnaissance techniques:

| Tool | Purpose |
|--------|---------|
| WHOIS | Domain registration information |
| DIG | DNS record enumeration |
| Subfinder | Subdomain discovery |

Key findings:

- Google's domain is registered through MarkMonitor.
- Google operates its own DNS infrastructure.
- Google uses multiple IPv4 and IPv6 addresses for load balancing.
- Google's mail service is handled through smtp.google.com.
- Facebook exposes tens of thousands of discoverable subdomains through passive OSINT sources.

These tools form the foundation of passive reconnaissance and are essential skills for penetration testing, bug bounty hunting, OSINT investigations, and defensive security analysis.
