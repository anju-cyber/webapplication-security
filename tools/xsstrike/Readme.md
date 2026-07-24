# Reflected XSS into HTML Context with Most Tags and Attributes Blocked

## Overview

This repository documents the successful exploitation of a **Reflected Cross-Site Scripting (XSS)** vulnerability in the PortSwigger Web Security Academy lab **"Reflected XSS into HTML context with most tags and attributes blocked."**

The lab was solved using two different approaches:

- Manual testing and analysis
- Automated verification using XSStrike

The objective of this project is to understand how HTML context affects payload execution and how manual analysis can be combined with automated tools during web application security testing.

---

## Lab Information

| Category | Details |
|----------|---------|
| Platform | PortSwigger Web Security Academy |
| Vulnerability | Reflected Cross-Site Scripting (Reflected XSS) |
| Lab | Reflected XSS into HTML context with most tags and attributes blocked |
| Testing Method | Manual + XSStrike |
| Status | Successfully Solved |

---

## Tools Used

- Burp Suite Professional
- Firefox Browser
- Browser Developer Tools
- XSStrike
- Kali Linux

---

## Repository Structure

```
.
├── README.md
├── NOTES.md
├── LAB-SOLUTION.md
├── REPORT/
│   └── xsstrike_git.docx
└── screenshots/
```

---

## Learning Objectives

- Understand reflected XSS
- Analyze HTML injection context
- Identify allowed HTML tags
- Identify allowed HTML attributes
- Create a working XSS payload manually
- Verify the vulnerability using XSStrike

---

## Disclaimer

This project was completed in the PortSwigger Web Security Academy lab for educational and authorized security testing purposes only.
