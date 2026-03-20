# 🦈 Lab 04 — HTTP Stream Analysis

> **Objective:** Capture real HTTP traffic, follow the TCP/HTTP stream, and read the actual plain text request and response exchanged between the machine and a web server — demonstrating why HTTP is insecure.

---

## 📋 Lab Details

| Field | Details |
|---|---|
| Tool | Wireshark + curl |
| OS | Kali Linux |
| Interface | eth0 |
| Filter Used | `http` |
| Target | http://example.com |
| Server IP | 104.18.26.120 |
| Date | March 2026 |
| Difficulty | Beginner |

---

## 🎯 Objective

- Capture HTTP traffic using Wireshark
- Follow the HTTP stream to read the full request and response
- Read plain text HTTP headers and body
- Understand why HTTP is vulnerable to interception
- Compare what HTTP vs HTTPS would look like in Wireshark

---

## 🔧 Steps Performed

### Step 1 — Force HTTP Traffic Using curl
The browser automatically upgraded HTTP to HTTPS — preventing plain text capture. Used `curl` to force an HTTP connection:

```bash
curl http://example.com
```

This sent a plain HTTP GET request without any encryption — exactly what Wireshark needed to capture readable traffic.

### Step 2 — Apply HTTP Filter
Typed `http` in the Wireshark filter bar. HTTP packets appeared showing GET requests in the Info column.

### Step 3 — Follow HTTP Stream
Right clicked on a GET packet → **Follow → HTTP Stream**

A new window opened showing the full conversation in two colours:
- **Red text** — data sent FROM my machine TO the server (HTTP Request)
- **Blue text** — data sent FROM the server TO my machine (HTTP Response)

---

## 🔍 HTTP Stream Analysis

### Red Text — HTTP Request (My Machine → Server)

```http
GET / HTTP/1.1
Host: example.com
User-Agent: curl/8.13.0
Accept: */*
```

**Breaking it down:**

| Field | Value | Meaning |
|---|---|---|
| Method | GET | Retrieve the root page |
| Path | / | Root path of the website |
| HTTP Version | HTTP/1.1 | Protocol version |
| Host | example.com | Which website is being requested |
| User-Agent | curl/8.13.0 | What made the request — curl tool |
| Accept | \*/\* | Accept any content type in response |

**Security note:** The User-Agent reveals what software you are using. In a browser this would show your browser name and version — information an attacker can use for fingerprinting.

### Blue Text — HTTP Response (Server → My Machine)

```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1256
Server: ECS (nyb/1D07)

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
</head>
<body>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents.
    You may use this domain in literature without prior coordination
    or asking for permission.</p>
</body>
</html>
```

**Breaking it down:**

| Field | Value | Meaning |
|---|---|---|
| Status Line | HTTP/1.1 200 OK | Request succeeded |
| Content-Type | text/html | Response body is an HTML webpage |
| Content-Length | 1256 | Size of response body in bytes |
| Server | ECS (nyb/1D07) | Server software — useful for attackers |
| Body | HTML content | The actual webpage content |

---

## 📊 Full HTTP Conversation

```
MY MACHINE (10.0.2.15)              SERVER (104.18.26.120)
        │                                    │
        │── GET / HTTP/1.1 ────────────────►│
        │   Host: example.com               │
        │   User-Agent: curl/8.13.0         │
        │                                    │
        │◄─ HTTP/1.1 200 OK ───────────────│
        │   Content-Type: text/html         │
        │   <html>...</html>                │
        │                                    │
```

**Everything above was in plain text** — readable by anyone on the network with Wireshark running.

---

## 🧠 Security Relevance

### Why This Lab Matters — MITM Attack Demonstration

What was just demonstrated is exactly what a **Man-in-the-Middle (MITM) attacker** sees when they intercept HTTP traffic:

```
Normal HTTP:
User → [PLAIN TEXT] → Server
       Anyone on the network can read this

With MITM:
User → [PLAIN TEXT] → Attacker → Server
       Attacker reads everything, can modify it too
```

If this was a **login page** instead of example.com, the stream would show:

```http
POST /login HTTP/1.1
Host: bank.com
User-Agent: curl/8.13.0

username=user&password=mysecretpassword123
```

The attacker would see the username and password in **plain text**. This is exactly why logging into any website over HTTP is dangerous.

### HTTP vs HTTPS in Wireshark

| | HTTP | HTTPS |
|---|---|---|
| What you see | Full readable request and response | Encrypted gibberish |
| Headers visible | Yes — Host, User-Agent, Cookies | No |
| Body visible | Yes — form data, passwords, content | No |
| Example | `GET / HTTP/1.1\r\nHost: example.com` | `.?....~.3..Eg......^.?..qY` |

### Server Header — Information Disclosure
The response revealed:
```
Server: ECS (nyb/1D07)
```
This tells an attacker exactly what web server software is running. Attackers use this to look up known vulnerabilities for that specific server version — called **banner grabbing** or **information disclosure**.

**Defence:** Web servers should suppress or falsify the Server header to hide this information.

### curl vs Browser
The browser automatically redirected HTTP to HTTPS — a security feature called **HSTS (HTTP Strict Transport Security)**. curl bypassed this by directly requesting `http://` without following redirects.

**Security lesson:** HSTS is an important defence against SSL Stripping attacks. If HSTS is not configured, an attacker can strip HTTPS and force the browser to use HTTP — making all traffic readable.

---

## 💡 What Would HTTPS Look Like?

If the same request was made to an HTTPS site, the Wireshark stream would show:

```
.?R...g.s..;.....6`.J.Ff...........
..T..Z..y.D.g.5..3.......k..9......
....*7..Qa.c..5..B...r.....p.......
```

Completely unreadable encrypted data. The attacker knows a connection was made to a server but cannot read a single byte of the content. This is the power of TLS encryption.

---

## 📌 Key Takeaways

> 1. HTTP transmits everything in plain text — headers, body, cookies, passwords
> 2. Anyone on the same network running Wireshark can read all HTTP traffic
> 3. The Server header in HTTP responses reveals web server software — useful for attackers
> 4. curl can force HTTP connections when browsers upgrade to HTTPS automatically
> 5. HTTPS makes Wireshark capture useless — only encrypted gibberish is visible
> 6. HSTS prevents browsers from downgrading HTTPS to HTTP

---

## 🔍 Filters Used

```
http                                  ← Show only HTTP traffic
tcp and ip.dst == 104.18.26.120       ← Show TCP traffic to example.com
```

## 🔧 Command Used

```bash
curl http://example.com
```

---

*Part of [Wireshark Practical Labs](./README.md)*
