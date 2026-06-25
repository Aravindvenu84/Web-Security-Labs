# Feroxbuster Enumeration - DVWA

## Objective

The objective of this exercise was to perform web content discovery against a DVWA installation and identify exposed resources, hidden files, and information disclosure issues.

Target:

```text
http://192.168.24.153
```

---

# About Feroxbuster

Feroxbuster is a fast and recursive content discovery tool written in Rust.

It is commonly used during the reconnaissance and enumeration phases of web application testing.

Capabilities include:

- Directory discovery
- File discovery
- Recursive scanning
- Extension brute-forcing
- Link extraction
- Response filtering

Official Repository:

https://github.com/epi052/feroxbuster

---

# Command Used

```bash
feroxbuster -u http://192.168.24.153 \
-w /usr/share/feroxbuster/raft-medium-directories.txt
```

---

# Scan Output

The scan successfully identified multiple directories and files exposed by the web server.

## Screenshot

![Feroxbuster Scan Output]<img width="1754" height="972" alt="Screenshot from 2026-06-11 11-04-27" src="https://github.com/user-attachments/assets/9d0e1547-4c3f-4b63-9b6e-cc56bd15c436" />


---

# Directories Discovered

| Path | Status |
|--------|--------|
| /DVWA | 301 |
| /DVWA/config | 301 |
| /DVWA/database | 301 |
| /DVWA/docs | 301 |
| /DVWA/tests | 301 |
| /DVWA/external | 301 |

---

# Information Disclosure Findings

The enumeration process revealed several sensitive files that should not normally be accessible through a web server.

---

# Exposed Configuration Files

Discovered:

```text
/DVWA/config/config.inc.php.bak
```

The file was accessible without authentication.

Using:

```bash
curl http://192.168.24.153/DVWA/config/config.inc.php.bak
```

the configuration contents were retrieved.

### Information Revealed

```php
db_database = dvwa
db_user = admin
db_password = password
db_port = 3306
```

This exposed backend database configuration details.

## Screenshot

![Configuration Disclosure](<img width="1754" height="972" alt="Screenshot from 2026-06-11 11-04-27" src="https://github.com/user-attachments/assets/7cb21171-a2a8-486e-a06a-b7aac7a31e18" />
)

---

# Database Exposure

Feroxbuster discovered:

```text
/DVWA/database/sqli.db
```

The database file was directly downloadable.

Command used:

```bash
wget http://192.168.24.153/DVWA/database/sqli.db
```

This allowed offline analysis of application data.

## Screenshot

![Database Exposure](Feroxbuster-database-exposure.png)

---

# Database Analysis

The downloaded SQLite database was inspected using:

```bash
sqlite3 sqli.db
```

Tables discovered:

```sql
.tables
```

Output:

```text
guestbook
users
```

Schema inspection:

```sql
.schema
```

revealed application database structure.

---

# User Enumeration

Query executed:

```sql
SELECT * FROM users;
```

Results:

```text
admin
gordonb
1337
pablo
smithy
```

The database contained user information and passwords stored in plaintext.

Examples:

```text
admin : password
gordonb : abc123
pablo : letmein
```

## Screenshot

![User Table](Feroxbuster-user-table.png)

---

# Additional Findings

## Directory Listing Enabled

Multiple directories appeared to allow directory indexing or exposed internal project resources.

Examples:

```text
/DVWA/docs/
/DVWA/database/
/DVWA/tests/
```

This increased information disclosure and assisted reconnaissance.

---

# Security Issues Identified

| Issue | Severity |
|---------|----------|
| Directory Enumeration Success | Informational |
| Backup File Exposure | Medium |
| Configuration Disclosure | High |
| Database File Exposure | High |
| User Enumeration | High |
| Plaintext Password Storage | Critical |

---

# Attack Chain Demonstrated

The following methodology was successfully demonstrated:

```text
Service Discovery
        ↓
Content Discovery
        ↓
Sensitive File Discovery
        ↓
Configuration Disclosure
        ↓
Database Exposure
        ↓
User Enumeration
```

This highlights how reconnaissance alone can reveal significant information before exploitation begins.

---

# Lessons Learned

- Enumeration is one of the most important phases of web application testing.
- Backup files often contain sensitive information.
- Exposed databases can reveal application internals and user data.
- Recursive scanning significantly increases discovery effectiveness.
- Information disclosure can greatly simplify later attack stages.

---

# Commands Used

```bash
feroxbuster -u http://192.168.24.153 \
-w /usr/share/feroxbuster/raft-medium-directories.txt

curl http://192.168.24.153/DVWA/config/config.inc.php.bak

wget http://192.168.24.153/DVWA/database/sqli.db

sqlite3 sqli.db
```

---

# Conclusion

Feroxbuster successfully identified multiple exposed resources within the DVWA application, including configuration files, database files, and user information.

The exercise demonstrated how content discovery tools can reveal valuable intelligence and security weaknesses without exploiting a single vulnerability.
