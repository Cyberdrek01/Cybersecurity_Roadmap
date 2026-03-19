# 🔥 Firewalls

> A firewall is the first line of defence in any network. Understanding how firewalls work — and more importantly, how attackers bypass them — is one of the most important concepts in cybersecurity.

---

## 📋 Table of Contents
- [What is a Firewall?](#what-is-a-firewall)
- [How Firewalls Work](#how-firewalls-work)
- [Firewall Rules](#firewall-rules)
- [Types of Firewalls](#types-of-firewalls)
- [Stateful vs Stateless](#stateful-vs-stateless)
- [Whitelist vs Blacklist](#whitelist-vs-blacklist)
- [Security Relevance](#security-relevance)
- [Bypassing Firewalls](#bypassing-firewalls)
- [Defence in Depth](#defence-in-depth)
- [Quick Reference](#quick-reference)

---

## What is a Firewall?

A firewall is a security system that **monitors and controls incoming and outgoing network traffic** based on a set of predefined security rules. It acts as a barrier between a trusted internal network and untrusted external networks (like the internet).

**Firewalls can be:**
- **Hardware** — a physical device placed between network and internet
- **Software** — a program running on a device (Windows Defender Firewall, iptables)
- **Cloud-based** — firewall as a service for cloud infrastructure

**OSI layers firewalls operate at:**
- Basic firewalls — Layer 3 (Network) and Layer 4 (Transport)
- Advanced firewalls — all the way up to Layer 7 (Application)

---

## How Firewalls Work

A firewall inspects every packet that passes through it and makes a decision:

```
Incoming/Outgoing Packet
         ↓
  Check firewall rules (top to bottom)
         ↓
  Does it match Rule 1? → No → Check Rule 2
         ↓
  Does it match Rule 2? → Yes
         ↓
  Apply action: ALLOW / DENY / LOG
         ↓
  If no rule matches → DEFAULT DENY (block everything)
```

The key principle: **first matching rule wins**.

---

## Firewall Rules

Firewall rules are conditions that define what traffic is allowed or blocked. Each rule specifies:

| Field | Description | Example |
|---|---|---|
| **Direction** | Inbound or Outbound | Inbound |
| **Protocol** | TCP, UDP, ICMP | TCP |
| **Source IP** | Where the traffic comes from | Any / 192.168.1.0/24 |
| **Destination IP** | Where the traffic is going | 10.0.0.1 |
| **Port** | Which port the traffic uses | 443 |
| **Action** | What to do with the traffic | Allow / Deny / Log |

**Example ruleset:**

```
Rule 1:  Allow  TCP  Any         → This server   Port 80   (HTTP)
Rule 2:  Allow  TCP  Any         → This server   Port 443  (HTTPS)
Rule 3:  Allow  TCP  10.0.0.0/8  → This server   Port 22   (SSH from trusted network)
Rule 4:  Deny   TCP  Any         → This server   Port 22   (Block SSH from everywhere else)
Rule 5:  Allow  UDP  Any         → 8.8.8.8        Port 53   (DNS)
...
Last:    Deny   ALL  Any         → Any            Any       (DEFAULT DENY — block everything else)
```

**Default Deny / Implicit Deny:**
The last rule always blocks everything not explicitly allowed. This is a foundational security principle — if you haven't explicitly said it's allowed, it's blocked.

---

## Types of Firewalls

### 1. Packet Filtering Firewall (Stateless)
- Operates at **Layer 3 and Layer 4**
- Inspects each packet **individually** based on:
  - Source/Destination IP
  - Source/Destination Port
  - Protocol (TCP/UDP/ICMP)
- **Does not track** connection state
- Fast and lightweight
- **Weakness:** Cannot tell if a packet is part of a legitimate established connection or an attack

---

### 2. Stateful Inspection Firewall
- Operates at **Layer 3, 4, and partially 5**
- Tracks the **state of active connections** in a state table
- Allows only packets that are part of an established, legitimate connection
- **Blocks** unsolicited incoming packets even if they have the right IP/port
- Much more secure than stateless

**State table example:**
```
Source IP      Source Port   Dest IP       Dest Port   State
192.168.1.5    54321         142.250.1.1   443         ESTABLISHED
192.168.1.8    60123         8.8.8.8       53          ESTABLISHED
```

---

### 3. Proxy Firewall (Application Proxy)
- Operates at **Layer 7 — Application Layer**
- Acts as an **intermediary** between client and server
- Client never communicates directly with the server
- Proxy inspects the full content of each request and response
- Can filter based on content, URLs, file types

```
Client → [Proxy Firewall] → Server
         (inspects content)
```

**Benefit:** Hides internal network structure — external servers only see the proxy's IP.

---

### 4. Next-Generation Firewall (NGFW)
- Combines all above capabilities
- Adds:
  - **Deep Packet Inspection (DPI)** — reads inside packet payload
  - **Intrusion Prevention System (IPS)** built in
  - **Application awareness** — identifies applications by behaviour, not just port
  - **SSL/TLS inspection** — decrypts and inspects encrypted traffic
  - **User identity tracking**

---

### 5. WAF — Web Application Firewall
- Specifically designed to protect **web applications**
- Operates at **Layer 7**
- Protects against:
  - SQL Injection
  - Cross-Site Scripting (XSS)
  - CSRF (Cross-Site Request Forgery)
  - IDOR (Insecure Direct Object Reference)
- Sits in front of web servers

```
Internet → [WAF] → Web Server
```

**Examples:** Cloudflare WAF, AWS WAF, ModSecurity

---

## Stateful vs Stateless

| Feature | Stateless | Stateful |
|---|---|---|
| Tracks connections | No | Yes |
| Inspects context | No | Yes |
| Speed | Faster | Slightly slower |
| Security | Lower | Higher |
| Memory usage | Low | Higher |
| Weakness | Can't validate if response was requested | More complex to configure |

**Stateless weakness example:**
An attacker crafts a TCP packet with the correct source IP and port to pass a stateless firewall rule. The firewall allows it because it only checks headers, not whether a connection was actually established.

**Stateful strength:**
The same packet gets blocked because the stateful firewall checks its state table — no corresponding connection request exists for this packet.

---

## Whitelist vs Blacklist

### Blacklist (Block known bad)
- Explicitly blocks known malicious IPs, domains, signatures
- Everything not on the blacklist is **allowed by default**
- **Weakness:** New threats are unknown — they pass through until added to the list
- Reactive — always one step behind attackers

### Whitelist (Allow known good)
- Only explicitly approved traffic is allowed
- Everything not on the whitelist is **blocked by default**
- Much stronger security posture
- **Challenge:** Maintenance — every legitimate service must be explicitly approved

```
Blacklist logic:  Allow everything EXCEPT what I know is bad
Whitelist logic:  Block everything EXCEPT what I know is good
```

**In practice:**
- Internal corporate networks use whitelisting for maximum security
- Consumer devices use blacklisting for convenience
- **Default Deny** (last firewall rule) is the whitelist principle applied to firewalls

---

## Security Relevance

**What a firewall protects against:**
- Direct intrusion attempts from the internet
- Unauthorised port scanning
- Known malicious IPs and domains
- Unencrypted sensitive service exposure

**What a firewall does NOT protect against:**
- Attacks that come through allowed ports (e.g. web attacks on port 80/443)
- Insider threats — internal users
- Encrypted malicious traffic (unless NGFW with SSL inspection)
- Social engineering — attackers tricking users to initiate connections
- Outbound attacks initiated from inside the network

---

## Bypassing Firewalls

### 1. Phishing + C2 (Command and Control)

**The most common bypass technique:**

```
Step 1: Firewall blocks all direct incoming connections ✓
Step 2: Attacker sends phishing email to employee
Step 3: Employee clicks malicious link
Step 4: Browser makes OUTGOING request to attacker's server
        (Firewall allows outgoing traffic — this is normal web browsing)
Step 5: Attacker's server responds through the allowed connection
Step 6: Malware is delivered and installed
Step 7: Malware "calls home" to attacker's C2 server via outgoing traffic
Step 8: Attacker now has full access to internal network
```

**Why it works:** Firewalls primarily block incoming traffic. Outgoing traffic initiated by internal users is usually allowed.

---

### 2. Port Tunneling

Attacker wraps malicious traffic inside an allowed protocol:
- **HTTP tunneling** — hide attack traffic inside normal HTTP requests (port 80)
- **DNS tunneling** — encode data inside DNS queries (port 53 almost never blocked)
- **HTTPS tunneling** — encrypt malicious traffic so deep inspection can't see it

---

### 3. Firewall Evasion via Fragmentation

Breaking malicious packets into tiny fragments that individually look harmless. Reassembly happens at the destination, after passing through the firewall.

**Defence:** NGFWs reassemble fragments before inspection.

---

### 4. Using Allowed Ports for Malicious Services

Running a malicious server on port 80 or 443 — most firewalls allow these by default for web traffic.

---

## Defence in Depth

Since no single security control is perfect, organisations layer multiple controls — if one fails, others catch the threat:

```
┌─────────────────────────────────────────────┐
│              DEFENCE IN DEPTH               │
├─────────────────────────────────────────────┤
│ Perimeter Firewall    ← Blocks direct attacks│
│ Email Filter          ← Catches phishing     │
│ WAF                   ← Protects web apps    │
│ Endpoint Security     ← Detects malware      │
│ IDS / IPS             ← Detects intrusions   │
│ SIEM / SOC            ← Monitors everything  │
│ User Training         ← Stops human errors   │
└─────────────────────────────────────────────┘
```

Each layer assumes the previous one can fail. This is the foundational security architecture principle in enterprise environments.

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Firewall purpose | Monitor and control traffic based on rules |
| Rule processing | Top to bottom — first match wins |
| Default Deny | Last rule blocks everything not explicitly allowed |
| Stateless | Inspects individual packets, no connection tracking |
| Stateful | Tracks active connections, more secure |
| Packet Filtering | Layer 3/4 — IP, port, protocol |
| Proxy Firewall | Layer 7 — content inspection, hides internal network |
| NGFW | All layers — DPI, IPS, SSL inspection |
| WAF | Protects web apps from SQLi, XSS, CSRF |
| Whitelist | Allow only known good — block everything else |
| Blacklist | Block only known bad — allow everything else |
| C2 bypass | Malware uses outgoing traffic to evade inbound rules |
| Defence in Depth | Multiple layered security controls |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
