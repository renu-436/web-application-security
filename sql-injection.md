# SQL Injection

## 1. Introduction

SQL Injection is a web application vulnerability that occurs when untrusted user input is incorporated into an SQL query without proper protection.

An attacker may manipulate the SQL query through specially crafted input.

## 2. Objective

To identify and demonstrate SQL Injection in a controlled DVWA environment and understand how prepared statements can prevent the vulnerability.

## 3. Lab Environment

- Operating System: Kali Linux
- Application: DVWA
- Web Server: Apache
- Database: MariaDB
- Browser: Firefox

## 4. Vulnerable Application

Damn Vulnerable Web Application (DVWA) was installed locally on Kali Linux.

The SQL Injection module was configured at the Low security level for the demonstration.

## 5. Normal Input

The following input was first tested:

```text
1
This returned information associated with the specified user ID.

 
