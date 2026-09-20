# Cross-Site Scripting (XSS)

## 1. Introduction

Cross-Site Scripting (XSS) is a web application security vulnerability in which an attacker injects malicious script into a web page. If the application does not properly handle the input, the script may execute in a user's browser.

XSS commonly occurs when an application accepts untrusted user input and displays it without proper validation or output encoding.

---

## 2. Objective

The objectives of this task are:

- Understand Cross-Site Scripting (XSS).
- Demonstrate Stored XSS.
- Demonstrate Reflected XSS.
- Understand the difference between Stored and Reflected XSS.
- Observe how injected JavaScript can execute in a browser.
- Understand common XSS prevention techniques.
- Learn about input validation, output encoding, and Content Security Policy (CSP).

---

## 3. Lab Environment

The testing was performed in an intentionally vulnerable local environment.

- Operating System: Kali Linux
- Web Application: DVWA (Damn Vulnerable Web Application)
- Web Server: Apache
- Database: MariaDB
- Browser: Firefox
- Testing Environment: Localhost
- DVWA Security Level: Low

All testing was performed against the local DVWA installation for educational and authorized security testing.

---

## 4. Types of XSS

The major types of Cross-Site Scripting are:

1. Stored XSS
2. Reflected XSS
3. DOM-Based XSS

In this practical task, Stored XSS and Reflected XSS were demonstrated using DVWA.

---

# 5. Stored XSS

## 5.1 Description

Stored XSS, also called Persistent XSS, occurs when malicious input is stored by a web application and later displayed to users.

Examples of locations where data may be stored include:

- Guestbooks
- Comments
- User profiles
- Messages
- Database records

If the stored content is displayed without proper output encoding, malicious JavaScript may execute in the browser.

---

## 5.2 Testing Location

DVWA → XSS (Stored)

The page contains:

- Name
- Message
- Sign Guestbook
- Clear Guestbook

---

## 5.3 Normal Input Test

### Name

```text
Test User
```

### Message

```text
Hello, this is a security testing message.
```

The message was submitted using the **Sign Guestbook** button.

### Observation

The normal message was successfully stored and displayed in the guestbook.

---

## 5.4 Stored XSS Payload

The following payload was entered in the Message field:

```html
<script>alert('Stored XSS Test')</script>
```

### Name

```text
XSS Test
```

After clicking **Sign Guestbook**, a browser alert displaying:

```text
Stored XSS Test
```

was successfully triggered.

### Observation

The JavaScript was stored by the application and executed when the stored content was displayed.

This demonstrated Stored XSS in the intentionally vulnerable DVWA application.

---

# 6. Reflected XSS

## 6.1 Description

Reflected XSS occurs when malicious input is immediately returned by the web application in its response without proper handling or encoding.

Unlike Stored XSS, the malicious input is not permanently stored by the application.

---

## 6.2 Testing Location

DVWA → XSS (Reflected)

---

## 6.3 Normal Input Test

The following input was entered:

```text
Test User
```

The input was submitted using the **Submit** button.

### Observation

The application displayed the supplied input normally.

---

## 6.4 Reflected XSS Payload

The following payload was entered:

```html
<script>alert('Reflected XSS Test')</script>
```

After clicking **Submit**, a browser alert displaying:

```text
Reflected XSS Test
```

was successfully triggered.

### Observation

The supplied JavaScript was reflected by the application and executed immediately in the browser.

This demonstrated Reflected XSS in the intentionally vulnerable DVWA application.

---

# 7. Difference Between Stored and Reflected XSS

| Feature | Stored XSS | Reflected XSS |
|---|---|---|
| Storage | Malicious input is stored | Malicious input is not permanently stored |
| Execution | Executes when stored content is displayed | Executes when the crafted input is reflected |
| Persistence | Persistent | Non-persistent |
| Example | Guestbook or comments | Search/input parameter |
| DVWA Page | XSS (Stored) | XSS (Reflected) |

---

# 8. DOM-Based XSS

DOM-Based XSS is a type of XSS in which client-side JavaScript processes attacker-controlled data and places it into a dangerous DOM context.

The vulnerability occurs primarily in client-side code rather than directly through server-side HTML generation.

DOM-Based XSS was studied conceptually in this task and was not used as the primary practical demonstration.

---

# 9. Root Cause of XSS

XSS vulnerabilities can occur when an application:

- Accepts untrusted user input.
- Does not validate input appropriately.
- Displays user-controlled data without proper output encoding.
- Inserts untrusted data directly into HTML.
- Uses unsafe JavaScript or DOM operations.
- Does not implement appropriate browser security controls.

The main security issue is the unsafe handling of untrusted data in a browser-executable context.

---

# 10. XSS Prevention

## 10.1 Input Validation

Applications should validate user input according to the expected format and context.

For example, if an application expects a numeric ID, it should accept only valid numeric input.

Input validation should be combined with appropriate output encoding rather than being treated as the only XSS defense.

---

## 10.2 Output Encoding

User-controlled data should be encoded before being displayed in HTML.

Example in PHP:

```php
echo htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
```

This converts special characters into safe HTML representations when used in the appropriate HTML output context.

---

## 10.3 Content Security Policy (CSP)

Content Security Policy is an additional browser security mechanism that can restrict which scripts and resources a web page is allowed to load or execute.

Example Apache configuration:

```apache
Header set Content-Security-Policy "default-src 'self'; script-src 'self'"
```

CSP should be configured according to the requirements of the application.

---

## 10.4 Secure Coding Practices

Developers should:

- Treat user input as untrusted.
- Use context-appropriate output encoding.
- Avoid unsafe dynamic HTML generation.
- Avoid inserting untrusted data directly into JavaScript.
- Use secure frameworks and templating systems.
- Apply an appropriate Content Security Policy.
- Keep application frameworks and dependencies updated.

---

# 11. Testing Procedure

## Stored XSS

1. Opened DVWA.
2. Set the DVWA security level to Low.
3. Opened **XSS (Stored)**.
4. Entered normal input.
5. Submitted the input.
6. Entered the Stored XSS payload.
7. Submitted the payload.
8. Observed the JavaScript alert.
9. Captured the result as evidence.

## Reflected XSS

1. Opened **XSS (Reflected)**.
2. Entered normal input.
3. Submitted the input.
4. Entered the Reflected XSS payload.
5. Submitted the payload.
6. Observed the JavaScript alert.
7. Captured the result as evidence.

---

# 12. Test Payloads Used

### Stored XSS

```html
<script>alert('Stored XSS Test')</script>
```

### Reflected XSS

```html
<script>alert('Reflected XSS Test')</script>
```

These payloads were used only against the intentionally vulnerable local DVWA environment.

---

# 13. Evidence

The following screenshots were collected during testing:

1. Stored XSS normal input.
2. Stored XSS successful payload execution.
3. Reflected XSS normal input.
4. Reflected XSS successful payload execution.

Screenshots can be added to the repository as supporting evidence.

---

# 14. Result

Stored XSS and Reflected XSS were successfully demonstrated in the local DVWA environment.

The Stored XSS test demonstrated that malicious input could be stored and subsequently executed when displayed.

The Reflected XSS test demonstrated that malicious input could be reflected by the application and executed immediately in the browser.

Common XSS prevention techniques such as input validation, output encoding, secure coding practices, and Content Security Policy were also studied.

---

# 15. Conclusion

This practical demonstrated how Cross-Site Scripting vulnerabilities can occur when web applications improperly handle untrusted user input.

Stored XSS and Reflected XSS were successfully tested using DVWA. The exercise also demonstrated the importance of proper input handling, context-appropriate output encoding, secure coding practices, and additional browser security controls such as Content Security Policy.

---

# 16. Disclaimer

All testing was performed against an intentionally vulnerable local DVWA environment for educational and authorized security testing purposes only.

Do not test XSS payloads against websites or applications without explicit authorization.
