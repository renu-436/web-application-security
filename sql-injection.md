# SQL Injection

## 1. Introduction

SQL Injection is a web application vulnerability that occurs when user-controlled input is incorporated into an SQL query without proper protection. An attacker may manipulate the SQL query and access information that should not be directly available.

This practical exercise was performed using DVWA (Damn Vulnerable Web Application) in a controlled local Kali Linux environment.

## 2. Objective

The objectives of this practical are:

- To identify SQL Injection in a vulnerable web application.
- To demonstrate SQL Injection using DVWA.
- To retrieve database information through SQL Injection in the controlled lab.
- To understand the risks of exposing usernames and password data.
- To understand how prepared statements prevent SQL Injection.

## 3. Lab Environment

- Operating System: Kali Linux
- Application: DVWA
- Web Server: Apache
- Database: MariaDB
- Programming Language: PHP
- Browser: Firefox

## 4. DVWA Setup

DVWA was installed and configured locally on Kali Linux.

The required Apache web server and MariaDB database services were configured, and the DVWA database was connected successfully.

The SQL Injection module was then accessed through the DVWA web interface.

## 5. Security Level

The DVWA security level was set to Low.

The Low security level was used to demonstrate the vulnerability clearly in the controlled laboratory environment.

## 6. Normal Input Test

A normal User ID was first entered:

1

The application returned information associated with the specified user.

This established the normal behavior of the application before performing the SQL Injection test.

## 7. SQL Injection Demonstration

The following SQL Injection payload was tested:

1' OR '1'='1' #

The application returned multiple user records instead of a single record.

This demonstrated that the user input was being interpreted as part of the SQL query.

## 8. Observation

The normal input returned information for one user.

After the SQL Injection payload was submitted, multiple user records were displayed.

This indicates that the application was vulnerable to SQL Injection because the input could modify the logic of the SQL query.

## 9. Username and Password Extraction

A UNION-based SQL Injection was tested in the controlled DVWA environment to demonstrate exposure of database fields.

Test payload:

1' UNION SELECT user,password FROM users #

The test targeted the user and password fields in the DVWA users table.

The result demonstrated that sensitive database information, including usernames and password hashes, could be exposed when an application is vulnerable to SQL Injection.

## 10. Root Cause

The vulnerability occurs when application code directly combines user-controlled input with an SQL query.

An unsafe query construction may conceptually look like:

    $query = "SELECT * FROM users WHERE user_id = '$id'";

In this approach, the input is incorporated directly into the SQL statement.

An attacker may therefore provide specially crafted input that changes the intended SQL query.

## 11. Prevention Using Prepared Statements

Prepared statements separate SQL instructions from user-supplied data.

### Vulnerable Approach

    $query = "SELECT * FROM users WHERE user_id = '$id'";

### Secure Approach

    $stmt = $conn->prepare("SELECT * FROM users WHERE user_id = ?");
    $stmt->bind_param("i", $id);
    $stmt->execute();

The question mark acts as a parameter placeholder. The supplied value is treated as data rather than SQL syntax.

Prepared statements are therefore an important defense against SQL Injection.

## 12. Additional Security Measures

Other measures that can reduce SQL Injection risk include:

- Input validation
- Parameterized queries
- Prepared statements
- Least-privilege database accounts
- Secure database configuration
- Safe error handling
- Regular security testing
- Use of secure database access libraries and frameworks

## 13. Evidence

The practical demonstration produced the following evidence:

1. DVWA SQL Injection page
2. Normal User ID test
3. SQL Injection using 1' OR '1'='1' #
4. Multiple user records returned
5. UNION-based extraction test targeting username and password fields

Screenshots can be added to the repository as supporting evidence.

## 14. Result

The SQL Injection vulnerability was successfully demonstrated in the controlled DVWA environment.

The testing showed that improperly handled user input can alter SQL query behavior and potentially expose database information.

The exercise also demonstrated the importance of prepared statements and parameterized queries as a primary defense against SQL Injection.

## 15. Conclusion

This practical provided hands-on experience with SQL Injection using DVWA.

The exercise demonstrated how maliciously crafted input can affect an application's database query and potentially expose sensitive information.

Using prepared statements, parameterized queries, proper input validation, and least-privilege database access can significantly reduce the risk of SQL Injection.

## 16. Disclaimer

All testing documented in this repository was performed in a controlled local DVWA laboratory environment for educational and authorized cybersecurity training purposes.

The techniques should only be used on systems where explicit permission for security testing has been obtained.
