# Task 3 — Web Application Security

## Objective

The objective of this task is to understand and demonstrate common web application security vulnerabilities in a controlled laboratory environment using Damn Vulnerable Web Application (DVWA).

The task covers vulnerability identification, basic source-code analysis, security-level comparison, and practical testing.

---

## Lab Environment

- Kali Linux
- Apache HTTP Server
- PHP
- MariaDB
- Damn Vulnerable Web Application (DVWA)
- Web Browser
- Burp Suite

All vulnerability testing is performed against the local DVWA instance.

---

## Topics Covered

### 1. SQL Injection

SQL Injection testing and source-code analysis, including the use of prepared statements.

Detailed notes:

`sql-injection/sql-injection.md`

### 2. Cross-Site Scripting (XSS)

Testing of Stored XSS and Reflected XSS vulnerabilities.

Detailed notes:

`xss/xss.md`

### 3. Cross-Site Request Forgery (CSRF)

Testing of password-change requests and comparison of CSRF protection across DVWA security levels.

Detailed notes:

`csrf/csrf.md`

### 4. File Inclusion

Testing and analysis of Local File Inclusion (LFI) and Remote File Inclusion (RFI).

Detailed notes:

`file-inclusion/file-inclusion.md`

### 5. Burp Suite

Advanced web application testing using Burp Suite, including HTTP request interception and analysis.

Detailed notes:

`burp-suite/burp-suite.md`

### 6. Web Security Headers

Analysis of HTTP security headers and their role in improving web application security.

Detailed notes:

`security-headers/security-headers.md`

---

## Task Progress

| Section | Status |
|---|---|
| SQL Injection | Completed |
| XSS | Completed |
| CSRF | Completed |
| File Inclusion | Completed |
| Burp Suite | Completed |
| Web Security Headers | Completed |

---

## Evidence

All screenshots collected during the practical exercises are stored in:

```
screenshots/
```

Supporting reports and documents are stored in:

```
reports/
```

---

## Repository Structure

```
Task-3-Web-Application-Security/
├── README.md
├── sql-injection/
│   └── sql-injection.md
├── xss/
│   └── xss.md
├── csrf/
│   └── csrf.md
├── file-inclusion/
│   └── file-inclusion.md
├── burp-suite/
│   └── burp-suite.md
├── security-headers/
│   └── security-headers.md
├── screenshots/
└── reports/
```

---

## Conclusion

This task provides practical exposure to common web application security vulnerabilities and defensive concepts.

The completed sections currently cover SQL Injection, XSS, CSRF, and File Inclusion. Burp Suite and Web Security Headers will be documented as they are completed.
