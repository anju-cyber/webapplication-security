# SQLMap Validation

## Objective

Validate the manually identified SQL Injection vulnerability using SQLMap and demonstrate automated database fingerprinting.

---

# Why SQLMap?

Manual testing confirmed the SQL Injection vulnerability. SQLMap was then used to automate the validation process, fingerprint the backend database, and demonstrate database enumeration techniques.

---

# Capturing the Request

The vulnerable HTTP request was intercepted using Burp Suite and saved as `request.txt`.

Using the saved request ensures that SQLMap sends the same HTTP method, headers, cookies, and parameters as the browser.

---

# SQLMap Workflow

## Step 1 – Detect SQL Injection

```bash
sqlmap -r request.txt
```

### Purpose

Detect whether the captured request is vulnerable to SQL Injection.

### Expected Outcome

* Parameter testing
* SQL Injection confirmation
* Backend database identification

### SQLMap Detection

![SQLMap Detection](screenshots/06-sqlmap-detection.png)

---

## Step 2 – Fingerprint the Database

```bash
sqlmap -r request.txt --banner
```

### Purpose

Retrieve the Oracle database banner.

### Why?

The database banner provides version information that helps identify the backend DBMS.

### Database Banner

![Database Banner](screenshots/07-sqlmap-banner.png)

---

## Step 3 – Identify Current Database

```bash
sqlmap -r request.txt --current-db
```

### Purpose

Retrieve the name of the currently selected database.

### Learning

Understanding the active database is the first step before further enumeration.

### Current Database

![Current Database](screenshots/08-current-db.png)

---

## Step 4 – Identify Current Database User

```bash
sqlmap -r request.txt --current-user
```

### Purpose

Identify the database account used by the application.

### Current User

![Current User](screenshots/09-current-user.png)

---

## Step 5 – Enumerate Databases

```bash
sqlmap -r request.txt --dbs
```

### Purpose

List the databases that are accessible to the current database user.

> Note: This step demonstrates SQLMap's enumeration capabilities. It extends beyond the original objective of this PortSwigger lab.

### Database Enumeration

![Database Enumeration](screenshots/10-database-enumeration.png)
---

## Step 6 – Enumerate Tables

```bash
sqlmap -r request.txt -D DATABASE --tables
```

### Purpose

Retrieve the tables from a selected database.

### Table Enumeration

![Table Enumeration](screenshots/11-table-enumeration.png)

---

## Step 7 – Enumerate Columns

```bash
sqlmap -r request.txt -D DATABASE -T TABLE --columns
```

### Purpose

Retrieve the columns of a selected table.

### Column Enumeration

![Column Enumeration](screenshots/12-column-enumeration.png)

---

# Manual Testing vs SQLMap

| Manual Testing                                    | SQLMap                                      |
| ------------------------------------------------- | ------------------------------------------- |
| Identified the SQL Injection manually             | Automatically detected the SQL Injection    |
| Determined the number of columns using `ORDER BY` | Automatically generated detection payloads  |
| Retrieved the Oracle version using `UNION SELECT` | Retrieved the database banner automatically |
| Required manual analysis                          | Automated detection and enumeration         |

---

# Advantages of SQLMap

* Fast vulnerability validation.
* Automatic database fingerprinting.
* Supports multiple SQL Injection techniques.
* Simplifies database enumeration.
* Saves HTTP sessions for later analysis.
* Integrates well with Burp Suite.

---

# Limitations

* SQLMap should not replace manual testing.
* Automated tools may produce false positives or require confirmation.
* Testing must always remain within the authorized scope.

---

# Key Learning

This lab demonstrated how SQLMap can efficiently validate a manually discovered SQL Injection vulnerability. While manual testing provided an understanding of the exploitation process, SQLMap simplified verification and database fingerprinting, making it a valuable tool during authorized web application penetration tests.
