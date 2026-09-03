# Finding: Business Logic Vulnerability — Client-Side Price Manipulation

## Severity

**High**

## Affected Functionality

```http
/cart
```

## Vulnerability Type

**Business Logic Vulnerability — Price Manipulation**

## Description

The application fails to properly enforce the intended pricing logic on the server side. A product price supplied by the client can be modified before the purchase is completed, allowing an authenticated user to purchase a product at an unauthorized price.

The application appears to trust the client-controlled price value instead of retrieving and validating the authoritative product price on the server side.

This allows an attacker to manipulate the transaction workflow and alter the financial value of a purchase.

## Technical Details

During testing, a product was added to the shopping cart and the corresponding HTTP request was intercepted using Burp Suite.

The request contained a parameter representing the product price. The original price value was modified before the request was forwarded to the application.

The application accepted the modified price and subsequently allowed the purchase to be completed using the manipulated value.

## Steps to Reproduce

1. Log in to the application using valid credentials.
2. Navigate to the product listing page.
3. Select a product and add it to the cart.
4. Intercept the relevant cart/purchase request using Burp Suite.
5. Send the captured request to Burp Repeater.
6. Identify the client-controlled parameter containing the product price.
7. Modify the price value to an unauthorized amount.
8. Forward the modified request.
9. Observe that the application accepts the modified price.
10. Complete the purchase.
11. Verify that the transaction is processed using the manipulated price.

## Evidence

### 1. Product Selection

The product is displayed on the application before being added to the cart.

![Product Page](screenshots/01-product-page.png)

---

### 2. Product Added to Cart

The selected product is added to the shopping cart before the request is modified.

![Product Added to Cart](screenshots/02-product-added-to-cart.png)

---

### 3. Original Cart Request

The original HTTP request is captured in Burp Suite and contains the product price value supplied to the application.

![Original Cart Request](screenshots/03-original-cart-request.png)

---

### 4. Modified Price

The client-controlled price parameter is modified to an unauthorized value before forwarding the request.

![Modified Price Request](screenshots/04-modified-price-request.png)

---

### 5. Successful Purchase Using Modified Price

The application accepts the manipulated price and allows the purchase to be completed.

![Successful Purchase](screenshots/05-successful-purchase-with-modified-price.png)

## Impact

Successful exploitation may allow an attacker to:

* Purchase products for an unauthorized price.
* Cause direct financial loss to the business.
* Manipulate transaction values.
* Undermine the integrity of pricing and purchase records.
* Reduce customer trust in the application's purchasing process.

The primary confirmed impact in this assessment is **unauthorized price manipulation resulting in a purchase at a modified price**.

## Root Cause

The application relies on a **client-supplied price value** during the purchase workflow without sufficiently validating the value against the authoritative product price stored on the server.

The server therefore fails to enforce the application's intended business rule that the final transaction price must correspond to the actual price of the selected product.

## Remediation

* Never trust price or other financial values supplied by the client.
* Retrieve the authoritative product price from the server-side data store.
* Recalculate the final transaction amount on the server.
* Validate the product identifier against the current server-side price before completing the transaction.
* Ignore or reject client-supplied price values where they are not required.
* Apply server-side validation to all transaction-related parameters.
* Log and monitor abnormal price manipulation attempts.
* Include business-logic abuse cases in regression and security testing.

## OWASP / Classification Reference

**Business Logic Vulnerability**

This finding represents a failure to enforce an intended business rule within the purchase workflow.

## Environment

This vulnerability was identified in an authorized **PortSwigger Web Security Academy laboratory environment** for educational security testing.
