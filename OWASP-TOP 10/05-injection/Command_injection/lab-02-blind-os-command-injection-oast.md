# Lab 02 - Blind OS Command Injection with Out-of-Band Interaction

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

Identify a Blind OS Command Injection vulnerability by forcing the target server to perform an out-of-band (OAST) DNS lookup to a Burp Collaborator domain.

---

## Vulnerability Overview

Blind OS Command Injection occurs when user input is executed as an operating system command, but the application does not return the command output in the HTTP response.

In these situations, attackers often rely on Out-of-Band Application Security Testing (OAST) techniques. Instead of displaying output directly, the injected command forces the server to interact with an external system controlled by the tester.

A successful DNS or HTTP interaction confirms that command execution occurred on the target server.

---

## Lab Scenario

The application contains a feedback form that processes user-supplied input on the server.

Although no command output is displayed, the vulnerable parameter can be abused to execute a system command that triggers a DNS lookup to a Burp Collaborator domain.

When the external interaction is observed, command execution is confirmed.

---

## Tools Used

- Burp Suite Community Edition / Professional
- Burp Proxy
- Burp Repeater
- Burp Collaborator
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application homepage and navigated to the feedback form.

**Screenshot**

```
screenshots/lab02-home_page.png
```

---

### Step 2 – Intercept the Feedback Request

Submitted sample feedback and intercepted the request using Burp Suite.

The request was forwarded to Burp Repeater for further testing.

**Screenshot**

```
screenshots/lab02-submit_feedback.png
```

---

### Step 3 – Inject the Payload

Modified the `email` parameter to include a command that would perform a DNS lookup.

Example:

```text
x||nslookup x.<collaborator-domain>||
```

A unique Burp Collaborator payload was inserted into the request.

**Screenshot**

```
screenshots/lab02-modify_request.png
```

---

### Step 4 – Send the Request

Forwarded the modified request to the application.

The HTTP response did not contain any visible indication that the command had executed.

This behavior is typical for Blind Command Injection vulnerabilities.

---

### Step 5 – Check Burp Collaborator

Opened Burp Collaborator and reviewed incoming interactions.

A DNS request originating from the target server was recorded against the generated Collaborator domain.

This confirmed that the injected operating system command had executed successfully.

**Screenshot**

```
screenshots/lab02-burp_collaborator.png
```

---

### Step 6 – Complete the Lab

After the Collaborator interaction was received, the lab was marked as solved.

**Screenshot**

```
screenshots/lab02-lab_solved.png
```

---

## Result

The application was vulnerable to Blind OS Command Injection.

Although command output was not visible in the application's response, an out-of-band DNS interaction confirmed successful command execution on the target server.

The lab was successfully completed.

---

## Injection Details

| Property | Value |
|----------|-------|
| Injection Point | `email` Parameter |
| Vulnerability Type | Blind OS Command Injection |
| Verification Method | DNS Interaction |
| OAST Platform | Burp Collaborator |

---

# Impact

Successful exploitation could allow an attacker to:

- Execute arbitrary operating system commands
- Perform external network interactions from the server
- Enumerate internal infrastructure
- Access sensitive files and data
- Potentially achieve complete server compromise

---

# Root Cause

The application incorporated user-controlled input into an operating system command without proper validation or sanitization.

As a result, shell metacharacters supplied by the attacker were interpreted by the operating system and executed.

---

# Remediation

- Avoid passing user input directly to system commands.
- Use secure APIs instead of shell execution whenever possible.
- Validate and sanitize all user-supplied input.
- Apply strict allow-list validation.
- Restrict outbound network access where appropriate.
- Run application services with minimal operating system privileges.

---

# Key Learning

This lab demonstrates that command execution vulnerabilities can still be identified even when no output is returned to the user.

Out-of-band testing techniques such as DNS and HTTP callbacks are valuable methods for detecting Blind OS Command Injection vulnerabilities and confirming successful exploitation.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP Command Injection Prevention Cheat Sheet
- PortSwigger Web Security Academy – Blind OS Command Injection with Out-of-Band Interaction
- PortSwigger Burp Collaborator Documentation
