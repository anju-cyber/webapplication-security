# Lab 01 - Excessive Trust in Client-Side Controls

## Lab Information

| Field | Value |
|-------|-------|
| Category | Insecure Design |
| Vulnerability | Excessive Trust in Client-Side Controls |
| Platform | PortSwigger Web Security Academy |
| Difficulty | Apprentice |
| Status | ✅ Solved |

---

## Objective

Demonstrate how relying on client-side controls for business logic can allow attackers to manipulate product prices and purchase items below their intended cost.

---

## Vulnerability Overview

Applications should never trust data received from the client. Values such as product prices, discounts, account balances, and user roles must always be validated on the server.

In this lab, the application trusts the price value supplied by the client when adding an item to the shopping cart. By intercepting and modifying the request, an attacker can reduce the product price and complete a purchase without sufficient store credit.

---

## Lab Scenario

The application provides an online store where users can purchase products using store credit.

Initially, the leather jacket cannot be purchased because the available credit is insufficient. However, the application sends the product price as part of the client request. By modifying this value before it reaches the server, the item can be purchased at a significantly lower price.

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

Opened the online store and reviewed the available products.

Attempted to purchase the leather jacket using the available store credit.

The purchase failed because the account balance was lower than the product price.

**Screenshot**

```
screenshots/lab-01-home_page.png
```

---

### Step 2 – Capture the Shopping Cart Request

Intercepted the request responsible for adding the product to the shopping cart.

Observed that the request contained a user-controlled `price` parameter.

The request was sent to Burp Repeater for further testing.

**Screenshot**

```
screenshots/lab-01-cart_request.png
```

---

### Step 3 – Modify the Price

Edited the value of the `price` parameter and replaced it with a much smaller amount.

The modified request was forwarded to the server.

**Screenshot**

```
screenshots/lab-01-request_modify.png
```

---

### Step 4 – Verify the Price Change

Refreshed the shopping cart after forwarding the modified request.

The application accepted the manipulated value and displayed the updated product price, confirming that pricing was being trusted from the client.

**Screenshot**

```
screenshots/lab-01-modification_success.png
```

---

### Step 5 – Complete the Purchase

With the reduced price now below the available store credit, completed the checkout process successfully.

The lab was marked as solved.

**Screenshot**

```
screenshots/lab-01-lab_solved.png
```

---

## Result

The application trusted client-supplied pricing information without performing server-side validation.

By modifying the `price` parameter, the product cost was reduced, allowing the purchase to be completed using insufficient store credit.

The lab was successfully completed.

---

## Attack Details

| Property | Value |
|----------|-------|
| Vulnerability | Client-Side Price Manipulation |
| Injection Point | `price` Parameter |
| Business Logic Issue | Trusting Client Input |
| Impact | Unauthorized Price Modification |

---

# Impact

Successful exploitation could allow an attacker to:

- Purchase products at arbitrary prices
- Abuse promotional discounts
- Manipulate payment values
- Cause financial losses to the business
- Exploit weaknesses in business logic

---

# Root Cause

The application relied on a client-controlled parameter to determine the product price.

Instead of calculating the correct price on the server, the application accepted the value supplied by the client without validation.

---

# Remediation

- Never trust client-supplied values for pricing or business logic.
- Calculate product prices exclusively on the server.
- Validate all payment-related values before processing transactions.
- Store product pricing securely in the backend database.
- Perform regular security testing for business logic vulnerabilities.

---

# Key Learning

This lab demonstrates that security vulnerabilities are not always caused by technical flaws such as SQL Injection or Cross-Site Scripting. Weak business logic can be equally dangerous.

Critical values such as product prices should always be generated and verified on the server. Any information received from the client should be treated as untrusted and validated before use.

---

# References

- OWASP Top 10 2025 – Insecure Design
- OWASP Secure Coding Practices
- PortSwigger Web Security Academy – Excessive Trust in Client-Side Controls
