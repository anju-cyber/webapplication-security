# Lab Report: User ID Controlled by Request Parameter (Unpredictable User IDs)

**Category:** A01:2025 – Broken Access Control  
**Severity:** High  
**Target:** User Account Management API / Profile Endpoint  
**Platform:** PortSwigger Web Security Academy  

---

## 1. Executive Summary
An Insecure Direct Object Reference (IDOR) vulnerability was identified in the user account module. The application uses unpredictable Globally Unique Identifiers (GUIDs) to reference user accounts, relying on **security through obscurity**. However, it fails to perform server-side authorization checks when loading account data. Once an attacker obtains a target user's GUID (e.g., from a public blog post), they can swap the ID in their request to view and retrieve sensitive data belonging to that user.

---

## 2. Technical Overview

| Attribute | Details |
| --- | --- |
| **Vulnerability Type** | Insecure Direct Object Reference (IDOR) |
| **Affected Parameter** | `id` (Query Parameter) |
| **Authentication Level** | Low-Privileged User |
| **Impact** | Unauthorized Access / Sensitive Data Disclosure |

---

## 3. Methodology & Steps to Reproduce

### Step 1: Identifier Harvesting
1. Navigated to the public blog section of the application.
2. Located a post authored by the target user (`carlos`).
3. Clicked on author details / profile link and observed the URL string.
4. Extracted and recorded the victim's unpredictable user ID (`carlos-guid-here`).

> **Observation:** The application exposes user GUIDs publicly in blog post metadata.

<!-- Screenshot Placeholder: Add your screenshot showing Carlos's GUID from the blog post -->
![Victim GUID Harvest](./screenshots/lab01-guid-harvest.png)

---

### Step 2: Authentication & Session Baseline
1. Logged into the application using low-privileged test credentials (`wiener:peter`).
2. Navigated to the **My Account** section (`/my-account?id=wiener-guid-here`).
3. Observed that the user profile details are loaded based directly on the `id` parameter provided in the request line.

<!-- Screenshot Placeholder: Add your screenshot showing your own account request in Burp/Browser -->
![Own Account Access](./screenshots/lab01-my-account.png)

---

### Step 3: Parameter Tampering & Exploitation
1. Intercepted the `/my-account` request in Burp Suite (or edited the URL directly in the browser).
2. Replaced the `id` parameter value with `carlos`'s previously harvested GUID:
   ```http
   GET /my-account?id=<carlos-guid-here> HTTP/1.1
   Host: target-app.web-security-academy.net
   Cookie: session=<your-session-cookie>
