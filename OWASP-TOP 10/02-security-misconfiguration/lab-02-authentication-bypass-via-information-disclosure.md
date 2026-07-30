# Lab 03 - Authentication Bypass via Information Disclosure

## Lab Information

| Field | Value |
|-------|-------|
| Category | Broken Access Control |
| Vulnerability | Authentication Bypass via Information Disclosure |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status |  Solved |

---

## Objective

Gain unauthorized access to the administrator panel by exploiting information disclosure and spoofing a trusted IP address.

---

## Vulnerability Overview

Applications sometimes grant privileged access based on the client's IP address. If the application trusts client-controlled HTTP headers without proper validation, an attacker may be able to impersonate a trusted host and bypass authentication controls.

In this lab, diagnostic information leaked through the HTTP `TRACE` method revealed the internal authorization mechanism, allowing the required header to be forged.

---

## Lab Scenario

The administrator panel was initially inaccessible to normal users.

Further testing revealed that access was allowed either for authenticated administrators or for requests originating from the localhost address (`127.0.0.1`).

Using the HTTP `TRACE` method exposed an internal request header used for IP-based authorization. By spoofing this header, the application incorrectly treated the request as coming from the local machine and granted administrator access.

---

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Burp Match and Replace
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Attempt to Access the Admin Panel

Browsed directly to:

```
/admin
```

The application denied access and indicated that the administrator panel was only available to administrator accounts or requests originating from localhost.

**Screenshot**

```
screenshots/lab03-admin-blocked.png
```

---

### Step 2 – Inspect the Application Using TRACE

Sent the following request to Burp Repeater:

```
TRACE /admin HTTP/1.1
```

The response reflected the original request and disclosed an internal authorization header:

```
X-Custom-IP-Authorization
```

This revealed that the application relied on this header to determine whether a request originated from the localhost IP address.

**Screenshot**

```
screenshots/lab03-trace-response.png
```

---

### Step 3 – Configure Burp Match and Replace

Configured Burp Suite to automatically add the following request header to every outgoing request:

```
X-Custom-IP-Authorization: 127.0.0.1
```

This caused all subsequent requests to appear as if they originated from the local machine.

**Screenshot**

```
screenshots/lab03-match-replace-rule.png
```

---

### Step 4 – Access the Admin Panel

Reloaded the application after enabling the Match and Replace rule.

The application granted access to the administrator interface without requiring administrator credentials.

**Screenshot**

```
screenshots/lab03-admin-access.png
```

---

### Step 5 – Delete the Target User

Used the administrator interface to delete the user **carlos**, completing the lab.

**Screenshot**

```
screenshots/lab03-lab-solved.png
```

---

## Result

By exploiting information disclosure and spoofing the trusted IP authorization header, administrative access was obtained without authenticating as an administrator.

The lab was successfully completed.

---

# Impact

Successful exploitation could allow an attacker to:

- Bypass authentication controls
- Gain unauthorized administrative access
- Perform privileged administrative actions
- Delete or modify user accounts
- Compromise application integrity

---

# Root Cause

The application disclosed internal authorization details through the HTTP `TRACE` method and trusted a client-controlled HTTP header to determine whether requests originated from localhost.

Because the header could be forged, the server incorrectly granted administrative privileges.

---

# Remediation

- Disable the HTTP `TRACE` method unless it is explicitly required.
- Never trust client-supplied headers for authentication or authorization decisions.
- Validate the client's IP address using server-side network information.
- Enforce authentication and authorization on every privileged request.
- Remove unnecessary diagnostic information from server responses.

---

# Key Learning

This lab demonstrates how seemingly harmless information disclosure can expose internal security mechanisms. Once these mechanisms become visible, attackers may be able to manipulate trusted headers and bypass authentication controls.

Security decisions should always rely on trusted server-side information rather than values supplied by the client.

---

# References

- OWASP Top 10 2025 – Broken Access Control
- PortSwigger Web Security Academy – Authentication Bypass via Information Disclosure
