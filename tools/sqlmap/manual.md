# Manual Exploitation

## Objective

Retrieve the Oracle database version by manually exploiting a UNION-Based SQL Injection vulnerability.

---

# Lab Overview

The application was vulnerable to a UNION-Based SQL Injection because user-supplied input was directly incorporated into an SQL query without proper validation or parameterized statements.

The objective of the manual assessment was to identify the vulnerability, determine the query structure, and retrieve the Oracle database version.

---

# Understanding the Vulnerability

## Vulnerability Type

**UNION-Based SQL Injection**

## Database

**Oracle Database**

## Root Cause

The application failed to properly validate user input before using it in an SQL query. As a result, user-controlled input modified the original SQL statement executed by the database.

---

# Manual Testing Methodology

## Step 1 – Identify the Injection Point

The application accepted user input through a URL parameter.

A single quote (`'`) was appended to the parameter to observe how the application responded.

### Observation

The application returned an Internal Server Error, indicating that the user input was affecting the SQL query.

*(Insert Screenshot: SQL Error)*

---

## Step 2 – Determine the Number of Columns

The `ORDER BY` clause was used to determine the number of columns returned by the original SQL query.

Example approach:

```sql
ORDER BY 1--
ORDER BY 2--
ORDER BY 3--
```

### Why ORDER BY?

`ORDER BY` helps determine the number of columns in the original query. This information is required before performing a successful UNION attack.

*(Insert Screenshot: ORDER BY Testing)*

---

## Step 3 – Verify UNION Injection

After identifying the correct number of columns, a UNION SELECT statement was used.

Example approach:

```sql
UNION SELECT NULL,NULL--
```

### Why NULL?

`NULL` is compatible with most SQL data types. It allows the tester to identify the correct number of columns without worrying about data type mismatches.

*(Insert Screenshot: UNION NULL Testing)*

---

## Step 4 – Oracle Specific Query

Oracle requires every SELECT statement to include a FROM clause.

The special Oracle table `DUAL` was used.

Example approach:

```sql
UNION SELECT banner,NULL FROM v$version--
```

### Why FROM DUAL?

`DUAL` is a special one-row system table provided by Oracle. It is commonly used when selecting constants or expressions without referencing an application table.

### Why v$version?

`v$version` is an Oracle dynamic performance view that stores version information about the Oracle database.

### Why BANNER?

The `BANNER` column contains the human-readable Oracle database version string.

*(Insert Screenshot: Oracle Version Retrieved)*

---

# Result

The Oracle database version was successfully retrieved using a UNION-Based SQL Injection.

The application was confirmed to be vulnerable.

---

# Impact

An attacker could exploit this vulnerability to retrieve sensitive database information. Depending on the application's privileges, further enumeration and data extraction could also be possible.

---

# Mitigation

* Use parameterized queries (prepared statements).
* Validate and sanitize user input.
* Apply the principle of least privilege to database accounts.
* Implement secure error handling.
* Perform regular security testing.

---

# Key Learning

* Manual testing provides a clear understanding of how SQL Injection works.
* `ORDER BY` identifies the number of columns.
* `UNION SELECT` combines results from two queries.
* `NULL` is used to avoid data type conflicts.
* Oracle requires the `DUAL` table for standalone SELECT statements.
* `v$version` contains Oracle version information.
* `BANNER` stores the readable database version.
