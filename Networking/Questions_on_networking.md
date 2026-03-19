# 🛡️ Cybersecurity Study Notes — Networking Q&A

## 📋 Table of Contents
- [Topic 1 — What is a Network?](#topic-1--what-is-a-network)
- [Topic 2 — Public vs Private IP](#topic-2--public-vs-private-ip)
- [Topic 3 — Switch & MAC Addresses](#topic-3--switch--mac-addresses)
- [Topic 4 — Router & NAT](#topic-4--router--nat)
- [Topic 5 — DNS](#topic-5--dns)
- [Topic 6 — HTTP & HTTPS](#topic-6--http--https)
- [Topic 7 — TCP vs UDP](#topic-7--tcp-vs-udp)
- [Topic 8 — Firewalls](#topic-8--firewalls)

---

## Topic 1 — What is a Network?

**Q1. What actually is a network?**
> A network is a connection of two or more devices/nodes that communicate and share resources (files, printers, internet) with each other.

---

**Q2. What are the two main types of networks and the difference?**
> - **LAN (Local Area Network):** Covers a small area like a home or office.
> - **WAN (Wide Area Network):** Covers a large area — the internet itself is the biggest WAN.
> - **MAN (Metropolitan Area Network):** Covers a city.

---

**Q3. What is an IP address and why does every device need one?**
> An IP address is a unique identifier assigned to every device on a network. It is needed to identify and communicate with specific devices.
> Two types:
> - **Public IP** — visible on the internet
> - **Private IP** — used inside a LAN (e.g. 192.168.x.x)

---

**Q4. Wired vs wireless — which is easier to attack and why?**
> Wireless is easier to attack because the signal travels through the air — an attacker can intercept traffic without any physical access to cables or devices. Wired attacks require physical access to the network infrastructure, making them much harder.
>
> Key phrase: **"signal interception without physical access"**

---

**Q5. An attacker is "inside the LAN" — what does that mean and why is it dangerous?**
> It means the attacker has gained access to the local network. This is more dangerous than an outside attacker because they can:
> - Directly sniff traffic of other devices
> - Launch Man-in-the-Middle (MITM) attacks
> - Access internal systems that are never exposed to the internet

---

## Topic 2 — Public vs Private IP

**Q1. Difference between Public and Private IP?**
> - **Private IPs** are used inside local networks and are not routable on the internet (e.g. 192.168.x.x)
> - **Public IPs** are globally unique addresses used to communicate over the internet
> - A router uses **NAT** to convert private IPs to public IPs when going online

---

**Q2. Your laptop has 192.168.1.45 — Public or Private? How do you know?**
> Private IP. It falls in the 192.168.x.x reserved private range.
>
> The three private IP ranges:
> ```
> 10.0.0.0     – 10.255.255.255
> 172.16.0.0   – 172.31.255.255
> 192.168.0.0  – 192.168.255.255
> ```
> Any IP outside these ranges = Public IP.

---

**Q3. Two homes both have a device with 192.168.1.1 — how is that possible?**
> Private IP addresses only need to be unique within their own network. Two completely separate networks can reuse the same private IP ranges because they never directly communicate with each other. Conflict only occurs if two devices share the same IP on the same network.

---

**Q4. As an attacker, why is knowing someone's Public IP useful?**
> With a target's public IP you can:
> - Run an **Nmap port scan** to discover open services
> - **Geolocate** the target to identify their city and ISP
> - Launch **DoS/DDoS** attacks
> - Attempt to **exploit open services** (e.g. brute force SSH on port 22)

---

## Topic 3 — Switch & MAC Addresses

**Q1. What is a switch and what is its main job?**
> A switch is a network device that connects multiple devices within a LAN. Its main job is to map MAC addresses to physical ports and forward data only to the correct port — unlike a hub which broadcasts to all devices.

---

**Q2. What is a MAC address and how is it different from an IP address?**
> A MAC address is a unique physical identifier burned into a network device's hardware.
> Format: `00:1A:2B:3C:4D:5E`
>
> Unlike an IP address which can change across networks, a MAC address is tied to the hardware. However it can be temporarily changed in software through a technique called **MAC Spoofing**.

---

**Q3. Device A wants to send data to Device C — walk through the process.**
> 1. Device A sends data with Device C's MAC address as the destination
> 2. Switch receives it and looks up the destination MAC in its MAC address table
> 3. Finds which physical port Device C is connected to
> 4. Sends data **only to that port**
> 5. If MAC is unknown → switch temporarily **broadcasts to all ports** until it learns the correct port

---

**Q4. What is a MAC address table (CAM table)?**
> A MAC address table maps MAC addresses to physical ports on the switch.
>
> Example:
> | Port   | MAC Address       |
> |--------|-------------------|
> | Port 1 | AA:AA:AA:AA:AA:AA |
> | Port 2 | BB:BB:BB:BB:BB:BB |
> | Port 3 | CC:CC:CC:CC:CC:CC |
>
> The switch builds it automatically by watching incoming traffic — every time a device sends data, the switch records its MAC address and the port it came from.

---

**Q5. What is a MAC Flooding attack?**
> An attacker floods the switch with thousands of fake MAC addresses, filling up the CAM table. Once full, the switch can no longer make forwarding decisions and starts broadcasting all traffic to every port — turning it into a hub. The attacker can then sniff all traffic on the network.
>
> - **Attack name:** CAM Table Overflow Attack
> - **Tool used:** Macof

---

## Topic 4 — Router & NAT

**Q1. What is a router and how is it different from a switch?**
> - **Router:** Connects different networks together and routes traffic between them using IP addresses
> - **Switch:** Connects devices within the same network using MAC addresses

---

**Q2. Why does a router use IP addresses instead of MAC addresses?**
> MAC addresses only work within a local network — they are not designed to travel across the internet. IP addresses are globally routable, meaning every router in the world understands them.
>
> - MAC = your **name** (only recognized locally)
> - IP = your **postal address** (anyone anywhere can route to it)

---

**Q3. Walk through what happens when you type google.com — router's role?**
> 1. DNS resolution converts `google.com` to an IP address
> 2. Browser sends request to that IP
> 3. Router receives it and replaces your **private IP** with your **public IP** using NAT
> 4. Request travels across the internet to Google's server
> 5. Google responds to your public IP
> 6. Router translates it back to your private IP and delivers to your device

---

**Q4. What is a routing table?**
> A routing table is a database stored in a router containing:
>
> | Field | Purpose |
> |---|---|
> | Destination Network | Where the data needs to go |
> | Subnet Mask | Defines the network size |
> | Next Hop | Which router to forward to next |
> | Interface | Which port to send out from |
> | Metric | Cost to determine best route |

---

**Q5. How does your private IP reach Google if private IPs can't travel the internet?**
> The router uses **NAT (Network Address Translation):**
> ```
> Your Device (192.168.1.5)
>         ↓
> Router replaces private IP → Public IP (e.g. 103.x.x.x)
>         ↓
> Data travels across internet to Google
>         ↓
> Google responds to 103.x.x.x
>         ↓
> Router translates back → 192.168.1.5
>         ↓
> Your device gets the response
> ```

---

## Topic 5 — DNS

**Q1. What is DNS and why does it exist?**
> DNS stands for **Domain Name System**. It translates human-readable domain names (like `google.com`) into IP addresses that machines understand. It exists because humans remember names easily but computers communicate using IP addresses.

---

**Q2. Walk through the full DNS resolution process.**
> 1. Browser checks its **local cache** — if found, uses cached IP directly
> 2. If not cached, sends request to **DNS Resolver** (usually provided by ISP)
> 3. Resolver queries **Root DNS Server** — doesn't know the IP but knows which TLD server handles `.com`
> 4. **TLD Server** for `.com` directs resolver to the Authoritative DNS Server for `google.com`
> 5. **Authoritative DNS Server** returns the actual IP address
> 6. Resolver **caches** the result and returns IP to browser
> 7. Browser connects to Google's web server using that IP

---

**Q3. What are DNS records? Name at least 4 types.**
> DNS records are entries in DNS servers that define how a domain behaves.
>
> | Record | Purpose |
> |---|---|
> | **A Record** | Maps domain to IPv4 address |
> | **AAAA Record** | Maps domain to IPv6 address |
> | **CNAME Record** | Aliases one domain to another |
> | **MX Record** | Specifies mail servers for a domain |
> | **TXT Record** | Stores text information |
> | **NS Record** | Identifies authoritative DNS server — used in DNS enumeration attacks |

---

**Q4. What is DNS caching and what is the security risk?**
> DNS caching temporarily stores DNS query results so the full resolution process doesn't need to repeat. Each record has a **TTL (Time To Live)** that determines how long it stays cached.
>
> **Security risk — DNS Cache Poisoning:**
> An attacker injects a fake malicious DNS record into a resolver's cache. All users of that resolver get redirected to the attacker's fake site until the TTL expires.

---

**Q5. What is DNS Spoofing and how do you protect against it?**
> DNS Spoofing is when an attacker corrupts DNS records so a legitimate domain points to a malicious IP. The victim visits what looks like the real site but is controlled by the attacker — leading to credential theft.
>
> **Protection:**
> - Always verify **HTTPS** padlock
> - Avoid clicking suspicious links
> - Avoid public/untrusted WiFi
> - Use **DNSSEC** enabled domains
> - Use trusted DNS servers: Cloudflare `1.1.1.1` or Google `8.8.8.8`

---

## Topic 6 — HTTP & HTTPS

**Q1. What is HTTP and what does it stand for?**
> HTTP stands for **Hypertext Transfer Protocol**. It is a request-response protocol used for communication between a web browser (client) and a web server. The client always initiates the request — the server only responds when asked.

---

**Q2. Difference between HTTP and HTTPS?**
> HTTPS (HTTP Secure) adds **SSL/TLS encryption** to HTTP. This encrypts all data in transit so even if intercepted, it appears as unreadable gibberish.
> - SSL is deprecated — **TLS** is the modern standard
> - The **S** in HTTPS stands for Secure

---

**Q3. Walk through an HTTP request and response cycle.**
> **HTTP Request contains:**
> ```
> GET /index.html HTTP/1.1       ← Method + Path + Version
> Host: google.com               ← Which website
> User-Agent: Chrome/120         ← Browser info
> Cookie: session=abc123         ← Session data
> ```
>
> **HTTP Response contains:**
> ```
> HTTP/1.1 200 OK                ← Status code
> Content-Type: text/html        ← Type of data
> Set-Cookie: session=abc123     ← Server setting cookie
>
> <html>...</html>               ← Actual webpage content
> ```
>
> **HTTP Methods:**
> | Method | Purpose |
> |---|---|
> | GET | Retrieve data |
> | POST | Send data to server |
> | PUT | Update existing data |
> | DELETE | Remove data |
> | OPTIONS | Ask what methods are allowed |

---

**Q4. What are HTTP status codes? Give examples.**
> | Code | Meaning | Cyber Relevance |
> |---|---|---|
> | 200 | OK | Normal success |
> | 301 | Moved Permanently | Redirect — can be abused |
> | 400 | Bad Request | Malformed input |
> | 401 | Unauthorized | Login required |
> | 403 | Forbidden | Exists but blocked — try harder |
> | 404 | Not Found | Resource missing |
> | 500 | Internal Server Error | Often triggered by SQLi attempts |
> | 503 | Service Unavailable | Seen during DoS attacks |

---

**Q5. What are the risks of HTTP? Give a real attack scenario.**
> HTTP transmits all data in **plain text** — no encryption. A MITM (Man-in-the-Middle) attacker on the same network can intercept all traffic.
>
> **Attack scenario:** User logs into an HTTP site on public WiFi → attacker captures username and password in plain text using Wireshark.

---

**Q6. You are on public WiFi on an HTTP site — what can an attacker see and do?**
> Because there is no encryption, an attacker on the same network can see:
> - All websites you visit
> - Form data including usernames and passwords
> - Cookies
> - Session IDs
>
> Using a stolen session ID the attacker can perform **Session Hijacking** — impersonating you on a website without needing your password. Tools used: Wireshark, Ettercap.

---

## Topic 7 — TCP vs UDP

**Q1. What is TCP and what does it stand for?**
> TCP stands for **Transmission Control Protocol**. It is a reliable, connection-oriented protocol that ensures accurate and ordered data delivery. Before sending data, TCP establishes a connection using the Three-Way Handshake.

---

**Q2. What is UDP and what does it stand for?**
> UDP stands for **User Datagram Protocol**. It is a fast, connectionless protocol also known as **"fire and forget"** — it sends data without waiting for acknowledgement or guaranteeing delivery. Used where speed is prioritized over accuracy.

---

**Q3. What is the TCP Three-Way Handshake?**
> A three-step process used to establish a reliable connection between client and server:
>
> | Step | Sent By | Message | Meaning |
> |---|---|---|---|
> | **SYN** | Client | "I want to connect" | Synchronize |
> | **SYN-ACK** | Server | "I received your request, I'm ready" | Synchronize + Acknowledge |
> | **ACK** | Client | "Great, let's communicate" | Acknowledge |
>
> After these three steps the connection is established and data transfer begins.

---

**Q4. Main difference between TCP and UDP with real world examples.**
> | Feature | TCP | UDP |
> |---|---|---|
> | Connection | Connection-oriented | Connectionless |
> | Reliability | Guaranteed delivery | No guarantee |
> | Speed | Slower | Faster |
> | Order | Data arrives in order | No ordering |
> | Use cases | WhatsApp, email, banking, web browsing | Live streaming, gaming, video calls, DNS |
>
> Note: **DNS uses UDP** for regular queries because speed matters more than guaranteed delivery.

---

**Q5. Why would anyone use UDP if it doesn't guarantee delivery?**
> UDP is preferred when:
> - **Speed is critical** — gaming, live video calls, streaming
> - **Small data loss is acceptable** — a dropped frame in a video call is barely noticeable
> - **Low latency is required** — online gaming can't afford TCP's handshake overhead
> - **Broadcasting** — UDP can send to multiple receivers simultaneously, TCP cannot

---

**Q6. What is a SYN Flood Attack?**
> An attacker exploits the TCP Three-Way Handshake:
> 1. Attacker sends massive volume of **SYN packets** to the server
> 2. Server replies with **SYN-ACK** and allocates resources for each connection
> 3. Attacker never sends the final **ACK**
> 4. Server keeps waiting — these are called **half-open connections**
> 5. Server's connection table fills up completely
> 6. Server can no longer accept legitimate connections → **Denial of Service**
>
> **Protection:** SYN Cookies — server doesn't allocate resources until final ACK is received.

---

## Topic 8 — Firewalls

**Q1. What is a firewall and what is its main purpose?**
> A firewall is a system that monitors and controls incoming and outgoing network traffic based on predefined security rules. Its main purpose is to prevent unauthorized access, block malicious content, and protect internal networks from external threats.

---

**Q2. What are the different types of firewalls?**
> | Type | Layer | What it does |
> |---|---|---|
> | **Packet Filtering (Stateless)** | Network Layer | Checks IP, port, protocol on each packet |
> | **Stateful Inspection** | Network + Transport | Tracks active connections and context |
> | **Proxy Firewall** | Application Layer | Acts as intermediary — client never talks directly to server |
> | **Next-Gen Firewall (NGFW)** | All layers | Deep packet inspection + IDS/IPS built in |
> | **WAF (Web Application Firewall)** | Application Layer | Protects web apps from SQLi, XSS etc. |

---

**Q3. What is the difference between a stateful and stateless firewall?**
> **Stateless:**
> - Checks each packet individually based on IP, port, protocol
> - Fast and lightweight
> - Does not track connection context
> - Weakness: attacker can craft a packet that looks legitimate and pass through
>
> **Stateful:**
> - Tracks active connections and their state
> - Knows whether a response packet is valid based on prior requests
> - Blocks unsolicited responses automatically
> - Much more secure than stateless

---

**Q4. What are firewall rules and how do they work?**
> Firewall rules are predefined conditions that tell the firewall whether to allow or block traffic.
>
> **Processing flow:**
> ```
> Packet arrives
>       ↓
> Check rules top to bottom
>       ↓
> First matching rule is applied
>       ↓
> Action: ALLOW / BLOCK / LOG
>       ↓
> If no rule matches → DEFAULT DENY (block everything)
> ```
>
> The last rule is always **Default Deny / Implicit Deny** — if no rule explicitly allows traffic, it is blocked.

---

**Q5. Whitelist vs Blacklist — which is safer?**
> | Approach | Logic | Security Level |
> |---|---|---|
> | **Whitelist** | Allow only known good — block everything else | ✅ Much safer |
> | **Blacklist** | Block only known bad — allow everything else | ⚠️ Weaker |
>
> Blacklisting is **reactive** — you can only block threats you already know about.
> Whitelisting is **proactive** — unknown threats are blocked by default.
> In cybersecurity, whitelisting is always preferred where possible.

---

**Q6. How can an attacker bypass a firewall using a phishing email?**
> **Attack flow:**
> 1. Firewall blocks all direct incoming connections
> 2. Attacker sends a phishing email to an employee
> 3. Employee clicks a malicious link
> 4. Browser makes an **outgoing request** — firewall allows outgoing traffic
> 5. Malicious server responds through the allowed outgoing connection
> 6. Malware enters the system
>
> **Attack name:** C2 (Command and Control) — malware inside the network calls home to the attacker's server through outgoing traffic, completely bypassing the firewall.
>
> **Key lesson:** Firewalls alone are not enough. Security must be layered — this is called **Defence in Depth:**
>
> ```
> Firewall          ← blocks direct intrusion
> Email Filter      ← catches phishing emails
> Endpoint Security ← detects malware on devices
> SIEM / SOC        ← monitors suspicious behaviour
> User Training     ← stops employees clicking bad links
> ```

---

## 📊 Progress Summary

| Topic | Questions | Score |
|---|---|---|
| What is a Network | 5 | 4.5 / 5 |
| Public vs Private IP | 4 | 4.5 / 5 |
| Switch & MAC Addresses | 5 | 3.5 / 5 |
| Router & NAT | 5 | 4.5 / 5 |
| DNS | 5 | 5 / 5 |
| HTTP & HTTPS | 6 | 5 / 5 |
| TCP vs UDP | 6 | 5 / 5 |
| Firewalls | 6 | 5 / 5 |
| **Total** | **42** | **37 / 42 — 88%** |

---

## ⚡ Key Attacks Learned

| Attack | Protocol/Layer | What it does |
|---|---|---|
| MAC Flooding | Layer 2 (Switch) | Overflows CAM table, turns switch into hub |
| MAC Spoofing | Layer 2 | Changes MAC address to impersonate a device |
| DNS Spoofing | DNS | Redirects domain to malicious IP |
| DNS Cache Poisoning | DNS | Injects fake records into resolver cache |
| MITM Attack | HTTP | Intercepts unencrypted traffic |
| Session Hijacking | HTTP | Steals session ID to impersonate user |
| SYN Flood | TCP | Exhausts server resources with half-open connections |
| Phishing + C2 | Firewall bypass | Uses outgoing traffic to bypass firewall rules |

---

*Networking Foundation*
