# Web Server Security Assessment Report

---

# Executive Summary

A web server security assessment was performed against an authorized local deployment of **OWASP Juice Shop** using **Nikto**, an open-source web server vulnerability scanner.

The objective of this assessment was to identify common web server security weaknesses, insecure configurations, exposed resources, and missing security headers. All findings produced by Nikto were reviewed and manually analyzed to distinguish verified issues from informational observations and potential false positives.

This assessment demonstrates the importance of combining automated scanning with manual verification as part of a professional Vulnerability Assessment and Penetration Testing (VAPT) methodology.

---

# Scope

| Property           | Value            |
| ------------------ | ---------------- |
| Target Application | OWASP Juice Shop |
| Environment        | Local Lab        |
| Host               | 10.212.99.96     |
| Port               | 3000             |
| Scanner            | Nikto v2.6.0     |

---

# Objective

The objectives of this assessment were:

* Identify web server security misconfigurations.
* Detect exposed resources and sensitive files.
* Review HTTP security headers.
* Analyze Nikto scan results.
* Manually verify reported findings.
* Produce professional security documentation.

---

# Methodology

The following assessment methodology was followed:

```text id="jzqjlwm"
Target Identification
        ↓
Service Enumeration (Nmap)
        ↓
Manual Browser Inspection
        ↓
Nikto Scan
        ↓
Finding Analysis
        ↓
Manual Verification
        ↓
Risk Assessment
        ↓
Reporting
```

---

# Assessment Summary

The Nikto scan identified multiple security observations.

The findings primarily consisted of:

* Missing HTTP security headers
* Publicly accessible directories
* Potentially exposed files
* Informational HTTP headers
* Possible false positives requiring verification

No critical vulnerability was confirmed solely from the automated scan output.

---

# Key Findings

| Finding                         | Severity              | Status         |
| ------------------------------- | --------------------- | -------------- |
| Missing Content Security Policy | Medium                | Verified       |
| Missing Referrer Policy         | Low                   | Verified       |
| Missing Permissions Policy      | Low                   | Verified       |
| Missing X-Content-Type-Options  | Low                   | Verified       |
| robots.txt                      | Informational         | Verified       |
| Public Directories              | Informational         | Verified       |
| CORS Configuration              | Requires Verification | Reviewed       |
| Exposed History Files           | Requires Verification | Reviewed       |
| JSON Resources                  | Requires Verification | Reviewed       |
| JAMonAdmin.jsp                  | False Positive        | Not Applicable |

---

# Risk Assessment

The majority of findings identified during the assessment represent security hardening recommendations rather than immediately exploitable vulnerabilities.

Potentially exposed files and unrestricted CORS configurations require manual verification before being treated as confirmed security issues.

This highlights the importance of validating automated scanner results during a professional penetration test.

---

# Recommendations

The following recommendations are suggested:

* Implement a Content Security Policy (CSP).
* Configure a Referrer-Policy header.
* Use a Permissions-Policy header where appropriate.
* Configure X-Content-Type-Options with the value `nosniff`.
* Enable HSTS when serving applications over HTTPS.
* Remove unnecessary files from the web root.
* Restrict access to sensitive directories and configuration files.
* Review publicly accessible JSON resources.
* Perform regular vulnerability assessments and manual penetration testing.

---

# Tools Used

* Kali Linux
* Nikto
* Nmap
* Firefox
* Burp Suite Community Edition

---

# Lessons Learned

This assessment reinforced several important concepts:

* Automated scanners provide an efficient starting point for identifying potential issues.
* Scanner output should never be accepted without manual verification.
* Security misconfigurations are often easier to detect than application logic flaws.
* Professional penetration testing requires analysis, validation, and documentation—not just running tools.

---

# Conclusion

The assessment successfully demonstrated the use of Nikto for web server security analysis within a controlled laboratory environment.

While Nikto quickly identified several potential weaknesses, the exercise showed that the value of the tool lies in assisting the tester rather than replacing manual analysis.

A professional VAPT process combines automated scanning, manual verification, evidence collection, risk assessment, and clear reporting to produce reliable security findings.
