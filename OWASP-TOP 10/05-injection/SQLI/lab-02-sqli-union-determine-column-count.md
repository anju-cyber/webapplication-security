# Lab 02 - SQL Injection UNION Attack: Determining the Number of Columns

## Lab Information

| Field | Value |
|-------|-------|
| Category | Injection |
| Subcategory | SQL Injection |
| Vulnerability | UNION-Based SQL Injection |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Determine the number of columns returned by the application's SQL query using the `ORDER BY` technique as preparation for a UNION-based SQL Injection attack.

---

## Vulnerability Overview

Before performing a UNION-based SQL Injection attack, an attacker must identify how many columns are returned by the original SQL query.

One common technique is to inject an `ORDER BY` clause with increasing column numbers. If the supplied column number exceeds the number of columns in the query, the database returns an error. This allows the attacker to determine the correct column count.

---

## Lab Scenario

The application filters products using a `category` parameter.

During testing, the parameter was found to be vulnerable to SQL Injection. By modifying the parameter and gradually increasing the column number in an `ORDER BY` clause, the number of columns used by the SQL query was successfully identified.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Mozilla Firefox
- PortSwigger Web Security Academy

---

## Testing Steps

### Step 1 – Browse the Application

Opened the application homepage and selected one of the available product categories.

**Screenshot**

```
screenshots/lab02-home_page.png
```

---

### Step 2 – Intercept the Request

Captured the request responsible for filtering products by category.

The request was sent to Burp Repeater for further testing.

**Screenshot**

```
screenshots/lab02-product_category.png
```

---

### Step 3 – Test the ORDER BY Clause

Modified the `category` parameter by injecting an `ORDER BY` clause.

Example:

```text
'+ORDER+BY+1--
```

The application processed the request successfully, indicating that the first column existed.

**Screenshot**

```
screenshots/lab02-column_1_exist.png
```

---

### Step 4 – Increase the Column Number

Gradually increased the column number within the injected `ORDER BY` clause.

When the supplied column number exceeded the number of columns returned by the query, the application generated a database error.

**Screenshot**

```
screenshots/lab02-column-2-error.png
```

---

### Step 5 – Determine the Correct Column Count

Repeated the process until the response no longer generated an error.

The successful request identified the exact number of columns returned by the SQL query, completing the lab.

**Screenshot**

```
screenshots/lab02-error_msg.png
```

---

## Result

The application was vulnerable to SQL Injection.

By manipulating the `ORDER BY` clause, the number of columns returned by the SQL query was successfully identified, enabling future UNION-based SQL Injection attacks.

The lab was successfully completed.

---

## Injection Details

| Property | Value |
|----------|-------|
| Injection Point | `category` Parameter |
| Injection Type | UNION-Based SQL Injection |
| Technique Used | ORDER BY Enumeration |
| Goal | Determine Number of Query Columns |

---

# Impact

Successful exploitation could allow an attacker to:

- Determine the structure of SQL queries
- Prepare UNION-based SQL Injection attacks
- Extract data from backend databases
- Enumerate database schemas
- Retrieve sensitive application data

---

# Root Cause

The application incorporated user-controlled input directly into SQL statements without proper validation or parameterized queries.

As a result, SQL keywords supplied by the user were interpreted as part of the database query.

---

# Remediation

- Use parameterized queries (prepared statements) for all database operations.
- Validate user input before processing database queries.
- Apply allow-list validation where appropriate.
- Avoid exposing detailed database error messages.
- Perform regular security testing for SQL Injection vulnerabilities.

---

# Key Learning

This lab demonstrates an important reconnaissance step in UNION-based SQL Injection.

Rather than immediately extracting data, the first objective is to understand the structure of the underlying SQL query. Determining the correct number of columns is essential before a successful UNION SELECT attack can be performed.

---

# References

- OWASP Top 10 2025 – Injection
- OWASP SQL Injection Prevention Cheat Sheet
- PortSwigger Web Security Academy – SQL Injection UNION Attack: Determining the Number of Columns Returned by the Query
