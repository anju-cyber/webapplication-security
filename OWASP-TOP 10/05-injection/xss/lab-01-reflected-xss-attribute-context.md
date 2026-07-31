
# Lab 01 - Reflected Cross-Site Scripting (XSS) in an HTML Attribute

## Lab Information

| Field | Value |
|-------|-------|
| Category | Injection |
| Subcategory | Cross-Site Scripting (XSS) |
| Vulnerability | Reflected XSS |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify a reflected Cross-Site Scripting (XSS) vulnerability where user input is reflected inside an HTML attribute and execute JavaScript by escaping the attribute context.

---

## Vulnerability Overview

Reflected Cross-Site Scripting (XSS) occurs when user-controlled input is immediately reflected in the server's response without proper output encoding.

In this lab, the application reflects user input inside a quoted HTML attribute. Although angle brackets (`<` and `>`) are HTML-encoded, quotation marks are not properly handled, allowing an attacker to break out of the attribute and inject a malicious event handler.

---

## Lab Scenario

The application includes a search feature that reflects the submitted search term within an HTML attribute.

By analyzing the reflection context and injecting a specially crafted payload, JavaScript execution can be achieved when a user interacts with the affected element.

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

Opened the application homepage and located the search functionality.

**Screenshot**

```
screenshots/lab01-home_page.png
```

---

### Step 2 – Identify the Reflection

Submitted a random search value and intercepted the request using Burp Suite.

Sent the request to Burp Repeater for further analysis.

The response reflected the supplied input inside a quoted HTML attribute.

**Screenshot**

```
screenshots/lab01-reflection.png
```

---

### Step 3 – Analyze the Reflection Context

Inspected the server response and confirmed that the input appeared inside an HTML attribute.

Although angle brackets were encoded, the quotation marks surrounding the attribute remained exploitable.

**Screenshot**

```
screenshots/lab01-reflection_context.png
```

---

### Step 4 – Test Payload Injection

Modified the search parameter with the following payload:

```text
" onmouseover="alert(1)
```

The injected quotation mark terminated the existing attribute, while the `onmouseover` event handler introduced attacker-controlled JavaScript.

**Screenshot**

```
screenshots/lab01-script_payload.png
```

---

### Step 5 – Verify Code Execution

Copied the generated URL into the browser and interacted with the affected element.

Moving the mouse over the injected element triggered JavaScript execution, confirming the reflected XSS vulnerability.

**Screenshot**

```
screenshots/lab01-payload_execution.png
```

---

## Result

User input was reflected inside an HTML attribute without sufficient output encoding.

By escaping the attribute and injecting an event handler, arbitrary JavaScript executed within the victim's browser.

The lab was successfully completed.

---

# Impact

Successful exploitation could allow an attacker to:

- Execute arbitrary JavaScript in a victim's browser
- Steal session cookies (where applicable)
- Perform actions on behalf of authenticated users
- Modify page content
- Redirect users to malicious websites
- Launch phishing attacks

---

# Root Cause

The application reflected user-controlled input inside an HTML attribute without properly encoding quotation marks.

This allowed an attacker to terminate the existing attribute and inject a new event handler.

---

# Remediation

- Apply context-aware output encoding for HTML attributes.
- Encode quotation marks before rendering user input.
- Validate and sanitize untrusted input.
- Implement a strong Content Security Policy (CSP).
- Prefer secure templating frameworks that automatically perform contextual output encoding.

---

# Key Learning

This lab demonstrates that HTML encoding alone is not always sufficient to prevent Cross-Site Scripting.

When user input is placed inside an HTML attribute, developers must perform context-aware encoding for quotation marks and other special characters. Understanding the reflection context is essential when identifying and exploiting XSS vulnerabilities.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP Cross-Site Scripting Prevention Cheat Sheet
- PortSwigger Web Security Academy – Reflected XSS into Attribute with Angle Brackets HTML-Encoded
