# Lab 02 - Information Disclosure Through an Exposed Debug Page

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

Identify an exposed debugging page and retrieve sensitive information disclosed through it.

---

## Vulnerability Overview

Debugging pages are designed to help developers troubleshoot applications during development. If these pages remain accessible in production, they may expose sensitive information such as environment variables, server configuration, installed modules, file paths, API keys, or secret values.

Attackers can leverage this information to better understand the application's environment and potentially compromise sensitive assets.

---

## Lab Scenario

The application homepage contains an HTML comment referencing an internal debug page.

By locating and accessing this page, sensitive server configuration details become available, including the application's `SECRET_KEY` environment variable.

The disclosed secret is then used to successfully complete the lab.

---

## Tools Used

- Burp Suite Community Edition
- Burp Target Site Map
- Burp Engagement Tools
- Burp Repeater
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application homepage while Burp Suite was running.

**Screenshot**

```
screenshots/lab02-home_page.png
```

---

### Step 2 – Discover Hidden Content

Opened **Target → Site Map** in Burp Suite.

Used the **Find Comments** engagement tool to inspect HTML comments within the application's responses.

A hidden comment referenced an internal debug page.

```
/cgi-bin/phpinfo.php
```

**Screenshot**

```
screenshots/lab02-finding.png
```

---

### Step 3 – Access the Debug Page

Sent the request for the discovered page to Burp Repeater.

```
GET /cgi-bin/phpinfo.php
```

The response returned detailed debugging information.

**Screenshot**

```
screenshots/lab02-burp_content_discovery.png
```

---

### Step 4 – Identify Sensitive Information

Reviewed the debugging output and located the application's environment variables.

The response disclosed the following sensitive value:

```
SECRET_KEY
```

This value was intended for internal application use and should not have been publicly accessible.

**Screenshot**

```
screenshots/lab02-find_secret_key.png
```

---

### Step 5 – Submit the Solution

Submitted the disclosed `SECRET_KEY` value to complete the lab.

**Screenshot**

```
screenshots/lab02-lab_solved.png
```

---

## Result

A publicly accessible debug page exposed sensitive application configuration, including the `SECRET_KEY` environment variable.

The disclosed value was successfully used to solve the lab.

---

# Impact

Successful exploitation could allow an attacker to:

- Retrieve sensitive environment variables
- Expose application secrets
- Discover server configuration details
- Identify installed software and modules
- Gather valuable reconnaissance information for further attacks

---

# Root Cause

A debugging page intended for development remained publicly accessible in the production environment.

Additionally, sensitive configuration data and environment variables were exposed without any access restrictions.

---

# Remediation

- Disable debugging pages before deploying applications to production.
- Restrict access to administrative and diagnostic endpoints.
- Never expose environment variables through publicly accessible pages.
- Remove unnecessary comments that disclose internal resources.
- Regularly review applications for exposed development artifacts.

---

# Key Learning

This lab demonstrates that information disclosure is not limited to error messages. Development artifacts such as debug pages and HTML comments can unintentionally reveal sensitive information that assists attackers during reconnaissance.

Removing or protecting these resources is an essential part of securing production applications.

---

# References

- OWASP Top 10 2025 – Security Misconfiguration
- PortSwigger Web Security Academy – Information Disclosure on Debug Page
