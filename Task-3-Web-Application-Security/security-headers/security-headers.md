# Web Security Headers Assessment

## 1. Objective

Assess HTTP security headers on a public test website using SecurityHeaders.com, configure security headers for the local Apache server hosting DVWA, and verify the resulting HTTP responses.

## 2. Tools and Environment

- **Assessment tool:** SecurityHeaders.com
- **Public test target:** `https://example.com`
- **Local test target:** `http://127.0.0.1/dvwa/`
- **Web server:** Apache HTTP Server 2.4.68
- **Operating system:** Kali Linux

## 3. Public Website Assessment

The SecurityHeaders.com scan of `https://example.com` reported an **F grade** in the saved report.

The report identified these missing headers:

- `Strict-Transport-Security`
- `Content-Security-Policy`
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Referrer-Policy`
- `Permissions-Policy`

This result applies to the scanned public test domain at the time of assessment. It does not represent the security posture of the local DVWA server.

**Evidence:** `../reports/44-security-headers-scan.pdf`

## 4. Apache Configuration

Enabled Apache's headers module:

```bash
sudo a2enmod headers
```

Edited the Apache virtual host configuration:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Added the following directives inside the `<VirtualHost *:80>` block:

```apache
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "SAMEORIGIN"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Permissions-Policy "camera=(), microphone=(), geolocation=()"
Header always set Content-Security-Policy "default-src 'self'; frame-ancestors 'self'"
```

Validated the configuration:

```bash
sudo apache2ctl configtest
```

The command returned `Syntax OK`. Apache was restarted to apply the configuration.

## 5. Verification

Checked the response headers on the DVWA login page:

```bash
curl -I http://127.0.0.1/dvwa/login.php
```

The response returned `HTTP/1.1 200 OK` and included the five configured security headers.

Checked the DVWA root path:

```bash
curl -I http://127.0.0.1/dvwa/
```

The response returned `HTTP/1.1 302 Found` and redirected to `login.php`. The five configured security headers were also present in this response. The redirect is expected for an unauthenticated request.

### Verification Results

| Header | Configured value | Result |
|---|---|---|
| `X-Content-Type-Options` | `nosniff` | Present |
| `X-Frame-Options` | `SAMEORIGIN` | Present |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Present |
| `Permissions-Policy` | Disables camera, microphone and geolocation | Present |
| `Content-Security-Policy` | `default-src 'self'; frame-ancestors 'self'` | Present |
| `Strict-Transport-Security` | Not configured | Not tested on local HTTP |

HSTS was not enabled because the local DVWA site was served over HTTP. HSTS should be configured for a properly deployed HTTPS site.

## 6. Security Considerations

- **Content Security Policy:** Restricts resource loading to the same origin and prevents framing by other origins. A restrictive policy may affect applications that rely on inline scripts or external resources; test application functionality before deployment.
- **X-Frame-Options:** `SAMEORIGIN` helps prevent other origins from embedding the application in a frame.
- **X-Content-Type-Options:** `nosniff` prevents browsers from MIME-sniffing content types.
- **Referrer-Policy:** Controls the amount of referrer information sent during navigation.
- **Permissions-Policy:** Disables selected browser features that this lab does not require.
- **HSTS:** Should be enabled only after HTTPS is correctly configured and validated.

Security headers are one layer of defence. They do not replace secure authentication, input validation, output encoding, prepared SQL statements, or other application-level protections.

## 7. Conclusion

The public test-domain scan identified six missing security headers. Five headers were then configured on the local Apache server hosting DVWA and verified using `curl -I`.

The local DVWA login endpoint returned HTTP 200, while the root path returned HTTP 302 to the login page. Both responses contained the five configured headers.

The public scan and local verification are separate results and should not be treated as a before-and-after scan of the same server.
