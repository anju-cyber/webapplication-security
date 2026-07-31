# Lab 01 - Blind OS Command Injection with Output Redirection

## Lab Information

| Field | Value |
|-------|-------|
| Category | Injection |
| Subcategory | OS Command Injection |
| Vulnerability | Blind OS Command Injection |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Practitioner |
| Status | ✅ Solved |

---

## Objective

Exploit a Blind OS Command Injection vulnerability by redirecting the output of an operating system command to a publicly accessible file and retrieving the command output.

---

## Vulnerability Overview

OS Command Injection occurs when an application executes user-controlled input as part of a system command without proper validation.

In Blind OS Command Injection, the application does not directly return the output of executed commands. Instead, attackers must use indirect techniques, such as output redirection, DNS lookups, or time delays, to confirm successful command execution.

In this lab, the output of the injected command is redirected to a web-accessible directory, allowing the attacker to retrieve it through a normal HTTP request.

---

## Lab Scenario

The application provides a feedback form where user input is processed by a server-side command.

Although the application does not display command output directly, the injected command can redirect its output into a file located inside the web server's image directory. Accessing this file confirms successful command execution.

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

Opened the application homepage and navigated to the feedback form.

**Screenshot**

```
screenshots/lab01-home_page.png
```

---

### Step 2 – Submit Feedback

Entered sample feedback and intercepted the request using Burp Suite.

The request was sent to Burp Repeater for further testing.

**Screenshot**

```
screenshots/lab01-submit_feedback.png
```

---

### Step 3 – Inject the Command

Modified the `email` parameter to inject an operating system command that redirected its output into a file inside the web server directory.

Example:

```text
||whoami>/var/www/images/output.txt||
```

The application processed the request without displaying any visible output, indicating a potential Blind Command Injection vulnerability.

**Screenshot**

```
screenshots/lab01-modify_request.png
```

---

### Step 4 – Retrieve the Output

Intercepted a request for a product image and modified the filename parameter to retrieve the newly created file.

Example:

```text
filename=output.txt
```

The server returned the contents of the file, revealing the output of the injected command.

**Screenshot**

```
screenshots/lab01-read_file.png
```

---

### Step 5 – Verify the Vulnerability

The response contained the result of the `whoami` command, confirming successful command execution on the underlying operating system.

The lab was successfully completed.

**Screenshot**

```
screenshots/lab01-lab_solve.png
```

---

## Result

The application was vulnerable to Blind OS Command Injection.

Although command output was not directly displayed, redirecting the output to a web-accessible file allowed successful retrieval and verification of command execution.

---

## Injection Details

| Property | Value |
|----------|-------|
| Injection Point | `email` Parameter |
| Vulnerability Type | Blind OS Command Injection |
| Technique Used | Output Redirection |
| Verification Method | Retrieve Output File |

---

# Impact

Successful exploitation could allow an attacker to:

- Execute arbitrary operating system commands
- Read sensitive files from the server
- Enumerate the operating system and environment
- Access confidential application data
- Potentially achieve full server compromise

---

# Root Cause

The application incorporated user-controlled input into a system command without proper validation or escaping.

As a result, attacker-supplied shell operators were interpreted by the operating system, allowing arbitrary commands to be executed.

---

# Remediation

- Never pass user input directly to operating system commands.
- Use secure APIs instead of shell commands whenever possible.
- Validate and sanitize all user input.
- Apply strict allow-list validation for expected values.
- Run applications with the minimum required operating system privileges.

---

# Key Learning

This lab demonstrates that the absence of visible command output does not necessarily mean an application is secure.

Blind Command Injection vulnerabilities often require indirect techniques to confirm execution. Output redirection is a simple but effective method for verifying whether injected commands are executed by the server.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP Command Injection Prevention Cheat Sheet
- PortSwigger Web Security Academy – Blind OS Command Injection with Output Redirection
