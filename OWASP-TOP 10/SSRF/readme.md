# SSRF Security Assessment

## Overview

This repository documents the identification and exploitation of a **Server-Side Request Forgery (SSRF)** vulnerability discovered during a web application security assessment.

The vulnerability was identified in the product stock checking functionality, where a user-controlled `stockapi` parameter influences the destination of a server-side request.

Testing was performed using **Burp Suite** to intercept, modify and replay HTTP requests.

---

## Finding

| Vulnerability                      | Severity | Endpoint         | Parameter  |
| ---------------------------------- | -------- | ---------------- | ---------- |
| Server-Side Request Forgery (SSRF) | High     | `/product/stock` | `stockapi` |

---

## What Was Tested

The assessment focused on determining whether the `stockapi` parameter could be manipulated to make the application server communicate with unintended destinations.

The testing demonstrated:

* Identification of the server-side request through Burp Suite.
* Manipulation of the `stockapi` parameter.
* Blocking of `localhost` and `127.0.0.1`.
* Bypass of the localhost restriction using `127.1`.
* Access to internal functionality.
* Bypass of the `/admin` restriction using double URL encoding.
* Access to a delete operation through the SSRF.

---

## Exploitation Flow

```text
Stock Check
     ↓
Capture Request
     ↓
Identify stockapi
     ↓
Modify Destination
     ↓
localhost / 127.0.0.1 → Blocked
     ↓
127.1 → Accepted
     ↓
127.1/admin → Blocked
     ↓
127.1/%2561dmin → Bypass
     ↓
Internal Admin Functionality
     ↓
Delete Functionality
```

---

## Key Requests

### Loopback Filter Bypass

```text
127.1
```

### Admin Path Encoding Bypass

```text
127.1/%2561dmin
```

### Impact Demonstration

```text
127.1/%2561dmin/delete?username=carlos
```

> Testing was performed in an authorized security-testing environment.

---

## Impact

The identified SSRF can allow an attacker to:

* Force the server to make requests to unintended URLs.
* Access internal resources.
* Reach resources that are not directly accessible externally.
* Perform functionality available to an internal user.
* Potentially disclose internal information.
* Potentially disclose internal infrastructure information.
* Access forbidden resources.

---

## Root Cause

The application trusts a destination controlled by the user through the `stockapi` parameter.

The existing input validation is insufficient and can be bypassed using alternative loopback addressing and URL encoding techniques.

---

## Recommended Remediation

* Do not trust user-controlled destinations.
* Validate all destination URLs server-side.
* Prefer an allowlist of approved destinations.
* Block access to internal and restricted resources.
* Prevent requests to loopback and other unauthorized internal destinations.
* Validate the actual resolved destination rather than relying only on string matching.
* Do not rely solely on blacklist-based filtering.
* Return `403 Forbidden` when access to a restricted internal resource is attempted.

---

## Tools Used

* **Burp Suite**
* Web browser
* HTTP request manipulation and analysis

---

## Repository Structure

```text
ssrf-security-assessment/
│
├── README.md
├── report.md
│
└── screenshots/
    ├── ssrf-normal-request.png
    ├── ssrf-stockapi-parameter.png
    ├── ssrf-localhost-filter.png
    ├── ssrf-127-1-bypass.png
    ├── ssrf-admin-bypass.png
    └── ssrf-impact-delete-user.png
```

---

## Detailed Report

For the complete vulnerability analysis, proof of concept, impact, root cause and remediation:

**[View `report.md`](report.md)**

---

## Disclaimer

This repository is intended for authorized security testing, education and research.

The techniques documented here should only be used against systems for which you have explicit permission to perform security testing.

