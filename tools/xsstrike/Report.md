Reflected XSS into HTML Context with Most Tags and Attributes Blocked
PortSwigger Web Security Academy
1. Objective

The objective of this lab was to identify and exploit a Reflected Cross-Site Scripting (XSS) vulnerability where most HTML tags and attributes are filtered. The assessment focused on understanding how user input is reflected within the HTML response, identifying the permitted HTML elements, constructing a valid payload, and verifying the vulnerability using both manual testing and the XSStrike tool.

2. Lab Information
Item	Details
Platform	PortSwigger Web Security Academy
Vulnerability	Reflected Cross-Site Scripting (Reflected XSS)
Lab Name	Reflected XSS into HTML context with most tags and attributes blocked
Testing Method	Manual Testing and XSStrike
Environment	PortSwigger Lab
Result	Successfully Exploited
3. Tools Used
Burp Suite Professional
Firefox Browser
Browser Developer Tools
XSStrike
Kali Linux
4. Manual Exploitation
Step 1 – Access the Lab

The vulnerable web application was opened, and the search functionality was selected as the primary input point for testing.

Reference: Screenshot 01 – Normal Website

Step 2 – Verify Input Reflection

A test value was entered into the search field to determine whether the application reflected user-controlled input.

The application reflected the supplied value, confirming that the input reached the HTML response.

Reference: Screenshot 02 – Reflection Input

Step 3 – Analyze the HTML Context

The browser's Developer Tools were used to inspect the response.

The reflected value was observed inside the HTML context, providing the information required to continue payload testing.

Reference: Screenshot 03 – HTML Context

Step 4 – Test the Initial Payload

The following payload was tested:

<script>alert(00)</script>

The payload was reflected but did not execute successfully, indicating that the application restricted the use of the <script> tag.

Reference: Screenshot 04 – Initial Payload

Step 5 – Identify Allowed HTML Tags

To determine which HTML elements were accepted, tag brute-force testing was performed.

This process helped identify an HTML tag that was not blocked by the application's filtering mechanism.

Reference: Screenshot 05 – Tag Brute Force

Step 6 – Confirm the Allowed Tag

After testing multiple HTML tags, an allowed tag was identified for further exploitation.

Reference: Screenshot 06 – Allowed Tag

Step 7 – Identify Allowed Attributes

Once the allowed HTML tag was identified, attribute brute-force testing was performed to determine which event attributes were accepted.

Reference: Screenshot 07 – Attribute Brute Force

Step 8 – Construct the Final Payload

Using the identified HTML tag together with a supported attribute, a new payload was created and tested.

Reference:

Screenshot 08 – Allowed Tag
Screenshot 09 – Final Payload
Step 9 – Successful Exploitation

The constructed payload executed successfully, confirming the presence of a Reflected Cross-Site Scripting vulnerability.

Reference: Screenshot 10 – Successful Execution

5. Automated Verification Using XSStrike

After completing the manual assessment, the same target was tested using XSStrike.

The tool analyzed the injection point and generated multiple context-aware payloads suitable for the application.

The generated payloads were manually verified, confirming the vulnerability identified during manual testing.

Reference: Screenshot 11 – XSStrike

6. Result

The assessment successfully demonstrated a Reflected Cross-Site Scripting vulnerability.

The testing process included:

Identification of reflected input
HTML context analysis
Initial payload testing
HTML tag enumeration
HTML attribute enumeration
Construction of a working payload
Successful JavaScript execution
Automated verification using XSStrike
7. Conclusion

This lab demonstrated the importance of understanding the HTML context before selecting XSS payloads. Manual testing provided insight into the application's input handling and filtering behavior, while XSStrike simplified payload generation and validation. Combining manual analysis with automated tools resulted in an efficient and reliable approach to identifying and verifying the Reflected XSS vulnerability.
