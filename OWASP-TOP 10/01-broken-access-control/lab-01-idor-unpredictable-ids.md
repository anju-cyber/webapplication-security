# Lab 01 - Insecure Direct Object Reference (IDOR) Using Predictable User IDs

## Lab Information

| Field | Value |
|-------|-------|
| Category | Broken Access Control |
| Vulnerability | Insecure Direct Object Reference (IDOR) |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify an Insecure Direct Object Reference (IDOR) vulnerability by manipulating a predictable user identifier and retrieve another user's API key.

---

## Vulnerability Overview

An Insecure Direct Object Reference (IDOR) occurs when an application exposes internal object identifiers without verifying whether the authenticated user is authorised to access the requested resource.

Instead of enforcing server-side access control, the application trusts the value supplied by the client. If identifiers are predictable, an attacker can modify them to access resources belonging to other users.

---

## Lab Scenario

The application contains public blog posts authored by different users.

Each author's profile contains a numeric user identifier that is exposed within the URL. After authenticating with valid credentials, the account page also references the logged-in user's identifier through the same parameter.

Because the application fails to validate ownership of the requested resource, replacing the identifier with another user's ID exposes their account information.

---

## Tools Used

- Burp Suite Community Edition
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Blog

Opened the application homepage and explored the available blog posts.

---

### Step 2 – Identify Carlos' User ID

Selected Carlos' profile and observed that the profile URL contained a predictable numeric user ID.

The identifier was noted for later testing.

**Screenshot**

```
screenshots/lab01-home_page.png
```

```
screenshots/lab01-guid-harvest.png
```

---

### Step 3 – Log in to the Application

Authenticated using the credentials provided by the lab.

After login, navigated to the **My Account** page.

**Screenshot**

```
screenshots/lab01-my-account.png
```

---

### Step 4 – Test for IDOR

Modified the `id` parameter in the account URL by replacing the current user's identifier with Carlos' user ID obtained earlier.

The application returned Carlos' account information instead of denying access.

---

### Step 5 – Retrieve the API Key

The response exposed Carlos' API key.

The key was submitted to complete the lab.

---

## Result

The application disclosed another user's sensitive account information without performing proper authorisation checks.

The lab was successfully completed.

**Screenshot**

```
screenshots/lab01-lab_solved.png
```

---

# Impact

Successful exploitation could allow an attacker to:

- Access another user's profile
- Retrieve sensitive information
- Access API keys
- View confidential account data
- Perform horizontal privilege escalation

---

# Root Cause

The application relied on a client-controlled identifier while failing to verify whether the authenticated user owned the requested resource.

Authorisation checks were missing on the server side.

---

# Remediation

- Perform server-side authorisation for every request.
- Never rely on client-supplied object identifiers.
- Verify that the authenticated user owns the requested resource.
- Use indirect or unpredictable object identifiers where appropriate.
- Apply the principle of least privilege.

---

# Key Learning

This lab demonstrates that authentication alone is not sufficient to protect sensitive resources.

Even when a user is logged in, every request must include proper server-side authorisation checks to ensure the requested object belongs to the authenticated user.

---

# References

- OWASP Top 10 2025 – Broken Access Control
- PortSwigger Web Security Academy – IDOR
