# HTTP & HTTPS

> HTTP and HTTPS are the foundation of all web communication. As a cybersecurity professional, you will encounter these protocols in almost every web-based attack and defence scenario — from MITM attacks to Burp Suite interception to web application hacking.

---

## 📋 Table of Contents
- [What is HTTP?](#what-is-http)
- [What is HTTPS?](#what-is-https)
- [HTTP vs HTTPS](#http-vs-https)
- [HTTP Request & Response Cycle](#http-request--response-cycle)
- [HTTP Methods](#http-methods)
- [HTTP Status Codes](#http-status-codes)
- [Cookies & Sessions](#cookies--sessions)
- [Security Relevance](#security-relevance)
- [HTTP/HTTPS Attacks](#httphttps-attacks)
- [Quick Reference](#quick-reference)

---

## What is HTTP?

**HTTP (Hypertext Transfer Protocol)** is the protocol used for communication between a web browser (client) and a web server. It defines how requests are formatted and sent, and how servers respond.

**Key properties:**
- Operates at **Layer 7 (Application Layer)** of the OSI model
- **Request-response protocol** — client always initiates, server only responds when asked
- **Stateless** — each request is independent, the server does not remember previous requests
- Default port: **80**
- Transmits data in **plain text** — everything is readable if intercepted

---

## What is HTTPS?

**HTTPS (HTTP Secure)** is HTTP with an added layer of encryption provided by **TLS (Transport Layer Security)**. It ensures that data transmitted between the browser and server is encrypted and cannot be read by anyone intercepting the traffic.

**Key properties:**
- Same as HTTP but with TLS encryption
- Default port: **443**
- Provides three security guarantees:
  - **Confidentiality** — data is encrypted in transit
  - **Integrity** — data cannot be modified in transit without detection
  - **Authentication** — SSL certificate verifies the server's identity

**Note:** SSL (Secure Sockets Layer) is the predecessor to TLS and is now deprecated due to known vulnerabilities. When people say "SSL" today they almost always mean TLS. Current standard is **TLS 1.3**.

---

## HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---|---|---|
| Port | 80 | 443 |
| Encryption | None — plain text | TLS encryption |
| Data visibility | Anyone can read intercepted data | Encrypted — unreadable if intercepted |
| Authentication | No server verification | SSL certificate verifies server identity |
| Security | Vulnerable to MITM | Protected against MITM |
| Use case | Internal tools, non-sensitive | Any site handling user data |
| URL indicator | `http://` | `https://` + padlock icon |

---

## HTTP Request & Response Cycle

Every web interaction follows this pattern:

### HTTP Request (Client → Server)

```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0) Chrome/120.0
Accept: text/html,application/xhtml+xml
Accept-Language: en-US
Cookie: session=abc123xyz; theme=dark
Connection: keep-alive
```

**Breaking it down:**
| Part | Example | Meaning |
|---|---|---|
| Method | GET | Type of request |
| Path | /index.html | Which resource is requested |
| HTTP Version | HTTP/1.1 | Protocol version |
| Host | www.example.com | Which website |
| User-Agent | Mozilla/5.0... | Browser and OS info |
| Cookie | session=abc123xyz | Session data sent to server |

### HTTP Response (Server → Client)

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1234
Set-Cookie: session=abc123xyz; HttpOnly; Secure
Server: nginx/1.18.0
Date: Thu, 19 Mar 2026 10:00:00 GMT

<!DOCTYPE html>
<html>
  <body>Welcome to Example.com</body>
</html>
```

**Breaking it down:**
| Part | Example | Meaning |
|---|---|---|
| Status Line | HTTP/1.1 200 OK | Protocol version + status code |
| Content-Type | text/html | Type of content in response body |
| Set-Cookie | session=abc123xyz | Server sets a cookie on the browser |
| Body | `<html>...</html>` | The actual web page content |

---

## HTTP Methods

HTTP methods define what action the client wants to perform on the server:

| Method | Purpose | Example Use |
|---|---|---|
| **GET** | Retrieve a resource | Load a webpage, fetch an image |
| **POST** | Send data to the server | Submit a login form, upload a file |
| **PUT** | Update/replace an existing resource | Update a user profile |
| **DELETE** | Remove a resource | Delete an account |
| **PATCH** | Partially update a resource | Change just one field |
| **OPTIONS** | Ask server what methods are supported | CORS preflight requests |
| **HEAD** | Same as GET but returns headers only | Check if resource exists |

**Cybersecurity relevance:**
- **GET** — parameters in URL are visible in logs and browser history
- **POST** — data in body, not visible in URL (but still interceptable on HTTP)
- **OPTIONS** — can reveal available methods to an attacker
- **PUT / DELETE** — if improperly secured, allow modification/deletion of resources

---

## HTTP Status Codes

3-digit codes that tell the client what happened with the request:

### 2xx — Success
| Code | Meaning |
|---|---|
| 200 | OK — request succeeded |
| 201 | Created — resource created successfully |
| 204 | No Content — success but no response body |

### 3xx — Redirection
| Code | Meaning | Cyber relevance |
|---|---|---|
| 301 | Moved Permanently | Can be abused for open redirects |
| 302 | Found (temporary redirect) | Used in phishing redirects |
| 304 | Not Modified | Cached version served |

### 4xx — Client Errors
| Code | Meaning | Cyber relevance |
|---|---|---|
| 400 | Bad Request | Malformed input — try different payload |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Resource exists but access denied → **try harder** |
| 404 | Not Found | Resource doesn't exist |
| 405 | Method Not Allowed | Try a different HTTP method |
| 429 | Too Many Requests | Rate limiting active |

### 5xx — Server Errors
| Code | Meaning | Cyber relevance |
|---|---|---|
| 500 | Internal Server Error | Server crashed — often caused by SQLi or malformed input |
| 502 | Bad Gateway | Upstream server error |
| 503 | Service Unavailable | Server overloaded — seen during DoS attacks |

**Pentesting tip:** A **403** means the resource **exists** but you don't have access — worth investigating further. A **500** often means your input broke something — potential vulnerability.

---

## Cookies & Sessions

### Cookies
Small pieces of data stored in the browser and sent with every request to the server. Used to maintain state in a stateless protocol.

**Types:**
- **Session cookies** — temporary, deleted when browser closes
- **Persistent cookies** — stored until expiry date
- **Secure cookies** — only sent over HTTPS
- **HttpOnly cookies** — cannot be accessed by JavaScript (protects against XSS theft)

**Cookie security flags:**
| Flag | Purpose |
|---|---|
| **Secure** | Only transmitted over HTTPS |
| **HttpOnly** | Not accessible via JavaScript |
| **SameSite** | Controls cross-site cookie sending (prevents CSRF) |

### Sessions
Since HTTP is stateless, sessions allow the server to "remember" a user across multiple requests.

**How sessions work:**
```
1. User logs in with credentials
2. Server creates a session and generates a Session ID
3. Server sends Session ID to browser via Set-Cookie header
4. Browser stores Session ID in a cookie
5. Every subsequent request includes the Session ID cookie
6. Server looks up the Session ID in its session store
7. Server knows who the user is without them logging in again
```

**Session ID security:**
- Must be long, random, and unpredictable
- Should expire after inactivity
- Must be invalidated on logout
- Should only be transmitted over HTTPS

---

## Security Relevance

HTTP/HTTPS is the attack surface for the majority of web application vulnerabilities. Understanding the request-response cycle is essential for using tools like **Burp Suite** — which intercepts, inspects, and modifies HTTP/HTTPS traffic between your browser and web servers.

---

## HTTP/HTTPS Attacks

### 1. Man-in-the-Middle (MITM)

**How it works:**
On an unencrypted HTTP connection, an attacker positioned between the client and server can read and modify all traffic.

```
Normal:  Client ──────────────────────► Server
MITM:    Client ──► Attacker ──► Server
                    (reads/modifies everything)
```

**What attacker can see:** Usernames, passwords, form data, session IDs, all page content

**Tools:** Wireshark, Ettercap, mitmproxy, Bettercap

**Defence:** Use HTTPS — all traffic is encrypted, intercepted data is unreadable

---

### 2. Session Hijacking (Cookie Stealing)

**How it works:**
An attacker steals a valid session ID and uses it to impersonate the victim — without needing their username or password.

**Methods to steal session IDs:**
- **Packet sniffing on HTTP** — session cookie transmitted in plain text
- **XSS attack** — malicious JavaScript reads and exfiltrates cookies
- **Network MITM** — intercept session cookie in transit

```
Victim logs in → gets Session ID: abc123xyz
Attacker steals Session ID: abc123xyz
Attacker sends request with Cookie: session=abc123xyz
Server thinks attacker IS the victim → full access granted
```

**Defence:**
- **HttpOnly flag** — prevents JavaScript from reading cookies
- **Secure flag** — cookies only sent over HTTPS
- **Session expiry** — short session timeouts
- **Session regeneration** — new session ID after login

---

### 3. SSL Stripping

**How it works:**
An attacker downgrades an HTTPS connection to HTTP, removing the encryption so traffic can be read in plain text.

```
Victim ──(HTTPS)──► Attacker ──(HTTP)──► Server
```

The victim sees they're on HTTP but may not notice (especially if the attacker strips HTTPS indicators from the page).

**Defence:**
- **HSTS (HTTP Strict Transport Security)** — tells browser to always use HTTPS, never downgrade
- Browser HSTS preload lists — hardcoded list of sites that always require HTTPS

---

### 4. Clickjacking

**How it works:**
An attacker embeds the target website in an invisible iframe over a fake page. The victim thinks they're clicking on something harmless but are actually clicking on the target site.

**Defence:**
- **X-Frame-Options header** — prevents site from being embedded in iframes
- **Content Security Policy (CSP)** — controls which origins can embed the page

---

### 5. HTTP Request Smuggling

**How it works:**
Exploits differences in how front-end and back-end servers parse HTTP request boundaries. Allows an attacker to "smuggle" a malicious request inside a legitimate one, bypassing security controls.

**Impact:** Cache poisoning, authentication bypass, access to restricted endpoints

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| HTTP port | 80 |
| HTTPS port | 443 |
| HTTP encryption | None — plain text |
| HTTPS encryption | TLS (not SSL — SSL is deprecated) |
| Request initiator | Always the client (browser) |
| GET | Retrieve — parameters visible in URL |
| POST | Send data — parameters in body |
| 200 | Success |
| 403 | Forbidden — resource exists, no access |
| 404 | Not Found |
| 500 | Server error — possible vulnerability |
| Session ID | Identifies logged-in user across requests |
| HttpOnly flag | Cookie not accessible via JavaScript |
| Secure flag | Cookie only sent over HTTPS |
| MITM defence | Use HTTPS |
| Session hijacking defence | HttpOnly + Secure flags + HTTPS |
| SSL Stripping defence | HSTS header |
| Burp Suite | Tool for intercepting and modifying HTTP/HTTPS traffic |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
