# Nikto – Complete Notes

---

# What is Nikto?

Nikto is an open-source web server vulnerability scanner written in Perl. It is designed to identify known vulnerabilities, insecure configurations, outdated software, exposed files, and common security weaknesses on HTTP and HTTPS servers.

Nikto is commonly used during the reconnaissance and vulnerability assessment phases of a web application penetration test.

---

# Purpose

The primary objective of Nikto is to automate the identification of web server security issues.

Nikto helps penetration testers quickly discover:

* Dangerous files
* Backup files
* Misconfigurations
* Default content
* Missing security headers
* Outdated software
* Known vulnerabilities

Nikto does **not** exploit vulnerabilities. It identifies potential issues that must be manually verified.

---

# How Nikto Works

```text
Target

↓

Connect to Web Server

↓

Identify Server

↓

Load Plugins

↓

Send HTTP Requests

↓

Analyze Responses

↓

Compare with Vulnerability Database

↓

Generate Findings
```

---

# Internal Architecture

```text
User

↓

Command Parser

↓

HTTP Engine

↓

Plugin Engine

↓

Vulnerability Database

↓

Response Analyzer

↓

Report Generator
```

---

# Scan Workflow

Professional workflow:

```text
Target

↓

Nmap

↓

Browser

↓

Burp Suite

↓

Nikto

↓

Manual Verification

↓

Evidence Collection

↓

Report
```

---

# Why Manual Verification is Important

Nikto reports **potential** findings.

Not every reported issue is an actual vulnerability.

Each finding must be manually verified before including it in a penetration testing report.

This reduces false positives and improves report accuracy.

---

# Common Findings

Nikto frequently reports:

* Missing Content Security Policy
* Missing HSTS
* Missing Referrer Policy
* Missing Permissions Policy
* Missing X-Content-Type-Options
* robots.txt
* Backup files
* Hidden directories
* Dangerous HTTP methods
* Default files
* Configuration weaknesses

---

# Advantages

* Free and open source
* Easy to use
* Fast deployment
* Plugin-based
* Large vulnerability database
* Supports HTTP and HTTPS
* Generates multiple report formats

---

# Limitations

* Cannot exploit vulnerabilities
* Generates false positives
* Easily detected by IDS/IPS
* Produces noisy scans
* Limited authentication capabilities
* Should not replace manual testing

---

# Important Commands

## Basic Scan

```bash
nikto -h TARGET
```

---

## HTTPS Scan

```bash
nikto -h https://TARGET
```

---

## Specific Port

```bash
nikto -h TARGET -p 3000
```

---

## Force SSL

```bash
nikto -h TARGET -ssl
```

---

## HTML Report

```bash
nikto -h TARGET -o report.html -Format html
```

---

## Text Report

```bash
nikto -h TARGET -o nikto.txt
```

---

## CSV Report

```bash
nikto -h TARGET -o report.csv -Format csv
```

---

## Update Database

```bash
nikto -update
```

---

## Display Version

```bash
nikto -Version
```

---

# Best Practices

* Verify findings manually.
* Scan only authorized targets.
* Update the Nikto database before assessments.
* Review HTTP headers manually.
* Combine Nikto with Nmap and Burp Suite.
* Document only verified findings.

---

# Nikto vs Other Tools

| Tool       | Primary Purpose                       |
| ---------- | ------------------------------------- |
| Nikto      | Web server vulnerability scanning     |
| Nmap       | Host and service discovery            |
| Burp Suite | Manual web application testing        |
| SQLMap     | SQL Injection automation              |
| Gobuster   | Directory and file discovery          |
| ffuf       | Fast content fuzzing                  |
| Nuclei     | Template-based vulnerability scanning |
| Nessus     | Enterprise vulnerability assessment   |

---
