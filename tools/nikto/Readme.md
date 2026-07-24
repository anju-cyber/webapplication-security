# Nikto Web Server Security Assessment

## Overview

This repository documents my practical learning and hands-on experience with **Nikto**, an open-source web server vulnerability scanner widely used during web application penetration testing.

The objective of this project is not only to learn Nikto commands but also to understand how to analyze scan results, distinguish between real findings and false positives, perform manual verification, and document vulnerabilities in a professional manner.

The assessment was performed against an **authorized local OWASP Juice Shop instance** running in a controlled lab environment.

---

# Project Objectives

* Understand Nikto's role in a VAPT methodology.
* Perform web server vulnerability scanning.
* Analyze Nikto scan results.
* Manually verify reported findings.
* Differentiate between vulnerabilities, security misconfigurations, informational findings, and false positives.
* Produce professional penetration testing documentation.

---

# Target Environment

| Property           | Value            |
| ------------------ | ---------------- |
| Target Application | OWASP Juice Shop |
| Environment        | Local Lab        |
| Target Host        | 10.212.99.96     |
| Target Port        | 3000             |
| Protocol           | HTTP             |

---

# Tools Used

* Kali Linux
* Nikto
* Nmap
* Firefox
* Burp Suite Community Edition

---

# Repository Structure

```text
nikto-web-server-security-assessment/

│
├── README.md
├── Notes.md
├── Practical.md
├── Findings.md
├── Report.md
├── nikto.txt
└── screenshots/
```

---

# Assessment Methodology

The assessment followed the workflow below:

```text
Target Identification
        ↓
Port Discovery (Nmap)
        ↓
Manual Browsing
        ↓
Nikto Scan
        ↓
Finding Analysis
        ↓
Manual Verification
        ↓
Risk Assessment
        ↓
Professional Reporting
```

---

# Learning Outcomes

After completing this project, I was able to:

* Explain how Nikto works internally.
* Perform web server security assessments.
* Understand Nikto scan results.
* Verify findings manually instead of relying solely on automated output.
* Identify common web server security misconfigurations.
* Create professional VAPT documentation suitable for a GitHub portfolio.

---

# Disclaimer

This assessment was performed only against an intentionally vulnerable application in a controlled laboratory environment for educational purposes. No unauthorized systems were scanned or tested.
