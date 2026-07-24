# Practical Assessment

## Objective

Perform a web server security assessment against an authorized OWASP Juice Shop instance using Nikto and manually verify the reported findings.

---

# Target Information

| Property | Value            |
| -------- | ---------------- |
| Target   | OWASP Juice Shop |
| Host     | 10.212.99.96     |
| Port     | 3000             |
| Protocol | HTTP             |

---

# Testing Methodology

The assessment followed the workflow below:

```text
Target Identification
        ↓
Service Enumeration (Nmap)
        ↓
Manual Browser Inspection
        ↓
Nikto Scan
        ↓
Result Analysis
        ↓
Manual Verification
        ↓
Evidence Collection
        ↓
Documentation
```

---

# Step 1 – Verify the Target

Before running Nikto, the target application was opened in a web browser to confirm that the web service was accessible.

**Evidence**

home page website- screenshorts/01-homepage.png

---

# Step 2 – Perform Service Enumeration

Nmap was used to identify the running web service and confirm that the application was listening on TCP port 3000.

Command:

```bash
nmap -sV 10.212.99.96
```

Purpose:

* Identify open ports.
* Detect the running service.
* Confirm the correct target for Nikto.

**Evidence**

nmap_scan screenshots/02-nmap-scan.png

---

# Step 3 – Execute Nikto Scan

The following command was used:

```bash
nikto -h http://10.212.99.96:3000 -o nikto.txt
```

Purpose:

* Scan the target web server.
* Identify security misconfigurations.
* Detect exposed files.
* Check for missing security headers.
* Produce a text report for analysis.

**Evidence**

nikto command and outputscreensorts/03-nikto.png
---

# Step 4 – Review the Scan Results

After the scan completed, the generated report (`nikto.txt`) was reviewed.

The scan identified several observations, including:

* Missing security headers.
* Exposed files and directories.
* robots.txt
* Interesting JSON files.
* HTTP response header information.
* Potential sensitive resources.

At this stage, no finding was considered a confirmed vulnerability.

---

# Step 5 – Manual Verification

Each Nikto finding was manually verified using a web browser and Burp Suite.

The objective was to determine whether the reported issue was:

* A confirmed finding.
* An informational observation.
* A false positive.

Manual verification reduces false positives and improves report accuracy.

---

# Testing Outcome

The assessment demonstrated that Nikto is effective for quickly identifying potential web server security issues.

However, the scan also showed that automated scanners alone cannot determine the actual security impact of every finding.

Professional penetration testing requires manual verification before reporting any issue as a confirmed vulnerability.

---

# Key Learning

* Nikto accelerates reconnaissance and web server assessment.
* Automated findings require manual validation.
* Missing security headers are often security hardening recommendations rather than direct vulnerabilities.
* Scanner output should always be analyzed instead of being copied directly into a report.
