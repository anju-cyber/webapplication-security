
# SSRF Security Assessment Report

## 1. Executive Summary

During security testing, a **Server-Side Request Forgery (SSRF)** vulnerability was identified in the application's product stock checking functionality.

The vulnerability exists because the application accepts a user-controlled destination through the `stockapi` parameter and uses it to make a server-side request.

During testing, the server was forced to make requests to a local/internal destination. Although direct requests to `localhost` and `127.0.0.1` were blocked, the filtering could be bypassed using an alternative loopback address (`127.1`).

Further testing demonstrated that the internal admin functionality could be accessed through the SSRF by using double URL encoding. The delete functionality was also successfully reached.

**Risk Rating: High**

---

## 2. Finding Summary

| ID      | Finding                     | Risk | Affected Endpoint | Parameter  |
| ------- | --------------------------- | ---- | ----------------- | ---------- |
| SSRF-01 | Server-Side Request Forgery | High | `/product/stock`  | `stockapi` |

---

## 3. Vulnerability Details

### Server-Side Request Forgery (SSRF)

**Risk:** High
**Affected Endpoint:** `/product/stock`
**Affected Parameter:** `stockapi`

### Description

SSRF occurs when an attacker can control the destination of a server-side request.

In the tested application, the product stock checking functionality uses the `stockapi` parameter to determine the destination of the request.

By modifying this parameter, it was possible to make the server send requests to a local/internal destination instead of the expected stock API.

The application attempted to block `localhost` and `127.0.0.1`, but the filtering could be bypassed by using the alternative loopback address `127.1`.

The internal admin functionality was then accessed through the SSRF. The `/admin` path was also filtered, but this restriction was bypassed by double encoding the letter `a`.

---

## 4. Proof of Concept

### Step 1 — Identify the Stock Request

The product stock checking functionality was used to generate a normal request.

The request was captured using Burp Suite and sent to **Repeater** for testing.

---

### Step 2 — Observe Normal Behaviour

The original request was sent without modification to confirm the expected behaviour of the stock checking functionality.

The application returned the normal stock response.

---

### Step 3 — Test the `stockapi` Parameter

The `stockapi` parameter was identified as controlling the destination of the server-side request.

The parameter was modified to test whether the server could be forced to communicate with the local system.

Direct values such as:

```text
localhost
```

and:

```text
127.0.0.1
```

were blocked by the application.

---

### Step 4 — Bypass the Localhost Filter

An alternative representation of the loopback address was tested:

```text
127.1
```

The request was accepted.

This demonstrated that the application was relying on insufficient input filtering to prevent access to the local system.

---

### Step 5 — Access Internal Admin Functionality

After confirming that the server could make a request to `127.1`, the internal admin path was tested:

```text
127.1/admin
```

The `/admin` path was blocked.

The filtering was then tested using double URL encoding.

The letter `a` in `admin` was double encoded:

```text
127.1/%2561dmin
```

This bypassed the restriction and allowed the internal admin functionality to be reached.

---

### Step 6 — Demonstrate Impact

The delete functionality was then accessed through the SSRF:

```text
127.1/%2561dmin/delete?username=carlos
```

This demonstrated that the SSRF was not limited to reading a resource. It could also be used to reach internal functionality capable of performing an action.

---

## 5. Attack Flow

```text
Attacker
   |
   v
/ product / stock
   |
   v
stockapi parameter
   |
   v
127.1
   |
   v
Internal application
   |
   v
/%2561dmin
   |
   v
Internal Admin Functionality
   |
   v
/delete?username=carlos
```

---

## 6. Impact

Successful exploitation of the vulnerability can allow an attacker to:

* Force the server to make requests to unintended URLs.
* Make requests to internal resources.
* Access resources that are not directly available to the attacker.
* Perform functionality that an internal user can perform.
* Potentially disclose internal information.
* Potentially disclose information about internal infrastructure.
* Access forbidden resources.

In the demonstrated scenario, the SSRF was used to reach internal administrative functionality and access a delete operation.

---

## 7. Root Cause

The primary root cause is the application's trust in a **user-controlled destination**.

The application accepts the destination through the `stockapi` parameter without sufficiently restricting where the server is allowed to connect.

The implemented input validation can also be bypassed using alternative representations of the loopback address and URL encoding.

---

## 8. Recommendations

### 8.1 Validate User-Controlled Destinations

Do not directly trust URLs or destinations supplied by users.

Implement strict server-side validation before making any outbound request.

### 8.2 Use an Allowlist

Where possible, allow requests only to explicitly approved destinations instead of attempting to block known malicious destinations.

### 8.3 Restrict Internal Resources

The application should prevent server-side requests to internal and restricted resources.

Requests attempting to access forbidden internal resources should be rejected with an appropriate HTTP response, such as:

```text
403 Forbidden
```

### 8.4 Validate After Resolution

Validation should account for alternative representations of IP addresses and the actual destination reached by the server, rather than relying only on simple string matching.

### 8.5 Do Not Rely on Blacklists

Blocking only strings such as:

```text
localhost
127.0.0.1
```

is insufficient because the same destination may be represented in different ways.

---

## 9. Risk Assessment

| Category          | Assessment                         |
| ----------------- | ---------------------------------- |
| Vulnerability     | Server-Side Request Forgery (SSRF) |
| Severity          | High                               |
| Affected Endpoint | `/product/stock`                   |
| Parameter         | `stockapi`                         |
| Primary Impact    | Internal resource access           |
| Additional Impact | Access to internal functionality   |

---

## 10. Evidence

The following screenshots can be included as supporting evidence:

```text
screenshots/
├── ssrf-normal-request.png
├── ssrf-stockapi-parameter.png
├── ssrf-localhost-filter.png
├── ssrf-127-1-bypass.png
├── ssrf-admin-bypass.png
└── ssrf-impact-delete-user.png
```

Rename the screenshot files according to the actual evidence you have.

---

## 11. Conclusion

A **High Risk SSRF vulnerability** was identified in the application's `/product/stock` functionality.

The `stockapi` parameter allows the attacker to influence the destination of a server-side request. The implemented protection against localhost access could be bypassed using `127.1`.

Further testing demonstrated that internal administrative functionality could be reached by bypassing the `/admin` restriction through double URL encoding.

The issue should be addressed by implementing strict destination validation, preferably using an allowlist, and preventing server-side requests to internal or otherwise unauthorized resources.
