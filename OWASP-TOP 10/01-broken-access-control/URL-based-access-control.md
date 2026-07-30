# Lab 02 - URL-Based Access Control Bypass Using X-Original-URL Header

## Lab Information

| Field | Value |
|-------|-------|
| Category | Broken Access Control |
| Vulnerability | URL-Based Access Control Bypass |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Bypass front-end access restrictions and gain unauthorized access to the administrative interface using the `X-Original-URL` HTTP header.

---

## Vulnerability Overview

Some web applications use a front-end server (such as a reverse proxy or load balancer) to restrict access to sensitive URLs like `/admin`. If the back-end application trusts headers such as `X-Original-URL`, an attacker may be able to override the requested path and access restricted resources.

This vulnerability occurs because the front-end and back-end servers interpret the request differently, allowing access control to be bypassed.

---

## Lab Scenario

The application blocks direct access to the administrator panel. However, the response suggests that the restriction is enforced by the front-end server.

By modifying the request with the `X-Original-URL` header, it is possible to instruct the back-end server to process a different URL than the one seen by the front-end.

This bypass grants unauthorized access to administrative functionality.

---

## Tools Used

- Burp Suite Community Edition
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the lab homepage and explored the available functionality.

**Screenshot**

```
screenshots/lab02-home_page.png
```

---

### Step 2 – Access the Admin Panel

Attempted to visit:

```
/admin
```

The application returned an **Access Denied** response.

This indicated that access restrictions were in place.

**Screenshot**

```
screenshots/lab02-admin-blocked.png
```

---

### Step 3 – Intercept the Request

Captured the request using Burp Suite and sent it to **Repeater** for further testing.

---

### Step 4 – Test the X-Original-URL Header

Modified the request as follows:

```
GET / HTTP/1.1
X-Original-URL: /invalid
```

The server returned a **404 Not Found** response.

This confirmed that the back-end application was processing the value supplied in the `X-Original-URL` header instead of the original request path.

**Screenshot**

```
screenshots/lab02-header-test-404.png
```

---

### Step 5 – Bypass Access Control

Updated the header value to:

```
X-Original-URL: /admin
```

The application returned the administrator panel even though direct access to `/admin` was blocked.

**Screenshot**

```
screenshots/lab02-access-admin.png
```

---

### Step 6 – Delete the Target User

Modified the request to delete the target user by using the query parameter:

```
/?username=carlos
```

while setting:

```
X-Original-URL: /admin/delete
```

The request successfully deleted the user and completed the lab.

**Screenshot**

```
screenshots/lab02-query_to_delete.png
```

---

## Result

By manipulating the `X-Original-URL` header, it was possible to bypass front-end access controls and gain unauthorized access to administrative functionality.

The lab was successfully completed.

**Screenshot**

```
screenshots/lab02-lab_solved.png
```

---

# Impact

Successful exploitation could allow an attacker to:

- Access restricted administrative pages
- Bypass front-end security controls
- Execute administrative actions
- Delete user accounts
- Perform privilege escalation

---

# Root Cause

The application relied on the front-end server to enforce access restrictions while the back-end server trusted the `X-Original-URL` header without validating the user's authorization.

This inconsistency between the front-end and back-end resulted in an access control bypass.

---

# Remediation

- Enforce authorization checks on the back-end server.
- Do not trust client-controlled routing headers such as `X-Original-URL`.
- Configure reverse proxies to remove or overwrite sensitive routing headers.
- Validate user permissions before processing every administrative request.
- Apply the principle of least privilege.

---

# Key Learning

This lab demonstrates that access control should never rely solely on front-end protections.

Even if sensitive URLs are blocked by a proxy or load balancer, the back-end application must independently verify whether the authenticated user is authorized to access the requested resource.

---

# References

- OWASP Top 10 2025 – Broken Access Control
- PortSwigger Web Security Academy – URL-Based Access Control Can Be Circumvented
