# Findings Analysis

This document contains the analysis of the Nikto scan results. Each reported finding was reviewed to understand its purpose, potential impact, and whether it represents a confirmed vulnerability, an informational observation, or a possible false positive.

---

# Finding 1 – Access-Control-Allow-Origin: *

### Nikto Output

```text
Retrieved access-control-allow-origin header: *
```

### Description

The application allows cross-origin requests from any origin.

### Risk

Depends on the application's implementation. Public resources may safely use this configuration, while authenticated APIs may expose sensitive data.

### Status

⚠ Requires Manual Verification

---

# Finding 2 – X-Recruiting Header

### Nikto Output

```text
Uncommon header(s) 'x-recruiting'
```

### Description

A custom HTTP response header was identified.

### Risk

Informational.

This is an application-specific header used by OWASP Juice Shop and does not represent a vulnerability.

### Status

Informational

---

# Finding 3 – robots.txt

### Nikto Output

```text
robots.txt contains entries.
```

### Description

The application exposes a robots.txt file.

### Risk

Low

Although robots.txt is publicly accessible by design, it may reveal hidden directories or administrative paths.

### Status

Verified

---

# Finding 4 – Missing Content-Security-Policy

### Description

The application does not define a Content Security Policy (CSP).

### Risk

Medium

The absence of CSP increases the potential impact of Cross-Site Scripting (XSS) vulnerabilities.

### Status

Verified

---

# Finding 5 – Missing Strict-Transport-Security

### Description

The HTTP Strict Transport Security (HSTS) header was not present.

### Risk

Low

HSTS forces browsers to communicate over HTTPS and helps prevent protocol downgrade attacks.

### Status

Expected (HTTP Lab Environment)

---

# Finding 6 – Missing Referrer-Policy

### Description

The application does not define a Referrer Policy.

### Risk

Low

This may result in unnecessary leakage of referrer information.

### Status

Verified

---

# Finding 7 – Missing Permissions-Policy

### Description

The application does not restrict browser features such as camera, microphone, or geolocation.

### Risk

Low

This is a security hardening recommendation.

### Status

Verified

---

# Finding 8 – Exposed Files

Nikto suggested several potentially interesting files:

* `.bash_history`
* `.mysql_history`
* `.psql_history`
* `.sqlite_history`
* `.htpasswd`

### Analysis

These files are only considered security issues if they are actually accessible and expose sensitive information.

Each file must be manually verified before being reported.

### Status

Manual Verification Required

---

# Finding 9 – Public Directories

Nikto identified:

* `/ftp/`
* `/public/`

### Analysis

These directories exist within OWASP Juice Shop for educational purposes.

Their presence alone does not represent a vulnerability.

### Status

Informational

---

# Finding 10 – JSON Files

Nikto reported several JSON resources, including:

* `users.json`
* `accounts.json`
* `login.json`
* `master.json`

### Analysis

The existence of these files does not automatically indicate a vulnerability.

The tester must verify:

* HTTP response code.
* Accessibility.
* Sensitive data exposure.

### Status

Manual Verification Required

---

# Finding 11 – JAMonAdmin.jsp

### Description

Nikto suggested a possible Java Application Monitor interface.

### Analysis

OWASP Juice Shop is built using Node.js rather than Java.

This finding is likely a false positive.

### Status

Likely False Positive

---

# Finding 12 – Missing X-Content-Type-Options

### Description

The application does not define the `X-Content-Type-Options` header.

### Risk

Low

Without this header, browsers may perform MIME type sniffing, which can increase the risk of content interpretation issues.

### Status

Verified

---

# Summary

| Finding                              | Severity      | Status                       |
| ------------------------------------ | ------------- | ---------------------------- |
| CORS (`Access-Control-Allow-Origin`) | Medium*       | Manual Verification Required |
| X-Recruiting Header                  | Informational | Verified                     |
| robots.txt                           | Low           | Verified                     |
| Missing Content-Security-Policy      | Medium        | Verified                     |
| Missing HSTS                         | Low           | Expected in HTTP Lab         |
| Missing Referrer-Policy              | Low           | Verified                     |
| Missing Permissions-Policy           | Low           | Verified                     |
| Exposed History Files                | High*         | Manual Verification Required |
| Public Directories                   | Informational | Verified                     |
| JSON Files                           | Medium*       | Manual Verification Required |
| JAMonAdmin.jsp                       | None          | Likely False Positive        |
| Missing X-Content-Type-Options       | Low           | Verified                     |

> **Note:** Severity marked with `*` depends on successful manual verification. A Nikto finding should never be reported as a confirmed vulnerability without validating it.
