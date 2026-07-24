# Lab 01 – Querying the Database Version (Oracle)

## Overview

This lab demonstrates a **UNION-Based SQL Injection** vulnerability in an Oracle database. The objective is to retrieve the Oracle database version by exploiting an injectable parameter in the application's SQL query.

The lab was solved using two different approaches:

* **Manual SQL Injection**
* **SQLMap Automation**

This documentation demonstrates both the manual exploitation methodology and the automated validation process using SQLMap.

---

## Lab Information

| Property      | Value                                                     |
| ------------- | --------------------------------------------------------- |
| Platform      | PortSwigger Web Security Academy                          |
| Category      | SQL Injection                                             |
| Difficulty    | Apprentice                                                |
| Database      | Oracle                                                    |
| Vulnerability | UNION-Based SQL Injection                                 |
| Tools Used    | Burp Suite Community Edition, SQLMap, Firefox, Kali Linux |

---------------------------------------------------------------------------------

## Lab Objective

Retrieve the Oracle database version by exploiting a UNION-based SQL Injection vulnerability.

---

## Learning Objectives

After completing this lab, I was able to:

* Understand how UNION-based SQL Injection works.
* Identify an injectable parameter manually.
* Determine the correct number of columns using `ORDER BY`.
* Use `UNION SELECT` to retrieve database information.
* Understand Oracle-specific objects such as `DUAL`, `v$version`, and the `BANNER` column.
* Capture HTTP requests using Burp Suite.
* Validate the vulnerability using SQLMap.
* Fingerprint the backend Oracle database using SQLMap.

---

## Repository Structure

```text
Lab-01-Querying-Database-Version-Oracle/

├── README.md
├── Manual.md
├── SQLMap.md
├── Report.md
├── request.txt
└── screenshots/
```

---

## Manual Testing

The vulnerability was first identified manually by:

1. Identifying the injectable parameter.
2. Triggering an SQL error using a single quote (`'`).
3. Determining the number of columns using `ORDER BY`.
4. Using `UNION SELECT` to retrieve the Oracle database version.
5. Confirming successful exploitation.

Complete details are available in **Manual.md**.

---

## SQLMap Validation

After confirming the vulnerability manually, SQLMap was used to:

* Detect SQL Injection.
* Fingerprint the Oracle database.
* Retrieve the database banner.
* Demonstrate additional database enumeration commands for learning purposes.

Complete details are available in **SQLMap.md**.

---

## Key Learning

This lab demonstrates that manual testing should always be performed before relying on automated tools. SQLMap is highly effective for validating findings, fingerprinting databases, and automating enumeration, but understanding the underlying SQL Injection technique remains essential for professional penetration testing.
