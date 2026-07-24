# Penetration Testing Report

## Finding Summary

| Field         | Value                                                                                        |
| ------------- | -------------------------------------------------------------------------------------------- |
| Finding ID    | SQLI-001                                                                                     |
| Vulnerability | UNION-Based SQL Injection                                                                    |
| Severity      | High                                                                                         |
| CWE           | CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') |
| OWASP Top 10  | A03:2021 – Injection                                                                         |
| Database      | Oracle                                                                                       |
| Status        | Confirmed                                                                                    |

---

# Executive Summary

During the security assessment of the target application, a UNION-Based SQL Injection vulnerability was identified within a user-controlled parameter. The application failed to properly validate user input before incorporating it into an SQL query.

The vulnerability allowed SQL queries to be modified, enabling retrieval of information directly from the Oracle database. Manual testing successfully extracted the database version, and the finding was subsequently validated using SQLMap.

---

# Vulnerability Description

SQL Injection occurs when an application incorporates untrusted user input directly into SQL statements without proper validation or parameterized queries.

In this lab, the vulnerable parameter accepted SQL syntax supplied by the user. By using the SQL `UNION` operator, it was possible to append a second query and retrieve data from Oracle's internal `v$version` view.

---

# Affected Component

| Item                 | Value                                |
| -------------------- | ------------------------------------ |
| Application          | PortSwigger Web Security Academy Lab |
| Vulnerable Parameter | Category Parameter                   |
| Request Type         | HTTP GET                             |
| Database             | Oracle                               |

---

# Proof of Concept

## Manual Exploitation

The following methodology was used:

1. Identified the injectable parameter.
2. Triggered an SQL error using a single quote (`'`).
3. Determined the number of columns using `ORDER BY`.
4. Verified UNION compatibility using `UNION SELECT NULL`.
5. Retrieved the Oracle database version from `v$version`.

Refer to **Manual.md** for the complete walkthrough.

---

## SQLMap Validation

The vulnerability was validated using SQLMap with the captured HTTP request.

Commands performed included:

```bash
sqlmap -r request.txt
sqlmap -r request.txt --banner
sqlmap -r request.txt --current-db
sqlmap -r request.txt --current-user
```

SQLMap successfully fingerprinted the backend Oracle database and retrieved version information, confirming the manual findings.

Refer to **SQLMap.md** for the detailed command explanations.

---

# Risk

A successful attacker could potentially:

* Retrieve sensitive database information.
* Enumerate databases and tables.
* Extract application data.
* Escalate the impact depending on database permissions.

Although this lab focuses on version retrieval, similar vulnerabilities in production environments may result in complete database compromise.

---

# Business Impact

If exploited in a production environment, this vulnerability could lead to:

* Exposure of confidential business data.
* Unauthorized access to customer information.
* Regulatory compliance violations.
* Reputational damage.
* Financial losses.

---

# Remediation

The following security controls are recommended:

* Use parameterized queries (prepared statements).
* Validate and sanitize all user input.
* Apply least-privilege permissions to database accounts.
* Disable verbose SQL error messages.
* Conduct regular secure code reviews and penetration testing.

---

# Tools Used

* Burp Suite Community Edition
* SQLMap
* Firefox
* Kali Linux

---

# References

* OWASP SQL Injection Prevention Cheat Sheet
* PortSwigger Web Security Academy – SQL Injection
* Oracle Database Documentation
* SQLMap Official Documentation

---

# Conclusion

The application was confirmed to be vulnerable to UNION-Based SQL Injection. Manual testing successfully demonstrated exploitation, while SQLMap validated the finding and automated database fingerprinting. This exercise highlights the importance of secure coding practices and demonstrates how automated tools should complement—not replace—manual penetration testing.
