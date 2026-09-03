# Business Logic Vulnerability — Price Manipulation

This folder documents a **Business Logic Vulnerability** identified during an authorized PortSwigger Web Security Academy laboratory exercise.

The vulnerability allows an authenticated user to manipulate a client-controlled product price during the purchase workflow. The application accepts the modified value instead of enforcing the authoritative product price on the server side.

## Finding Overview

| Field                  | Details                              |
| ---------------------- | ------------------------------------ |
| Vulnerability          | Business Logic Vulnerability         |
| Attack Type            | Price Manipulation                   |
| Severity               | High                                 |
| Affected Functionality | `/cart`                              |
| Testing Tool           | Burp Suite Professional              |
| Environment            | PortSwigger Web Security Academy Lab |
| Status                 | Confirmed                            |

## Attack Flow

```text
Select Product
      ↓
Add Product to Cart
      ↓
Capture HTTP Request
      ↓
Identify Price Parameter
      ↓
Modify Product Price
      ↓
Send Modified Request
      ↓
Application Accepts Modified Price
      ↓
Purchase Completed
```

## Key Security Observation

The application trusts a client-controlled price value during the purchase workflow instead of independently determining and validating the authoritative product price on the server side.

This allows the intended pricing rule to be bypassed.

## Evidence

The detailed evidence is available in [`report.md`](./report.md).

### Screenshots

* `01-product-page.png` — Product selection page.
* `02-product-added-to-cart.png` — Product added to the shopping cart.
* `03-original-cart-request.png` — Original HTTP request containing the price value.
* `04-modified-price-request.png` — Modified price submitted to the application.
* `05-successful-purchase-with-modified-price.png` — Successful transaction using the manipulated price.

## Recommended Remediation

* Validate all transaction values server-side.
* Retrieve product prices from the authoritative server-side data source.
* Recalculate transaction totals on the server.
* Do not trust client-supplied financial values.
* Monitor and log suspicious transaction manipulation attempts.

## Classification

**Business Logic Vulnerability — Price Manipulation**

## Lab Disclaimer

This vulnerability was identified in an authorized PortSwigger Web Security Academy laboratory environment for educational and defensive security testing.
