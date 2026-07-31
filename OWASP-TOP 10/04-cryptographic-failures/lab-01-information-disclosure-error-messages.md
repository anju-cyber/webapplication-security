# Lab 01 - Information Disclosure Through Error Messages

## Lab Information

| Field | Value |
|-------|-------|
| Category | Security Misconfiguration |
| Vulnerability | Information Disclosure |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify sensitive information disclosed through verbose error messages and determine the framework version used by the application.

---

## Vulnerability Overview

Applications should never expose detailed internal error messages to end users. When exceptions are not handled properly, the server may reveal sensitive information such as framework versions, source code locations, server paths, database details, or stack traces.

Attackers can use this information during reconnaissance to identify known vulnerabilities and plan further attacks.

---

## Lab Scenario

The application displays product information using a `productId` parameter.

By submitting an unexpected data type instead of a valid integer, the application throws an unhandled exception and returns a verbose stack trace. The error message discloses the exact framework and version running on the server.

---

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the lab homepage and selected one of the available product pages.

**Screenshot**

```
screenshots/lab01-home_page.png
```

---

### Step 2 – Inspect the Request

Captured the request in Burp Suite and reviewed the HTTP history.

The product page used the following parameter:

```
productId
```

**Screenshot**

```
screenshots/lab01-burp_history.png
```

---

### Step 3 – Send the Request to Repeater

Sent the product request to Burp Repeater for further testing.

Modified the request parameter from a valid integer to an unexpected string value.

Example:

```
GET /product?productId=abc
```

**Screenshot**

```
screenshots/lab01-request_modification.png
```

---

### Step 4 – Trigger an Exception

Submitted the modified request.

The application generated an unhandled exception and returned a detailed stack trace instead of a generic error page.

**Screenshot**

```
screenshots/lab01-unknown_input.png
```

---

### Step 5 – Identify the Framework Version

Reviewed the stack trace and identified the framework version disclosed by the server.

**Disclosed Information**

```
Apache Struts 2 2.3.31
```

**Screenshot**

```
screenshots/lab01-version_disclose.png
```

---

### Step 6 – Submit the Solution

Submitted the disclosed framework version to complete the lab successfully.

**Screenshot**

```
screenshots/lab01-lab_solved.png
```

---

## Result

An unhandled exception exposed a detailed stack trace containing the exact framework and version used by the application.

The disclosed information was sufficient to solve the lab.

---

# Impact

Successful exploitation could allow an attacker to:

- Identify the application framework
- Determine the exact software version
- Search for publicly known vulnerabilities
- Gather intelligence for targeted attacks
- Improve the effectiveness of further exploitation attempts

---

# Root Cause

The application failed to handle invalid input gracefully and returned verbose exception details directly to the client.

Instead of displaying a generic error message, the server disclosed internal implementation details through a stack trace.

---

# Remediation

- Replace detailed error pages with generic user-friendly messages.
- Log technical exception details on the server instead of exposing them to users.
- Implement proper exception handling throughout the application.
- Disable debug mode in production environments.
- Keep frameworks and dependencies updated.

---

# Key Learning

This lab demonstrates that even simple error messages can leak valuable reconnaissance information.

Although no direct compromise occurred, revealing the exact framework version significantly reduces an attacker's effort when searching for publicly known vulnerabilities affecting that software.

---

# References

- OWASP Top 10 2025 – Security Misconfiguration
- PortSwigger Web Security Academy – Information Disclosure in Error Messages
