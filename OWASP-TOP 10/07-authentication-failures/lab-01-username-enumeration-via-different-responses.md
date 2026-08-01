
# Lab 01 - Username Enumeration via Different Responses

## Lab Information

| Field | Value |
|-------|-------|
| Category | Authentication Failures |
| Vulnerability | Username Enumeration |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify a valid username by analyzing differences in server responses and then determine the correct password using Burp Suite Intruder.

---

## Vulnerability Overview

Username Enumeration occurs when an application reveals whether a username exists based on differences in its authentication responses.

Even subtle differences, such as response messages, response length, HTTP status codes, or response time, can help an attacker distinguish valid usernames from invalid ones. Once a valid username is identified, attackers can focus password guessing or brute-force attacks on a single account.

---

## Lab Scenario

The login page returns different responses depending on whether the supplied username exists.

Using Burp Suite Intruder, candidate usernames were tested to identify a valid account based on the application's response. After identifying the valid username, a second Intruder attack was performed against the password field to determine the correct password.

---

## Tools Used

- Burp Suite Community Edition / Professional
- Burp Proxy
- Burp Intruder
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application homepage and navigated to the login page.

**Screenshot**

```
screenshots/lab01-home_page.png
```

---

### Step 2 – Capture the Login Request

Submitted invalid credentials and intercepted the login request using Burp Suite.

The request was sent to Burp Intruder for further testing.

**Screenshot**

```
screenshots/lab01-login_form.png
```

---

### Step 3 – Enumerate Usernames

Configured Burp Intruder in **Sniper** mode.

The `username` parameter was selected as the payload position while keeping the password static.

A list of candidate usernames was supplied for testing.

**Screenshot**

```
screenshots/lab01-burp_intruder.png
```

---

### Step 4 – Analyze the Responses

Compared the responses generated for each username.

Most responses returned:

```
Invalid username
```

One response differed by returning:

```
Incorrect password
```

The response length was also noticeably different, confirming that the username existed.

**Screenshot**

```
screenshots/lab01-user_enum.png
```

---

### Step 5 – Enumerate the Password

After identifying the valid username, the payload position was moved to the `password` parameter.

A password wordlist was supplied to Intruder while keeping the username fixed.

**Screenshot**

```
screenshots/lab01-password_bruteforce.png
```

---

### Step 6 – Identify Successful Authentication

Reviewed the Intruder results.

Most requests returned an HTTP **200 OK** response.

One request returned an **HTTP 302 Redirect**, indicating successful authentication.

The corresponding password was identified from the payload results.

**Screenshot**

```
screenshots/lab01-password_enum.png
```

---

### Step 7 – Log In

Logged in using the identified username and password.

Successfully accessed the user account, completing the lab.

**Screenshot**

```
screenshots/lab01-lab_solved.png
```

---

## Result

The application disclosed whether a username existed by returning different authentication responses.

This allowed enumeration of a valid account, after which the correct password was identified through a targeted password attack.

The lab was successfully completed.

---

## Attack Details

| Property | Value |
|----------|-------|
| Vulnerability | Username Enumeration |
| Enumeration Method | Response Message Analysis |
| Secondary Technique | Password Brute Force |
| Detection Indicator | Response Length & Status Code |

---

# Impact

Successful exploitation could allow an attacker to:

- Identify valid user accounts
- Reduce the effort required for password attacks
- Perform targeted brute-force attacks
- Increase the likelihood of account compromise
- Assist credential stuffing attacks

---

# Root Cause

The application returned different responses for valid and invalid usernames during authentication.

These differences leaked information about account existence, enabling attackers to enumerate usernames before attempting password attacks.

---

# Remediation

- Return identical error messages for all authentication failures.
- Ensure response length and timing remain consistent.
- Implement account lockout or rate limiting after repeated failed logins.
- Require multi-factor authentication (MFA) for sensitive accounts.
- Monitor and alert on repeated authentication attempts.

---

# Key Learning

This lab demonstrates that authentication systems can leak sensitive information even without revealing passwords.

Small differences in response messages, response size, or HTTP status codes are often enough for attackers to identify valid usernames. Preventing information leakage is an important part of designing secure authentication mechanisms.

---

# References

- OWASP Top 10 2025 – Identification and Authentication Failures
- OWASP Authentication Cheat Sheet
- PortSwigger Web Security Academy – Username Enumeration via Different Responses
