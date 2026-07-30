
# Lab 01 - Source Code Disclosure via Backup Files

## Lab Information

| Field | Value |
|-------|-------|
| Category | Security Misconfiguration |
| Vulnerability | Source Code Disclosure |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Identify exposed backup files, retrieve sensitive source code, and use the disclosed credentials to solve the lab.

---

## Vulnerability Overview

Backup files are often created during development or deployment to preserve copies of source code. If these files are accidentally left inside the web root, they may become publicly accessible.

Exposed source code can reveal sensitive implementation details such as hard-coded credentials, API keys, internal endpoints, and application logic that can be leveraged by an attacker.

---

## Lab Scenario

The application exposes a `robots.txt` file that references a hidden backup directory.

Inside this directory, a backup copy of a Java source file is publicly accessible. Reviewing the source code reveals a hard-coded PostgreSQL database password, which can be used to successfully complete the lab.

---

## Tools Used

- Burp Suite Community Edition
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Review robots.txt

Browsed to:

```
/robots.txt
```

The file disclosed the existence of a hidden backup directory.

**Screenshot**

```
screenshots/lab03-robots-txt.png
```

---

### Step 2 – Browse the Backup Directory

Navigated to:

```
/backup
```

The directory contained a backup source code file.

```
ProductTemplate.java.bak
```

Alternatively, Burp Suite's **Discover Content** feature can be used to identify the hidden directory.

**Screenshot**

```
screenshots/lab03-backup-directory.png
```

---

### Step 3 – Download the Backup File

Accessed the following file:

```
/backup/ProductTemplate.java.bak
```

The server returned the Java source code without requiring authentication.

**Screenshot**

```
screenshots/lab03-source-code.png
```

---

### Step 4 – Review the Source Code

Inspected the Java source file and located a hard-coded PostgreSQL database password within the database connection builder.

This sensitive information was sufficient to complete the lab.

**Screenshot**

```
screenshots/lab03-hardcoded-password.png
```

---

### Step 5 – Submit the Password

Returned to the application and submitted the recovered database password.

The lab was solved successfully.

**Screenshot**

```
screenshots/lab03-lab-solved.png
```

---

## Result

A publicly accessible backup file exposed application source code containing sensitive database credentials.

The disclosed password was successfully used to solve the lab.

---

# Impact

Successful exploitation could allow an attacker to:

- Access sensitive application source code
- Obtain hard-coded credentials
- Understand internal application logic
- Discover hidden endpoints
- Assist further attacks against the application

---

# Root Cause

A backup copy of the application's source code was stored inside a publicly accessible directory.

The server failed to restrict access to development artifacts, exposing sensitive information to unauthenticated users.

---

# Remediation

- Never store backup or source code files within the web root.
- Remove temporary and development files before deployment.
- Restrict access to sensitive directories.
- Store secrets securely using environment variables or a dedicated secrets manager.
- Perform regular security reviews to identify exposed files.

---

# Key Learning

This lab demonstrates that sensitive information disclosure is not always caused by application vulnerabilities.

Simple deployment mistakes, such as leaving backup files on a production server, can expose source code and confidential credentials that significantly increase the risk of compromise.

---

# References

- OWASP Top 10 2025 – Security Misconfiguration
- PortSwigger Web Security Academy – Source Code Disclosure via Backup Files
