# OWASP Juice Shop - Penetration Test Report

## 1. Executive Summary

A penetration test was performed against the OWASP Juice Shop application to identify security weaknesses that could be exploited by an attacker.

During testing, an **SSRF (Server-Side Request Forgery)** vulnerability was identified in the product stock checking functionality.

The vulnerability allows an attacker to control the destination of a server-side request through the `stockapi` parameter. By modifying this parameter, the server can be forced to make requests to internal resources that should not be directly accessible.

During exploitation, the SSRF was used to reach an internal administrative endpoint and access functionality that was intended to be restricted.

The finding is rated **High Risk** because the vulnerability allows interaction with internal resources and potentially privileged functionality.

---

# 2. Scope

### Application

OWASP Juice Shop

### Affected Functionality

Product stock checking functionality

### Affected Endpoint

```text
/product/stock
```

### Testing Tool

Burp Suite

---

# 3. Finding Summary

| ID      | Finding                            | Risk |
| ------- | ---------------------------------- | ---- |
| SSRF-01 | Server-Side Request Forgery (SSRF) | High |

---

# 4. Finding: Server-Side Request Forgery (SSRF)

**Risk:** High

**Affected Endpoint:**

```text
/product/stock
```

**Affected Parameter:**

```text
stockapi
```

---

## 4.1 Description

Server-Side Request Forgery (SSRF) occurs when an application allows an attacker to influence the destination of a request made by the server.

In a vulnerable application, an attacker may be able to make the server send requests to destinations that the attacker cannot access directly.

This can become particularly serious when the server has access to internal services, administrative interfaces, or other protected resources.

In this case, the product stock functionality accepts a user-controlled `stockapi` parameter. The application uses this parameter to determine where the stock request should be sent.

During testing, the destination was modified to an internal localhost address. The server accepted the modified destination and made the request on behalf of the attacker.

This allowed access to an internal administrative functionality.

---

# 5. Proof of Concept

## Step 1 - Identify the Stock Check Request

A product's stock availability was checked through the application's stock functionality.

The request was captured using Burp Suite and sent to Burp Repeater for further testing.

The request contained a parameter named:

```text
stockapi
```

This parameter appeared to control the destination used by the server for the stock request.

---

## Step 2 - Test the Normal Behaviour

The original request was sent through Burp Repeater to understand the normal behaviour of the application.

The server returned the expected stock information.

This established a baseline before modifying the request.

---

## Step 3 - Test an Internal Destination

The `stockapi` parameter was modified to point toward the local server.

The application initially blocked obvious localhost values such as:

```text
localhost
```

and:

```text
127.0.0.1
```

This indicated that some input filtering was present.

However, the filtering relied on specific representations of localhost rather than preventing requests to internal destinations at the server-side request layer.

---

## Step 4 - Bypass the Localhost Filter

An alternative representation of the localhost address was tested:

```text
127.1
```

The application accepted this value and the server made the request to the local system.

This confirmed that the `stockapi` parameter could be used to influence server-side requests.

---

## Step 5 - Access the Internal Admin Panel

The SSRF was then used to request the internal administrative endpoint:

```text
127.1/admin
```

The direct request was blocked because the application contained another filter for the `/admin` path.

An encoding-based bypass was then tested.

The `a` in `admin` was double-encoded:

```text
%2561
```

The resulting request was:

```text
127.1/%2561dmin
```

The server processed the encoded path and the internal administrative functionality became accessible.

---

## Step 6 - Demonstrate Impact

The internal administrative delete functionality was then accessed using:

```text
127.1/%2561dmin/delete?username=carlos
```

This demonstrated that the SSRF was not limited to reading internal resources.

The server could also be forced to perform privileged functionality against an internal endpoint.

---

# 6. Attack Flow

```text
Attacker
   |
   | Modify stockapi
   v
/product/stock
   |
   | Server makes request
   v
127.1
   |
   | Access internal resource
   v
127.1/%2561dmin
   |
   | Access administrative functionality
   v
/admin/delete?username=carlos
```

---

# 7. Impact

An attacker exploiting this vulnerability could potentially:

* Force the server to make requests to unintended destinations.
* Access internal resources that are not directly exposed.
* Interact with internal administrative functionality.
* Access resources protected from external users.
* Disclose internal infrastructure information.
* Perform actions that should only be available to trusted internal users.

The demonstrated impact included reaching an internal administrative endpoint and invoking its delete functionality.

---

# 8. Root Cause

The primary root cause is that the application **trusts a user-controlled destination**.

The `stockapi` parameter is used to determine where the server sends a request without sufficiently restricting the destination.

Although input filtering was implemented for obvious values such as `localhost` and `127.0.0.1`, these filters could be bypassed using alternative representations.

The application therefore relied on blacklist-style input filtering instead of enforcing a secure server-side request policy.

---

# 9. Remediation

### 9.1 Do Not Trust User-Controlled Destinations

The application should not directly use user-controlled input as the destination of server-side requests.

---

### 9.2 Use an Allowlist

If the stock functionality requires requests to a fixed set of services, allow only approved destinations.

For example:

```text
https://approved-stock-service.example
```

All other destinations should be rejected.

---

### 9.3 Block Internal Network Access

Server-side requests should be prevented from reaching:

* Loopback addresses
* Private IP ranges
* Internal administrative services
* Link-local addresses
* Cloud metadata endpoints

This validation should occur after resolving the destination rather than relying only on string matching.

---

### 9.4 Validate Redirects

If the server follows redirects, validate the final destination as well.

An attacker should not be able to provide an external URL that redirects the server toward an internal resource.

---

### 9.5 Restrict Internal Administrative Endpoints

Internal administrative functionality should have proper authentication and authorization controls.

An internal endpoint should not assume that a request is trusted simply because it originates from the local server.

---

### 9.6 Return Generic Errors

Requests to forbidden destinations should be rejected with an appropriate HTTP error such as:

```text
403 Forbidden
```

Detailed internal request information should not be exposed to the client.

---

# 10. Risk Assessment

| Factor                  | Assessment                                            |
| ----------------------- | ----------------------------------------------------- |
| Vulnerability           | SSRF                                                  |
| Severity                | High                                                  |
| Attack Complexity       | Low                                                   |
| Authentication Required | Application-dependent                                 |
| User Interaction        | Not required                                          |
| Impact                  | Internal resource access and privileged functionality |

---

# 11. Evidence

The following evidence was captured during testing:

```text
screenshots/
├── ssrf-normal-request.png
├── ssrf-stockapi-modification.png
├── ssrf-localhost-bypass.png
├── ssrf-admin-access.png
└── ssrf-delete-functionality.png
```

> Replace the filenames above with the actual screenshot names from the project if they are different.

---

# 12. Conclusion

The assessment identified a **High-Risk Server-Side Request Forgery vulnerability** in the product stock checking functionality.

The vulnerable `stockapi` parameter allowed the server-side request destination to be controlled by the attacker.

Basic localhost filtering was bypassed using an alternative IP representation, and the SSRF was subsequently used to reach an internal administrative endpoint.

The testing demonstrated that the vulnerability could be used to interact with privileged internal functionality.

The issue should therefore be addressed by removing trust in client-controlled destinations, implementing strict allowlisting, validating resolved destinations, and enforcing proper authentication and authorization on internal administrative functionality.

---

## Finding Status

**Status:** Vulnerable

**Risk:** High

**Recommended Priority:** High
