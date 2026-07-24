# Lab Solution

## Objective

Identify and exploit a Reflected Cross-Site Scripting (XSS) vulnerability where most HTML tags and attributes are blocked.

---

# Manual Testing

## Step 1 – Access the Vulnerable Application

Opened the lab and identified the search functionality for testing user-controlled input.

**Screenshot 01 – Normal Website**

---

## Step 2 – Verify Input Reflection

Entered a test value and confirmed that the application reflected the supplied input.

**Screenshot 02 – Reflection Input**

---

## Step 3 – Analyze HTML Context

Inspected the HTML response using the browser's Developer Tools.

The reflected input was stored inside the HTML context, allowing further payload analysis.

**Screenshot 03 – HTML Context**

---

## Step 4 – Test Initial Payload

Tested the following payload:

```html
<script>alert(00)</script>
```

The payload was reflected but did not execute successfully.

**Screenshot 04 – Initial Payload**

---

## Step 5 – Identify Allowed HTML Tags

Performed tag brute-force testing to determine which HTML elements were accepted by the application.

**Screenshot 05 – Tag Brute Force**

---

## Step 6 – Find the Allowed Tag

After testing multiple tags, an allowed HTML tag was identified.

**Screenshot 06 – Allowed Tag**

---

## Step 7 – Identify Allowed Attributes

Performed attribute brute-force testing against the allowed HTML tag.

**Screenshot 07 – Attribute Brute Force**

---

## Step 8 – Construct the Payload

Using the allowed HTML tag and supported attribute, a new payload was created.

**Screenshot 08 – Allowed Tag**

**Screenshot 09 – Final Payload**

---

## Step 9 – Successful Execution

The payload executed successfully, confirming the Reflected XSS vulnerability.

**Screenshot 10 – Successful Execution**

-----------------------------------------------------------------------------------------

# Automated Verification Using XSStrike

After manually identifying the vulnerability, XSStrike was used to analyze the same injection point.

The tool generated multiple payloads suitable for the detected HTML context.

Several generated payloads were tested manually and successfully executed.

**Screenshot 11 – XSStrike**

----------------------------------------------------------------------

# Result

- User input reflection confirmed.
- HTML context analyzed.
- Allowed HTML tag identified.
- Allowed HTML attribute identified.
- Working XSS payload created.
- JavaScript executed successfully.
- Vulnerability verified using XSStrike.

---

# Conclusion

This lab demonstrates the importance of understanding HTML context before selecting payloads. Manual testing provided insight into the application's filtering behavior, while XSStrike accelerated payload generation and verification.
