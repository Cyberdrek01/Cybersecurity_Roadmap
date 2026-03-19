# 📦 TCP vs UDP

> TCP and UDP are the two core transport layer protocols. Every connection you make on the internet uses one of these. Understanding the difference between them — and how attackers exploit each — is fundamental to cybersecurity.

---

## 📋 Table of Contents
- [What is TCP?](#what-is-tcp)
- [What is UDP?](#what-is-udp)
- [TCP vs UDP Comparison](#tcp-vs-udp-comparison)
- [TCP Three-Way Handshake](#tcp-three-way-handshake)
- [TCP Connection Termination](#tcp-connection-termination)
- [TCP Flags](#tcp-flags)
- [Ports](#ports)
- [Security Relevance](#security-relevance)
- [Attacks on TCP & UDP](#attacks-on-tcp--udp)
- [Quick Reference](#quick-reference)

---

## What is TCP?

**TCP (Transmission Control Protocol)** is a connection-oriented, reliable transport protocol. Before any data is exchanged, TCP establishes a connection between the two parties and guarantees that all data arrives correctly, in order, and without duplication.

**Key properties:**
- Operates at **Layer 4 (Transport Layer)** of the OSI model
- **Connection-oriented** — connection must be established before data transfer
- **Reliable** — guarantees delivery, retransmits lost packets
- **Ordered** — data arrives in the correct sequence
- **Error checked** — detects corrupted data
- **Slower** than UDP due to overhead of reliability mechanisms
- Used when data accuracy is more important than speed

**Analogy:** TCP is like a phone call — you establish a connection, confirm the other person is there, speak, and end the call cleanly.

---

## What is UDP?

**UDP (User Datagram Protocol)** is a connectionless, unreliable transport protocol. It sends data without establishing a connection and does not guarantee delivery, ordering, or integrity. Also called **"fire and forget"**.

**Key properties:**
- Operates at **Layer 4 (Transport Layer)** of the OSI model
- **Connectionless** — no handshake, data is sent immediately
- **Unreliable** — no guarantee of delivery
- **No ordering** — packets may arrive out of sequence
- **Faster** than TCP — no connection setup, no acknowledgements
- Used when speed is more important than perfect accuracy

**Analogy:** UDP is like sending a letter — you post it and don't know if it arrived. But it's fast.

---

## TCP vs UDP Comparison

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Ordering | Data arrives in order | No ordering |
| Speed | Slower | Faster |
| Error checking | Yes | Minimal |
| Retransmission | Yes — lost packets resent | No |
| Header size | 20 bytes | 8 bytes |
| Use cases | Web, email, banking, file transfer | Gaming, streaming, DNS, VoIP |

### When to use each:

**Use TCP when:**
- Data must arrive completely and correctly
- Order of data matters
- Example: Downloading a file — missing bytes corrupt the file

**Use UDP when:**
- Speed matters more than perfect delivery
- Small data loss is acceptable
- Example: Video call — a dropped frame is barely noticeable, but a 1 second delay is painful
- Example: DNS — a simple query/response where speed matters and you can just retry

---

## TCP Three-Way Handshake

Before TCP can transfer data, it must establish a connection using a **three-way handshake**:

```
CLIENT                              SERVER
  │                                   │
  │──────── SYN ─────────────────────►│  "I want to connect"
  │                                   │
  │◄─────── SYN-ACK ─────────────────│  "I received your request, I'm ready"
  │                                   │
  │──────── ACK ─────────────────────►│  "Great, let's communicate"
  │                                   │
  │════════ DATA TRANSFER ════════════│
```

**Step by step:**

| Step | Flag | Sent By | Meaning |
|---|---|---|---|
| 1 | **SYN** | Client | Client requests connection, sends initial sequence number |
| 2 | **SYN-ACK** | Server | Server acknowledges request, sends its own sequence number |
| 3 | **ACK** | Client | Client acknowledges server's response — connection established |

**Sequence numbers:**
TCP uses sequence numbers to track every byte of data. This is how TCP guarantees ordering and detects missing data.

---

## TCP Connection Termination

Closing a TCP connection requires a **four-way handshake**:

```
CLIENT                              SERVER
  │──────── FIN ─────────────────────►│  "I'm done sending"
  │◄─────── ACK ─────────────────────│  "OK, received"
  │◄─────── FIN ─────────────────────│  "I'm done too"
  │──────── ACK ─────────────────────►│  "OK, connection closed"
```

---

## TCP Flags

TCP uses flags in the packet header to control the state of the connection:

| Flag | Full Name | Purpose |
|---|---|---|
| **SYN** | Synchronize | Initiate a connection |
| **ACK** | Acknowledge | Confirm receipt of data |
| **FIN** | Finish | Close a connection gracefully |
| **RST** | Reset | Immediately terminate a connection |
| **PSH** | Push | Send data immediately without buffering |
| **URG** | Urgent | Data is urgent, process immediately |

**Cybersecurity relevance of flags:**
Nmap uses different flag combinations to scan for open ports without completing a full handshake:
- **SYN scan** (`-sS`) — sends SYN, waits for SYN-ACK (port open) or RST (port closed), never sends ACK — stealthy
- **FIN scan** — sends FIN to a closed port — it responds with RST, open ports stay silent
- **NULL scan** — sends packet with no flags set

---

## Ports

Ports identify which application or service data is destined for on a device. Think of IP as the building address and port as the room number.

**Port ranges:**
| Range | Name | Description |
|---|---|---|
| 0 – 1023 | Well-known ports | Reserved for common services |
| 1024 – 49151 | Registered ports | Used by applications |
| 49152 – 65535 | Dynamic/Ephemeral ports | Temporary client-side ports |

**Critical ports to memorize:**

| Port | Protocol | Service |
|---|---|---|
| 21 | TCP | FTP (File Transfer) |
| 22 | TCP | SSH (Secure Shell) |
| 23 | TCP | Telnet (Unencrypted remote access) |
| 25 | TCP | SMTP (Email sending) |
| 53 | UDP | DNS (Domain Name System) |
| 80 | TCP | HTTP (Web) |
| 110 | TCP | POP3 (Email retrieval) |
| 143 | TCP | IMAP (Email) |
| 443 | TCP | HTTPS (Secure web) |
| 445 | TCP | SMB (Windows file sharing) |
| 3306 | TCP | MySQL (Database) |
| 3389 | TCP | RDP (Remote Desktop Protocol) |
| 8080 | TCP | HTTP Alternative / Proxy |

**Why ports matter in cybersecurity:**
- **Port scanning** (Nmap) reveals which services are running on a target
- Open ports = potential attack surface
- Misconfigured services on open ports = vulnerabilities
- Knowing default ports helps identify services even if running on non-standard ports

---

## Security Relevance

**TCP's reliability mechanisms can be exploited:**
- The handshake process can be abused (SYN Flood)
- Connection state can be hijacked (TCP Hijacking)
- Flag combinations reveal information about open ports (port scanning)

**UDP's lack of connection can be exploited:**
- Easy to spoof source IP (no connection verification)
- Can be used for amplification attacks
- Harder to filter than TCP

---

## Attacks on TCP & UDP

### 1. SYN Flood Attack (TCP DoS)

**How it works:**
An attacker exploits the TCP three-way handshake to exhaust server resources:

```
Step 1: Attacker sends massive volume of SYN packets
        (often with spoofed source IPs)

Step 2: Server responds to each with SYN-ACK
        Server allocates resources for each half-open connection
        Server adds entry to connection table

Step 3: Attacker never sends the final ACK
        Half-open connections pile up

Step 4: Server's connection table fills completely
        Server cannot accept any new connections

Step 5: Legitimate users cannot connect → Denial of Service
```

**Half-open connections:**
```
Normal connection:  SYN → SYN-ACK → ACK ✓ (completes)
SYN flood:          SYN → SYN-ACK → ??? (never completes)
                    Server waits... waits... times out
                    1000s of these = server overwhelmed
```

**Defence:**
- **SYN Cookies** — server does not allocate resources until the final ACK is received. The SYN-ACK contains a cryptographic cookie; the connection is only established if the client returns a valid ACK with that cookie.
- **Rate limiting** — limit SYN packets per IP per second
- **Firewall rules** — detect and block SYN flood patterns

---

### 2. UDP Flood Attack (DoS)

**How it works:**
Attacker sends massive amounts of UDP packets to random ports on a target. The target tries to process each packet, finds no application listening, and sends back ICMP "destination unreachable" packets — consuming resources.

**Defence:**
- Rate limiting UDP traffic
- Firewall rules blocking unexpected UDP traffic

---

### 3. TCP Session Hijacking

**How it works:**
An attacker inserts themselves into an established TCP connection by predicting sequence numbers and injecting packets.

**Steps:**
1. Attacker sniffs an established TCP connection
2. Attacker observes sequence numbers
3. Attacker sends a packet with the correct sequence number, injecting malicious commands
4. The connection continues, but the attacker has injected data

**Defence:**
- Use encrypted protocols (SSH instead of Telnet)
- TLS encryption makes sequence number prediction useless

---

### 4. DNS Amplification (UDP)

**How it works:**
Attacker spoofs victim's IP as source, sends small UDP DNS queries to open resolvers. Resolvers send large responses to victim's IP.

```
Attacker (spoofs victim IP) → DNS Resolver (40-byte query)
DNS Resolver → Victim (4000-byte response)
× 10,000 resolvers = victim flooded
```

**Amplification factor:** Up to 100x

**Defence:**
- Disable open DNS resolvers
- Rate limiting
- BCP38 — ISP filtering to prevent IP spoofing

---

### 5. Port Scanning (Reconnaissance)

**Not an attack itself** — but the first step in almost every attack. Nmap uses TCP and UDP to discover open ports and running services:

```bash
# Basic TCP SYN scan
nmap -sS 192.168.1.1

# UDP scan
nmap -sU 192.168.1.1

# Scan specific ports
nmap -p 80,443,22 192.168.1.1

# Detect service versions
nmap -sV 192.168.1.1

# Aggressive scan (OS detection, version, scripts)
nmap -A 192.168.1.1
```

**Defence:**
- Close unnecessary ports
- Use firewall to block port scanning tools
- Use IDS/IPS to detect and alert on scan patterns

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| TCP stands for | Transmission Control Protocol |
| UDP stands for | User Datagram Protocol |
| TCP type | Connection-oriented, reliable |
| UDP type | Connectionless, unreliable |
| TCP use cases | Web, email, banking, file transfer |
| UDP use cases | Gaming, DNS, streaming, VoIP |
| Three-way handshake | SYN → SYN-ACK → ACK |
| Four-way close | FIN → ACK → FIN → ACK |
| SYN flag | Initiate connection |
| ACK flag | Acknowledge data |
| RST flag | Reset/terminate connection |
| FIN flag | Gracefully close connection |
| HTTP port | 80 (TCP) |
| HTTPS port | 443 (TCP) |
| SSH port | 22 (TCP) |
| DNS port | 53 (UDP) |
| SYN Flood | Exhaust server with half-open connections |
| SYN Cookies | Defence against SYN Flood |
| Nmap SYN scan | -sS flag — stealthy port scan |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
