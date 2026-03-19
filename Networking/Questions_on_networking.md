# ❓ Networking — Interview Q&A Revision Sheet

> This file contains interview-style questions covering all networking topics with model answers written the way a junior cybersecurity candidate should answer them — clear, technical, and concise. Use this for exam revision, interview preparation, and self-testing.

---

## 📋 Table of Contents
- [Topic 1 — What is a Network?](#topic-1--what-is-a-network)
- [Topic 2 — Public vs Private IP](#topic-2--public-vs-private-ip)
- [Topic 3 — Switch & MAC Addresses](#topic-3--switch--mac-addresses)
- [Topic 4 — Router & NAT](#topic-4--router--nat)
- [Topic 5 — DNS](#topic-5--dns)
- [Topic 6 — HTTP & HTTPS](#topic-6--http--https)
- [Topic 7 — TCP vs UDP](#topic-7--tcp-vs-udp)
- [Topic 8 — Firewalls](#topic-8--firewalls)
- [Attacks Quick Reference](#attacks-quick-reference)

---

## Topic 1 — What is a Network?

**Q1. What is a network?**
> A network is a connection of two or more devices/nodes that communicate and share resources (files, printers, internet) with each other.

---

**Q2. What are the main types of networks and what is the difference between them?**
> - **LAN (Local Area Network):** Covers a small area like a home or office.
> - **WAN (Wide Area Network):** Covers a large area — the internet itself is the biggest WAN.
> - **MAN (Metropolitan Area Network):** Covers a city.

---

**Q3. What is an IP address and why does every device need one?**
> An IP address is a unique identifier assigned to every device on a network. It is needed to identify and communicate with specific devices. There are two types:
> - **Public IP** — visible on the internet, globally unique
> - **Private IP** — used inside a LAN (e.g. 192.168.x.x), not routable on the internet

---

**Q4. Which is easier to attack — wired or wireless networks? Why?**
> Wireless is easier to attack because the signal travels through the air — an attacker can intercept traffic without any physical access to cables or devices. Wired attacks require physical access to the network infrastructure.
>
> Key phrase: **"signal interception without physical access"**

---

**Q5. An attacker is described as being "inside the LAN" — what does this mean and why is it dangerous?**
> It means the attacker has gained access to the local network. This is more dangerous than an outside attacker because they can:
> - Directly sniff traffic of other devices on the network
> - Launch Man-in-the-Middle (MITM) attacks
> - Access internal systems that are never exposed to the internet

---

## Topic 2 — Public vs Private IP

**Q1. What is the difference between a Public IP and a Private IP?**
> - **Private IPs** are used inside local networks and are not routable on the internet.
> - **Public IPs** are globally unique addresses used to communicate over the internet.
> - A router uses **NAT (Network Address Translation)** to convert private IPs to public IPs when going online.
>
> Private IP ranges:
> ```
> 10.0.0.0     – 10.255.255.255
> 172.16.0.0   – 172.31.255.255
> 192.168.0.0  – 192.168.255.255
> ```
> Any IP address outside these ranges is a Public IP.

---

**Q2. Your laptop shows IP 192.168.1.45 — is this Public or Private? How do you know?**
> Private IP. It falls in the 192.168.x.x range which is a reserved private IP range (Class C). This IP cannot be routed on the internet.

---

**Q3. Two separate homes both have a device with IP 192.168.1.1 — how is this possible?**
> Private IP addresses only need to be unique within their own network. Two completely separate networks can reuse the same private IP ranges because they never directly communicate with each other. Conflict only occurs if two devices share the same IP on the same network.

---

**Q4. As an attacker, why is knowing someone's Public IP address useful?**
> With a target's public IP you can:
> - Run an **Nmap port scan** to discover which services are running
> - **Geolocate** the target to identify their city and ISP
> - Launch **DoS/DDoS** attacks flooding the IP with traffic
> - Attempt to **exploit open services** (e.g. brute force SSH on port 22)

---

## Topic 3 — Switch & MAC Addresses

**Q1. What is a switch and what is its main job?**
> A switch is a network device that connects multiple devices within a LAN. Its main job is to map MAC addresses to physical ports and forward incoming data only to the correct destination port — unlike a hub which broadcasts to all devices.

---

**Q2. What is a MAC address and how is it different from an IP address?**
> A MAC address is a unique physical identifier burned into a network device's hardware. Format: `00:1A:2B:3C:4D:5E`
>
> | Property | MAC Address | IP Address |
> |---|---|---|
> | Layer | Layer 2 | Layer 3 |
> | Scope | Local network only | Global (internet) |
> | Changes? | Fixed in hardware* | Changes across networks |
>
> *MAC addresses can be temporarily changed in software — this is called **MAC Spoofing**.

---

**Q3. Device A wants to send data to Device C through a switch — walk through the process.**
> 1. Device A sends a frame with Device C's MAC address as the destination
> 2. Switch receives the frame and reads the destination MAC address
> 3. Switch looks up the MAC address in its CAM (MAC address) table
> 4. Finds which physical port Device C is connected to
> 5. Sends the frame **only to that specific port**
> 6. If MAC is unknown → switch broadcasts to all ports temporarily until it learns the location

---

**Q4. What is a MAC address table (CAM table) and how does a switch build it?**
> A MAC address table maps MAC addresses to physical ports on the switch.
>
> The switch builds it automatically by watching incoming traffic — every time a device sends data, the switch reads the source MAC address and records which port it arrived from. Example:
> | Port | MAC Address |
> |---|---|
> | Port 1 | AA:AA:AA:AA:AA:AA |
> | Port 2 | BB:BB:BB:BB:BB:BB |

---

**Q5. What is a MAC Flooding attack and how does it work?**
> An attacker uses a tool (Macof) to flood the switch with thousands of fake frames containing random source MAC addresses. The CAM table has limited size — once full, the switch enters fail-open mode and broadcasts all traffic to every port (behaving like a hub). The attacker can then capture all traffic on the network.
>
> - **Attack name:** CAM Table Overflow Attack
> - **Tool:** Macof
> - **Defence:** Port Security — limit MAC addresses per port

---

## Topic 4 — Router & NAT

**Q1. What is a router and how is it different from a switch?**
> - **Router:** Connects different networks together, routes traffic between them using IP addresses (Layer 3)
> - **Switch:** Connects devices within the same network using MAC addresses (Layer 2)

---

**Q2. Why does a router use IP addresses instead of MAC addresses?**
> MAC addresses only work within a local network — they are not designed to travel across the internet. IP addresses are globally routable, meaning every router in the world understands them.
>
> - MAC = your **name** (only recognized locally)
> - IP = your **postal address** (anyone anywhere can route to it)

---

**Q3. Walk through what happens when you type google.com — focus on the router's role.**
> 1. DNS resolution converts `google.com` to an IP address
> 2. Browser sends request to that IP
> 3. Router receives the packet and applies **NAT** — replaces private IP with public IP
> 4. Packet travels across the internet to Google's server
> 5. Google responds to the public IP
> 6. Router checks its NAT translation table and forwards response to the correct internal device

---

**Q4. What is a routing table and what does it contain?**
> A routing table is a database in the router containing:
>
> | Field | Purpose |
> |---|---|
> | Destination Network | Where the packet is going |
> | Subnet Mask | Size of the destination network |
> | Next Hop | Which router to forward to next |
> | Interface | Which physical port to send out from |
> | Metric | Cost — used to pick the best route |

---

**Q5. How does a private IP reach Google if private IPs cannot travel the internet?**
> The router uses **NAT (Network Address Translation)**:
> ```
> Your Device (192.168.1.5)
>         ↓
> Router replaces private IP with Public IP (e.g. 103.x.x.x)
>         ↓
> Data travels across internet to Google
>         ↓
> Google responds to 103.x.x.x
>         ↓
> Router checks NAT table → translates back to 192.168.1.5
>         ↓
> Your device receives the response
> ```

---

## Topic 5 — DNS

**Q1. What is DNS and why does it exist?**
> DNS stands for **Domain Name System**. It translates human-readable domain names (like `google.com`) into IP addresses that machines use to communicate. It exists because humans remember names but computers communicate using IP addresses.

---

**Q2. Walk through the full DNS resolution process step by step.**
> 1. Browser checks **local cache** — if found, use cached IP
> 2. Check **OS cache** and /etc/hosts file
> 3. Query sent to **DNS Resolver** (ISP or configured DNS like 1.1.1.1)
> 4. Resolver queries **Root DNS Server** — returns address of TLD server
> 5. Resolver queries **TLD Server** (.com) — returns address of Authoritative server
> 6. Resolver queries **Authoritative DNS Server** — returns actual IP
> 7. Resolver **caches** the result and returns IP to browser
> 8. Browser connects to the web server using the IP

---

**Q3. What are DNS records? Name at least 4 types and explain each.**
> DNS records are entries in DNS servers defining how a domain behaves:
>
> | Record | Purpose |
> |---|---|
> | **A** | Maps domain to IPv4 address |
> | **AAAA** | Maps domain to IPv6 address |
> | **CNAME** | Aliases one domain to another |
> | **MX** | Specifies mail servers for the domain |
> | **TXT** | Stores text info (SPF, DMARC, verification) |
> | **NS** | Identifies authoritative DNS server — used in enumeration |

---

**Q4. What is DNS caching and what is the security risk?**
> DNS caching temporarily stores query results so the full resolution process doesn't repeat. Each record has a **TTL (Time To Live)** that determines how long it's cached.
>
> **Security risk — DNS Cache Poisoning:** An attacker injects a malicious DNS record into a resolver's cache. All users of that resolver get redirected to the attacker's fake IP until the TTL expires.

---

**Q5. What is DNS Spoofing and how do you protect against it?**
> DNS Spoofing is when an attacker sends fake DNS responses redirecting a legitimate domain to a malicious IP. The victim visits a fake site and enters credentials.
>
> **Protection:**
> - Verify **HTTPS** padlock (certificate mismatch will warn you)
> - Avoid suspicious links and public WiFi
> - Use **DNSSEC** — cryptographically validates DNS records
> - Use trusted DNS: Cloudflare `1.1.1.1` or Google `8.8.8.8`

---

## Topic 6 — HTTP & HTTPS

**Q1. What is HTTP and what does it stand for?**
> HTTP stands for **Hypertext Transfer Protocol**. It is a request-response protocol for communication between a web browser (client) and a web server. The client always initiates requests — the server only responds when asked. Default port: **80**.

---

**Q2. What is the difference between HTTP and HTTPS?**
> HTTPS (HTTP Secure) adds **TLS encryption** to HTTP. All data in transit is encrypted — if intercepted, it appears as unreadable gibberish.
> - HTTP port: 80 — plain text, vulnerable
> - HTTPS port: 443 — encrypted, secure
> - **SSL is deprecated** — TLS is the modern standard

---

**Q3. Walk through the HTTP request and response cycle.**
> **Request contains:** Method (GET/POST), Path, HTTP version, Host header, User-Agent (browser info), Cookies
>
> **Response contains:** Status code, Content-Type, Set-Cookie header, Response body (HTML/JSON)
>
> **HTTP Methods:**
> | Method | Purpose |
> |---|---|
> | GET | Retrieve a resource |
> | POST | Send data to server |
> | PUT | Update existing resource |
> | DELETE | Remove a resource |
> | OPTIONS | Ask what methods are allowed |

---

**Q4. What are HTTP status codes? Give examples with cybersecurity relevance.**
> | Code | Meaning | Cyber Relevance |
> |---|---|---|
> | 200 | OK | Normal success |
> | 301 | Moved Permanently | Possible open redirect |
> | 400 | Bad Request | Malformed input |
> | 401 | Unauthorized | Authentication required |
> | 403 | Forbidden | Resource exists — no access (try harder) |
> | 404 | Not Found | Resource missing |
> | 500 | Internal Server Error | Input may have broken something |
> | 503 | Service Unavailable | Seen during DoS attacks |

---

**Q5. What are the risks of HTTP over HTTPS? Give a real attack scenario.**
> HTTP transmits everything in plain text. A **MITM (Man-in-the-Middle)** attacker on the same network intercepts all traffic. Example: user logs into an HTTP site on public WiFi — attacker captures username and password in plain text using Wireshark.

---

**Q6. You are on public WiFi visiting an HTTP site — what can an attacker see and do?**
> Because there is no encryption, attacker can see:
> - All websites visited
> - Usernames and passwords from form submissions
> - Cookies and **Session IDs**
>
> With a stolen Session ID the attacker performs **Session Hijacking** — impersonating the victim on the website without needing their password. Tools used: Wireshark, Ettercap.

---

## Topic 7 — TCP vs UDP

**Q1. What is TCP and what does it stand for?**
> TCP stands for **Transmission Control Protocol**. It is a reliable, connection-oriented protocol that guarantees accurate, ordered data delivery. Before transferring data, TCP establishes a connection using the Three-Way Handshake.

---

**Q2. What is UDP and what does it stand for?**
> UDP stands for **User Datagram Protocol**. It is a fast, connectionless "fire and forget" protocol — it sends data without establishing a connection or guaranteeing delivery. Used where speed is more important than accuracy.

---

**Q3. What is the TCP Three-Way Handshake? Walk through it step by step.**
> A three-step process to establish a TCP connection:
>
> | Step | Flag | Sent By | Meaning |
> |---|---|---|---|
> | 1 | **SYN** | Client | "I want to connect" |
> | 2 | **SYN-ACK** | Server | "I received your request, I'm ready" |
> | 3 | **ACK** | Client | "Confirmed, let's communicate" |
>
> After these three steps, the connection is established and data transfer begins.

---

**Q4. What is the main difference between TCP and UDP? Give real-world examples.**
> | Feature | TCP | UDP |
> |---|---|---|
> | Connection | Connection-oriented | Connectionless |
> | Reliability | Guaranteed | No guarantee |
> | Speed | Slower | Faster |
> | Use cases | WhatsApp, banking, web browsing | Live streaming, gaming, DNS, VoIP |
>
> Note: **DNS uses UDP** — a simple query/response where speed matters more than guaranteed delivery.

---

**Q5. Why would anyone use UDP if it doesn't guarantee delivery?**
> UDP is preferred when:
> - **Speed is critical** — gaming, live video calls cannot afford TCP's handshake delay
> - **Small data loss is acceptable** — a dropped frame in a video call is barely noticeable
> - **Low latency required** — real-time applications need immediate transmission
> - **Broadcasting** — UDP can send to multiple receivers; TCP cannot

---

**Q6. What is a SYN Flood Attack? How does it work and how is it defended against?**
> An attacker exploits the TCP Three-Way Handshake:
> 1. Sends massive volume of **SYN packets** to the server
> 2. Server responds with **SYN-ACK** and allocates resources for each
> 3. Attacker never sends the final **ACK** — leaving **half-open connections**
> 4. Server's connection table fills completely
> 5. Server cannot accept legitimate connections → **Denial of Service**
>
> **Defence:** **SYN Cookies** — server doesn't allocate resources until the final ACK is received.

---

## Topic 8 — Firewalls

**Q1. What is a firewall and what is its main purpose?**
> A firewall is a security system that monitors and controls incoming and outgoing network traffic based on predefined rules. Its purpose is to prevent unauthorised access, block malicious traffic, and protect internal networks from external threats.

---

**Q2. What are the different types of firewalls?**
> | Type | Layer | What it does |
> |---|---|---|
> | **Packet Filtering (Stateless)** | Layer 3/4 | Checks IP, port, protocol per packet |
> | **Stateful Inspection** | Layer 3/4/5 | Tracks active connections and context |
> | **Proxy Firewall** | Layer 7 | Intermediary — inspects full content |
> | **NGFW** | All layers | DPI + IPS + SSL inspection |
> | **WAF** | Layer 7 | Protects web apps from SQLi, XSS etc. |

---

**Q3. What is the difference between a stateful and stateless firewall?**
> **Stateless:** Inspects each packet individually based on IP, port, and protocol. Fast but cannot verify if a response was actually requested.
>
> **Stateful:** Tracks active connections in a state table. Only allows response packets for connections that were initiated from inside. Blocks unsolicited incoming packets. Much more secure.

---

**Q4. What are firewall rules and how does a firewall process them?**
> Firewall rules are predefined conditions telling the firewall whether to allow or block traffic.
>
> **Processing:**
> ```
> Packet arrives → Check rules top to bottom
>                → First matching rule applied
>                → Action: ALLOW / DENY / LOG
>                → No match → DEFAULT DENY (block everything)
> ```
>
> **Default Deny / Implicit Deny:** The last rule always blocks everything not explicitly allowed. This is a core security principle.

---

**Q5. What is the difference between whitelist and blacklist in firewalls? Which is safer?**
> | Approach | Logic | Security |
> |---|---|---|
> | **Whitelist** | Allow only known good — block everything else | ✅ Safer |
> | **Blacklist** | Block only known bad — allow everything else | ⚠️ Weaker |
>
> Blacklisting is reactive — new threats slip through until added. Whitelisting is proactive — unknown threats are blocked by default. **Whitelist is always safer.**

---

**Q6. How can an attacker bypass a firewall? Explain using a real attack scenario.**
> **Attack: Phishing + C2 (Command and Control)**
> 1. Firewall blocks all direct incoming connections ✓
> 2. Attacker sends phishing email to an employee
> 3. Employee clicks malicious link
> 4. Browser makes **outgoing** request to attacker's server (firewall allows outgoing traffic)
> 5. Malicious server responds — malware is delivered
> 6. Malware "calls home" to attacker's C2 server via outgoing traffic
> 7. Attacker has full access to internal network
>
> **Key lesson:** Firewalls primarily protect against incoming threats. Outbound connections initiated by internal users are usually allowed. This is why security must be **layered** — Defence in Depth:
>
> ```
> Firewall → Email Filter → WAF → Endpoint Security → SIEM/SOC → User Training
> ```

---

## Attacks Quick Reference

| Attack | Target | How it works | Defence |
|---|---|---|---|
| MAC Flooding | Switch CAM table | Flood with fake MACs → switch broadcasts all traffic | Port Security |
| MAC Spoofing | Switch/Network | Change MAC to impersonate another device | 802.1X Authentication |
| ARP Poisoning | Layer 2 | Fake ARP replies redirect traffic through attacker | Dynamic ARP Inspection |
| IP Spoofing | Layer 3 | Forge source IP to hide identity or bypass controls | Ingress filtering, IPSec |
| DNS Spoofing | DNS | Fake DNS response redirects victim to malicious IP | DNSSEC, HTTPS |
| DNS Cache Poisoning | DNS Resolver | Inject fake records into resolver cache | DNSSEC, source port randomization |
| DNS Tunneling | Firewall bypass | Encode data in DNS queries to exfiltrate data | DNS traffic analysis |
| MITM | HTTP traffic | Intercept plain text traffic between client and server | HTTPS |
| Session Hijacking | HTTP Sessions | Steal Session ID to impersonate logged-in user | HttpOnly + Secure flags |
| SSL Stripping | HTTPS | Downgrade HTTPS to HTTP to read traffic | HSTS header |
| SYN Flood | TCP handshake | Exhaust server with half-open connections | SYN Cookies |
| UDP Flood | Server resources | Flood server with UDP packets | Rate limiting |
| Phishing + C2 | Firewall | Use outgoing traffic to bypass inbound firewall rules | Email filter + user training |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
