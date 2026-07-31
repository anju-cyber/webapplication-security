# Lab 02 - Stored Cross-Site Scripting (XSS) in Anchor `href` Attribute

## Lab Information

| Field | Value |
|-------|-------|
| Category | Injection |
| Subcategory | Cross-Site Scripting (XSS) |
| Vulnerability | Stored XSS |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify a Stored Cross-Site Scripting (XSS) vulnerability by injecting a malicious URL into the **Website** field of a blog comment and achieve JavaScript execution when another user clicks the generated hyperlink.

---

## Vulnerability Overview

Stored Cross-Site Scripting (Stored XSS) occurs when user-supplied input is permanently stored by the application and later displayed to other users without proper validation or output encoding.

In this lab, the value entered in the **Website** field is stored and rendered inside the `href` attribute of an anchor tag. Because the application fails to validate the URL scheme, an attacker can inject a `javascript:` URI that executes JavaScript when the link is clicked.

---

## Lab Scenario

The application allows users to post comments along with an optional website URL.

The supplied website value is stored and later rendered as a clickable hyperlink. By supplying a malicious `javascript:` URI instead of a legitimate website address, arbitrary JavaScript can be executed whenever the generated link is clicked.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application homepage and navigated to a blog post containing a comment section.

**Screenshot**

```
screenshots/lab02-home_page.png
```

---

### Step 2 – Submit a Test Comment

Submitted a comment using a random value in the **Website** field.

Captured the request using Burp Suite and sent it to Burp Repeater.

**Screenshot**

```
screenshots/lab02_comment_section.png
```

---

### Step 3 – Identify the Reflection Context

Viewed the published comment and intercepted the page request.

The supplied website value was reflected inside the `href` attribute of an anchor element.

**Screenshot**

```
screenshots/lab02-input_reflection.png
```

---

### Step 4 – Inject the Malicious Payload

Submitted a second comment using the following payload in the **Website** field:

```text
javascript:alert(1)
```

The application accepted and stored the payload without validating the URL scheme.

**Screenshot**

```
screenshots/lab02-payload_input.png
```

---

### Step 5 – Trigger the Stored XSS

Viewed the published comment and clicked the attacker's username hyperlink.

The browser executed the JavaScript payload, confirming successful Stored XSS.

**Screenshot**

```
screenshots/lab02-payload_execution.png
```

---

### Step 6 – Complete the Lab

After successfully triggering the payload, the lab was marked as solved.

**Screenshot**

```
screenshots/lab02-lab_solved.png
```

---

## Result

The application stored attacker-controlled input inside an anchor `href` attribute without validating the supplied URL.

A malicious `javascript:` URI executed arbitrary JavaScript when the generated hyperlink was clicked.

The lab was successfully completed.

---

## Reflection Context

| Property | Value |
|----------|-------|
| Reflection Type | Stored |
| Injection Point | Website Field |
| Output Context | HTML Anchor `href` Attribute |
| Trigger | User Click |
| Payload | `javascript:alert(1)` |

---

# Impact

Successful exploitation could allow an attacker to:

- Execute arbitrary JavaScript in another user's browser
- Steal session information (where applicable)
- Perform actions on behalf of authenticated users
- Redirect users to phishing websites
- Modify page content
- Deliver malicious payloads to multiple visitors

---

# Root Cause

The application failed to validate user-supplied URLs before storing and rendering them.

Instead of restricting accepted protocols (such as `https://` or `http://`), it allowed the dangerous `javascript:` scheme, enabling client-side code execution.

---

# Remediation

- Validate and whitelist acceptable URL schemes.
- Reject dangerous protocols such as `javascript:`, `data:`, and `vbscript:`.
- Apply context-aware output encoding.
- Implement a strong Content Security Policy (CSP).
- Validate all user-controlled input before storing it.

---

# Key Learning

This lab demonstrates that Stored XSS can occur even without injecting HTML tags or script elements.

Allowing untrusted input inside an anchor's `href` attribute without validating the URL scheme enables attackers to inject `javascript:` URIs that execute code when users interact with the link.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP Cross-Site Scripting Prevention Cheat Sheet
- PortSwigger Web Security Academy – Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded
