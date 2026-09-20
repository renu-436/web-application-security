# File Inclusion — LFI and RFI

## 1. Introduction

File Inclusion is a web application vulnerability that occurs when an application allows a user-controlled parameter to determine which file is loaded or included.

If file inclusion is not properly controlled, an attacker may attempt to access unintended files or, in some configurations, include files from a remote location.

The two main types are:

- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)

---

## 2. Objective

The objectives of this task are:

- To understand File Inclusion vulnerabilities.
- To understand Local File Inclusion (LFI).
- To understand Remote File Inclusion (RFI).
- To study how insecure file parameters can be abused.
- To understand prevention techniques.

---

## 3. Lab Environment

The testing environment consisted of:

- **Operating System:** Kali Linux
- **Web Application:** Damn Vulnerable Web Application (DVWA)
- **Web Server:** Apache
- **Database:** MariaDB
- **Browser:** Firefox
- **Security Level:** Low
- **Testing Environment:** Localhost

---

## 4. What is File Inclusion?

File Inclusion occurs when an application uses user-controlled input to select a file without properly validating the input.

For example:

```text
http://localhost/DVWA/vulnerabilities/fi/?page=file1.php
```

Here, the `page` parameter determines which file is requested.

If the application does not properly validate this parameter, an attacker may attempt to manipulate it.

---

# 5. Local File Inclusion (LFI)

## 5.1 Definition

Local File Inclusion (LFI) is a vulnerability where an attacker attempts to make an application include or access a file located on the same server.

A common technique is path traversal.

Example:

```text
../../../../etc/passwd
```

The `../` sequences attempt to move from the current directory toward parent directories.

---

## 5.2 LFI Testing

The DVWA File Inclusion page was examined using the following URL structure:

```text
http://localhost/DVWA/vulnerabilities/fi/?page=file1.php
```

A local file inclusion payload was tested:

```text
../../../../etc/passwd
```

The corresponding URL was:

```text
http://localhost/DVWA/vulnerabilities/fi/?page=../../../../etc/passwd
```

An absolute path was also tested:

```text
http://localhost/DVWA/vulnerabilities/fi/?page=/etc/passwd
```

---

## 5.3 LFI Result

The tested LFI payloads returned:

```text
File not found
```

Therefore, a successful LFI extraction was **not observed in this test environment**.

The DVWA File Inclusion directory was verified to contain files including:

```text
file1.php
file2.php
file3.php
file4.php
index.php
include.php
```

This confirmed that the File Inclusion module itself was present.

---

# 6. Remote File Inclusion (RFI)

## 6.1 Definition

Remote File Inclusion (RFI) occurs when an application allows a remotely hosted file to be included through a user-controlled parameter.

For example, an insecure application might accept a URL such as:

```text
http://example.com/malicious.php
```

as the value of a file parameter.

RFI generally depends on server configuration that permits remote file inclusion.

---

## 6.2 RFI Security Risk

If RFI is enabled and insufficiently protected, an attacker could potentially cause the server to retrieve and process an external file.

This can result in serious consequences depending on the application's configuration.

RFI should therefore be prevented through secure configuration and strict input validation.

---

# 7. LFI vs RFI

| Feature | LFI | RFI |
|---|---|---|
| Full Form | Local File Inclusion | Remote File Inclusion |
| File Location | Local server | Remote server |
| Common Technique | Path traversal | Remote URL |
| Example | `../../etc/passwd` | `http://example.com/file` |
| Main Risk | Accessing unintended local files | Including external files |
| Prevention | Input validation and allowlists | Disable remote inclusion and validate input |

---

# 8. Root Cause

The common root cause of File Inclusion vulnerabilities is insecure handling of user-controlled file parameters.

For example, an application should not blindly use:

```php
include($_GET['page']);
```

because the user controls the value of `page`.

---

# 9. Prevention

### 9.1 Use an Allowlist

Only allow known, predefined files.

Example:

```php
$allowed = [
    "file1.php",
    "file2.php",
    "file3.php"
];

if (in_array($_GET['page'], $allowed, true)) {
    include($_GET['page']);
}
```

---

### 9.2 Avoid Direct User-Controlled Includes

Applications should avoid directly passing user input to file inclusion functions such as:

```php
include()
require()
require_once()
```

without validation.

---

### 9.3 Disable Remote File Inclusion

Remote file inclusion should be disabled when it is not required.

PHP configuration should be securely configured so that remote file inclusion is not unnecessarily permitted.

---

### 9.4 Input Validation

User input should be validated against an expected set of values rather than simply filtering suspicious characters.

---

### 9.5 Use Secure File Paths

Applications should use controlled directories and predefined file mappings instead of accepting arbitrary filesystem paths from users.

---

# 10. Testing Procedure

The following procedure was followed:

1. Opened the DVWA application.
2. Set the DVWA security level to Low.
3. Opened the File Inclusion module.
4. Examined the `page` parameter.
5. Tested a normal file such as `file1.php`.
6. Tested an LFI path traversal payload.
7. Tested an absolute local file path.
8. Recorded the resulting behavior.

---

# 11. Testing Result

| Test Case | Result |
|---|---|
| File Inclusion page accessible | Passed |
| `file1.php` available | Passed |
| LFI path traversal tested | Tested |
| `/etc/passwd` extraction | Not successful |
| RFI practical execution | Not performed |
| Prevention techniques studied | Completed |

---

# 12. Evidence

The following screenshots can be added to the project repository if available:

- DVWA File Inclusion page
- File Inclusion URL showing the `page` parameter
- LFI test result showing `File not found`
- DVWA security level

---

# 13. Conclusion

File Inclusion vulnerabilities occur when applications improperly handle user-controlled file paths.

LFI involves attempting to access files located on the local server, while RFI involves attempting to include files hosted remotely.

The LFI payloads tested in the local DVWA environment returned `File not found`, so a successful LFI extraction was not claimed.

Proper input validation, allowlisting, controlled file mappings, and secure server configuration are important measures for preventing File Inclusion vulnerabilities.

---

# 14. Disclaimer

This testing was performed in a controlled local DVWA laboratory environment for educational and cybersecurity training purposes.

Security testing should only be performed on systems for which proper authorization has been obtained.
