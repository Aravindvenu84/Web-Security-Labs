# HTTrack Reconnaissance and Offline Analysis on DVWA

## Introduction

### What is HTTrack?

HTTrack is an open-source website mirroring tool that allows users to download websites from the Internet to a local directory. It recursively builds all directories, retrieves HTML, images, CSS, and other resources, and reconstructs the website structure for offline browsing.

In cybersecurity and penetration testing, HTTrack can be used during the reconnaissance phase to:

* Discover website structure
* Download publicly accessible resources
* Analyze HTML, CSS, and JavaScript files offline
* Identify forms and input parameters
* Examine authentication mechanisms
* Search for hidden endpoints and sensitive information

This exercise demonstrates the use of HTTrack against a local DVWA (Damn Vulnerable Web Application) instance.

---

# Objective

The objective of this exercise was to:

1. Mirror a DVWA website using HTTrack.
2. Analyze the downloaded content.
3. Identify forms and user input fields.
4. Investigate authentication mechanisms.
5. Search for JavaScript files and API endpoints.
6. Examine cookies and session information.

---

# Mirroring the Website

### Command Used

```bash
httrack http://192.168.24.95/DVWA/ -O DVWA-mirror
```

### Explanation

| Option                     | Description                                      |
| -------------------------- | ------------------------------------------------ |
| httrack                    | Launches HTTrack                                 |
| http://192.168.24.95/DVWA/ | Target website                                   |
| -O DVWA-mirror             | Stores mirrored content in DVWA-mirror directory |

---

## Screenshot

**[Add Screenshot Here]**

---

# Viewing the Downloaded Structure

### Command

```bash
tree DVWA-mirror
```

### Purpose

Displays the directory structure of the mirrored website.

### Output Summary

```text
DVWA-mirror
├── 192.168.24.95
│   └── DVWA
│       ├── dvwa
│       │   ├── css
│       │   └── images
│       ├── index.html
│       └── login.html
```

### Findings

The mirror contains:

* Login page
* CSS resources
* Login logo image

Authenticated sections were not downloaded.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 1: Identifying HTML Forms

### Command

```bash
grep -Ri "<form" .
```

### Purpose

Searches all downloaded files for HTML forms.

### Output

```html
<form action="http://192.168.24.95/DVWA/login.php" method="post">
```

### Analysis

A login form was identified.

Information gathered:

| Item   | Value     |
| ------ | --------- |
| Action | login.php |
| Method | POST      |

This indicates that user credentials are submitted to the login.php endpoint.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 2: Discovering Input Fields

### Command

```bash
grep -Ri "<input" .
```

### Purpose

Identifies user input fields accepted by the application.

### Output

```html
<input type="text" name="username">

<input type="password" name="password">

<input type="submit" value="Login">

<input type='hidden' name='user_token'>
```

### Findings

The following parameters were discovered:

| Parameter  | Type     |
| ---------- | -------- |
| username   | Text     |
| password   | Password |
| user_token | Hidden   |

---

## Security Observation

The presence of a hidden field named:

```html
user_token
```

suggests the application uses anti-CSRF protection.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 3: Investigating Login Functionality

### Command

```bash
grep -Ri "login" .
```

### Purpose

Searches for login-related functionality within the mirrored files.

### Findings

The following resources were discovered:

```text
login.php
login.css
login_logo.png
```

The login page appears to be the main entry point of the application.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 4: Searching for JavaScript Files

### Command

```bash
find . -name "*.js"
```

### Purpose

Locates JavaScript files that may contain hidden endpoints or application logic.

### Result

```text
No JavaScript files found
```

### Analysis

The mirrored content only included the login page and supporting resources.

No JavaScript resources were downloaded.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 5: Searching for API Endpoints

### Commands

```bash
grep -R "/api" .
```

```bash
grep -Ri "ajax" .
```

### Purpose

Identifies API endpoints and AJAX functionality.

### Result

```text
No matches found
```

### Analysis

No API references were discovered in the downloaded content.

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 6: Searching for Sensitive Keywords

### Commands

```bash
grep -Ri "admin" .
```

```bash
grep -Ri "password" .
```

```bash
grep -Ri "token" .
```

```bash
grep -Ri "csrf" .
```

```bash
grep -Ri "session" .
```

### Purpose

Searches for potentially sensitive functionality and security mechanisms.

### Findings

#### Password Parameter

```html
<input type="password" name="password">
```

#### Hidden Security Token

```html
<input type='hidden' name='user_token'>
```

No references were found for:

* admin
* csrf
* session

---

## Screenshot

**[Add Screenshot Here]**

---

# Exercise 7: Examining Cookies

### Command

```bash
cat cookies.txt
```

### Purpose

Displays cookies stored by HTTrack during the mirroring process.

### Output

```text
security=impossible

PHPSESSID=2d3d94cbec16158399fa7899916774dc

SameSite=Strict
```

### Analysis

#### security=impossible

DVWA security level was configured to:

```text
Impossible
```

#### PHPSESSID

Represents the active PHP session identifier.

#### SameSite=Strict

Provides protection against some cross-site request forgery attacks.

---

## Screenshot

**[Add Screenshot Here]**

---

# Key Findings

| Finding                     | Result |
| --------------------------- | ------ |
| Login Form Found            | Yes    |
| Username Field Found        | Yes    |
| Password Field Found        | Yes    |
| Hidden Security Token Found | Yes    |
| JavaScript Files Found      | No     |
| API Endpoints Found         | No     |
| Session Cookie Found        | Yes    |
| CSS Resources Found         | Yes    |
| Image Resources Found       | Yes    |

---

# Conclusion

HTTrack successfully mirrored the publicly accessible portion of the DVWA application and allowed offline analysis of the login page and related resources.

The exercise demonstrated how HTTrack can be used during reconnaissance to:

* Discover forms
* Identify user input parameters
* Examine authentication mechanisms
* Inspect cookies and session information
* Analyze website resources offline

Since the mirror was performed before authentication, protected DVWA modules such as SQL Injection, XSS, File Upload, and Command Injection sections were not downloaded. An authenticated crawl would be required to access and analyze those components.
