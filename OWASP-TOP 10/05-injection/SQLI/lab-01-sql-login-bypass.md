# Lab 01 - SQL Injection Authentication Bypass

## Lab Information

| Field | Value |
|-------|-------|
| Category | Injection |
| Subcategory | SQL Injection |
| Vulnerability | Authentication Bypass |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Exploit a SQL Injection vulnerability in the login functionality to gain unauthorized access as the administrator without knowing the password.

---

## Vulnerability Overview

SQL Injection occurs when an application inserts user-controlled input directly into an SQL query without proper validation or parameterized queries.

In this lab, the login form is vulnerable to SQL Injection. By manipulating the `username` parameter, it is possible to modify the SQL query and bypass the authentication process.

---

## Lab Scenario

The application provides a standard login page that requires a username and password.

During testing, the login request was intercepted using Burp Suite. By injecting SQL syntax into the `username` parameter, the original SQL query was altered, allowing authentication as the **administrator** account without supplying a valid password.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application and navigated to the login page.

**Screenshot**

```
screenshots/lab01-home_page.png
```

---

### Step 2 – Intercept the Login Request

Entered test credentials and intercepted the login request using Burp Suite.

The request was forwarded to Burp Repeater for testing.

---

### Step 3 – Test for SQL Injection

Modified the `username` parameter with the following payload:

```text
administrator'--
```

The SQL comment sequence (`--`) ignored the remaining part of the query, allowing the application to authenticate as the administrator account.

**Screenshot**

```
screenshots/lab01-password_bypass.png
```

---

### Step 4 – Verify Authentication Bypass

Forwarded the modified request.

The application authenticated the session as **administrator**, successfully bypassing the login process without requiring the correct password.

**Screenshot**

```
screenshots/lab01-lab_solved.png
```

---

## Result

The login functionality was vulnerable to SQL Injection, allowing authentication as the administrator account without valid credentials.

The lab was successfully completed.

---

## Injection Details

| Property | Value |
|----------|-------|
| Injection Point | Username Parameter |
| Vulnerability Type | Authentication Bypass |
| Database Interaction | SQL Query Manipulation |
| Payload Used | `administrator'--` |

---

# Impact

Successful exploitation could allow an attacker to:

- Bypass the authentication mechanism
- Gain unauthorized access to privileged accounts
- Escalate privileges within the application
- Access sensitive information
- Compromise the confidentiality and integrity of the application

---

# Root Cause

The application concatenated user input directly into an SQL query without using parameterized statements or prepared queries.

As a result, user-controlled input was interpreted as part of the SQL statement, allowing the authentication logic to be modified.

---

# Remediation

- Use parameterized queries (prepared statements) for all database interactions.
- Never concatenate user input into SQL queries.
- Validate and sanitize user input.
- Apply the principle of least privilege to database accounts.
- Implement logging and monitoring to detect suspicious authentication attempts.

---

# Key Learning

This lab demonstrates one of the most common SQL Injection scenarios—authentication bypass.

A single improperly handled input field allowed complete access to the administrator account. Secure coding practices, particularly parameterized queries, are essential to prevent SQL Injection vulnerabilities.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP SQL Injection Prevention Cheat Sheet
- PortSwigger Web Security Academy – SQL Injection Vulnerability Allowing Login Bypass
