# Burp Suite Advanced Testing

## Objective
Use Burp Suite to intercept and modify HTTP requests, inspect login traffic, and perform basic parameter fuzzing with Intruder in a local DVWA lab.

## Lab Environment
- **Target:** Damn Vulnerable Web Application (DVWA)
- **URL:** `http://127.0.0.1/dvwa/login.php`
- **Tool:** Burp Suite Community Edition
- **Security Level:** Low
- **Scope:** Local lab environment only

## 1. Login Request Interception
Opened DVWA using Burp Suite's built-in browser and inspected the login request through **Proxy → Intercept** and **Proxy → HTTP history**.

- Identified the `POST /dvwa/login.php` request.
- Inspected the request headers, cookies, and login parameters.
- Modified the username parameter from `admin` to `test` to observe the application's response.
- Observed that the modified login attempt failed.

**Evidence:** 
- `../screenshots/41-burp-login-intercept.png`
- `../screenshots/42-burp-login-intercept-failed.png`

## 2. Intruder Parameter Fuzzing
Sent the login request to **Intruder** and configured a payload position around the username value.

Test payloads:
- `admin`
- `test`
- `invalid`

Started the attack and compared the HTTP status codes and response lengths returned for each payload.

**Observation:** The tested responses returned HTTP `302` with response lengths of approximately `481–482` bytes. These results alone do not establish whether a username is valid; redirects and response lengths need to be interpreted in context.

**Evidence:**
- `../screenshots/43-burp-intruder-results.png`

## Findings
- Burp Suite can intercept and inspect HTTP requests sent to the local DVWA application.
- Request parameters can be modified to observe how the application handles changed input.
- Intruder can automate testing a small set of controlled payloads and compare responses.
- HTTP status codes and response lengths are useful indicators, but they are not sufficient by themselves to confirm authentication or account validity.

## Security Recommendations
- Use server-side authentication and validate credentials securely.
- Use HTTPS to protect credentials and session cookies in transit.
- Apply rate limiting and monitoring to login endpoints.
- Use secure session-cookie attributes, including `HttpOnly`, `Secure` when using HTTPS, and an appropriate `SameSite` policy.
- Avoid exposing detailed authentication errors that reveal unnecessary account information.

## Conclusion
Basic request interception, parameter modification, and Intruder testing were performed against the local DVWA lab. The observed responses were recorded for further analysis.

**Note:** Testing was restricted to the intentionally vulnerable local lab.
