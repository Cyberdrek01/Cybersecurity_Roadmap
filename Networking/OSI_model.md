# 🧱 OSI Model

> The OSI (Open Systems Interconnection) model is the foundational framework for understanding how network communication works. Every attack, every defence, every tool in cybersecurity operates at one or more of these 7 layers. Understanding the OSI model means you can look at any network concept and immediately know where it fits.

---

## 📋 Table of Contents
- [What is the OSI Model?](#what-is-the-osi-model)
- [The 7 Layers](#the-7-layers)
- [Layer 7 — Application](#layer-7--application)
- [Layer 6 — Presentation](#layer-6--presentation)
- [Layer 5 — Session](#layer-5--session)
- [Layer 4 — Transport](#layer-4--transport)
- [Layer 3 — Network](#layer-3--network)
- [Layer 2 — Data Link](#layer-2--data-link)
- [Layer 1 — Physical](#layer-1--physical)
- [How Data Travels Through the OSI Model](#how-data-travels-through-the-osi-model)
- [OSI Model in Cybersecurity](#osi-model-in-cybersecurity)
- [Key Mnemonics](#key-mnemonics)

---

## What is the OSI Model?

The OSI model is a conceptual framework that divides network communication into **7 distinct layers**. Each layer has a specific job and communicates with the layers directly above and below it.

It was created by the **International Organization for Standardization (ISO)** to standardize how different systems communicate with each other regardless of their underlying hardware or software.

**Why it matters in cybersecurity:**
- Every attack targets a specific layer
- Every security tool operates at one or more layers
- Understanding layers helps you identify where a vulnerability exists and how to defend against it

---

## The 7 Layers

```
┌─────────────────────────────────────────┐
│         Layer 7 — Application           │  ← HTTP, DNS, FTP, SMTP
├─────────────────────────────────────────┤
│         Layer 6 — Presentation          │  ← Encryption, Encoding
├─────────────────────────────────────────┤
│         Layer 5 — Session               │  ← Sessions, Authentication
├─────────────────────────────────────────┤
│         Layer 4 — Transport             │  ← TCP, UDP, Ports
├─────────────────────────────────────────┤
│         Layer 3 — Network               │  ← IP Addresses, Routing
├─────────────────────────────────────────┤
│         Layer 2 — Data Link             │  ← MAC Addresses, Switches
├─────────────────────────────────────────┤
│         Layer 1 — Physical              │  ← Cables, Signals, Hardware
└─────────────────────────────────────────┘
```

---

## Layer 7 — Application

**What it does:**
The layer closest to the end user. It provides network services directly to applications. This is where the user interacts with the network.

**Not the application itself** — it is the interface between the application and the network.

**Protocols:**
| Protocol | Purpose |
|---|---|
| HTTP / HTTPS | Web browsing |
| DNS | Domain name resolution |
| FTP | File transfer |
| SMTP | Sending emails |
| SSH | Secure remote access |
| Telnet | Unsecured remote access |

**Data unit:** Message

**Cybersecurity relevance:**
- SQL Injection — attacks the application layer
- Cross-Site Scripting (XSS) — application layer attack
- DNS Spoofing — targets DNS at this layer
- Phishing — exploits application layer protocols (HTTP, SMTP)

---

## Layer 6 — Presentation

**What it does:**
Translates data between the application layer and the network. It handles:
- **Encoding/Decoding** — converts data into a format the application can understand
- **Encryption/Decryption** — SSL/TLS operates here
- **Compression** — reduces data size for faster transmission

**Protocols:**
- SSL / TLS (encryption)
- JPEG, PNG, GIF (image formats)
- ASCII, UTF-8 (text encoding)

**Data unit:** Data

**Cybersecurity relevance:**
- SSL stripping attacks — downgrade HTTPS to HTTP at this layer
- Weak encryption vulnerabilities
- Data encoding used to hide malicious payloads (e.g. Base64 encoded malware)

---

## Layer 5 — Session

**What it does:**
Manages sessions — the establishment, maintenance, and termination of communication sessions between two devices.

Think of a session as a conversation — Layer 5 starts the conversation, keeps it alive, and ends it cleanly.

**Responsibilities:**
- **Session establishment** — creates a session between two devices
- **Session maintenance** — keeps the session alive during data transfer
- **Session termination** — closes the session when communication is complete
- **Authentication** — verifies identity at the start of a session
- **Synchronization** — adds checkpoints so data can resume after interruption

**Protocols:**
- NetBIOS
- RPC (Remote Procedure Call)
- PPTP

**Data unit:** Data

**Cybersecurity relevance:**
- Session Hijacking — attacker steals an active session
- Session Fixation — attacker forces a known session ID onto a victim
- Replay attacks — attacker captures and re-sends session data

---

## Layer 4 — Transport

**What it does:**
Responsible for end-to-end communication between devices. It breaks data into segments, sends them, and reassembles them at the destination.

**Key responsibilities:**
- **Segmentation** — breaks large data into smaller segments
- **Flow control** — manages how much data is sent at once
- **Error control** — detects and retransmits lost segments
- **Port numbering** — identifies which application the data belongs to

**Protocols:**
| Protocol | Type | Use case |
|---|---|---|
| TCP | Connection-oriented | Reliable delivery — web, email, banking |
| UDP | Connectionless | Fast delivery — gaming, streaming, DNS |

**Common ports:**
| Port | Protocol | Service |
|---|---|---|
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 22 | TCP | SSH |
| 21 | TCP | FTP |
| 25 | TCP | SMTP |
| 53 | UDP | DNS |
| 3306 | TCP | MySQL |

**Data unit:** Segment (TCP) / Datagram (UDP)

**Cybersecurity relevance:**
- SYN Flood attack — exploits TCP handshake at this layer
- Port scanning (Nmap) — probes open ports at this layer
- UDP Flood — DoS attack using UDP packets

---

## Layer 3 — Network

**What it does:**
Responsible for routing data packets between different networks. This is where IP addresses live.

**Key responsibilities:**
- **Logical addressing** — assigns IP addresses to devices
- **Routing** — determines the best path for data to travel
- **Packet forwarding** — moves packets from source to destination across networks

**Protocols:**
| Protocol | Purpose |
|---|---|
| IP (IPv4/IPv6) | Logical addressing and routing |
| ICMP | Error reporting and diagnostics (ping) |
| ARP | Resolves IP addresses to MAC addresses |
| OSPF / BGP | Routing protocols |

**Devices:** Routers, Layer 3 switches

**Data unit:** Packet

**Cybersecurity relevance:**
- IP Spoofing — attacker forges source IP address
- ICMP Flood (Ping of Death) — DoS attack using ICMP packets
- Man-in-the-Middle — attacker intercepts packets at network layer
- ARP Poisoning — attacker sends fake ARP replies to link their MAC to a legitimate IP

---

## Layer 2 — Data Link

**What it does:**
Responsible for node-to-node communication within the same network. This is where MAC addresses live.

**Key responsibilities:**
- **MAC addressing** — identifies devices on the local network
- **Framing** — wraps packets in frames for transmission
- **Error detection** — detects errors in transmitted frames
- **Flow control** — manages data flow between adjacent nodes

**Sub-layers:**
- **LLC (Logical Link Control)** — communicates with the network layer above
- **MAC (Media Access Control)** — communicates with the physical layer below

**Protocols:**
- Ethernet
- Wi-Fi (802.11)
- PPP (Point-to-Point Protocol)

**Devices:** Switches, Network Interface Cards (NICs)

**Data unit:** Frame

**Cybersecurity relevance:**
- MAC Flooding — overflows switch CAM table
- MAC Spoofing — changes MAC address to impersonate a device
- ARP Poisoning — operates at this layer
- VLAN Hopping — attacker accesses traffic from other VLANs

---

## Layer 1 — Physical

**What it does:**
The lowest layer — deals with the actual physical transmission of raw bits (0s and 1s) over a physical medium.

**Key responsibilities:**
- Transmits raw binary data over cables, fiber, or wireless signals
- Defines hardware specifications — cables, connectors, voltages, frequencies
- Converts digital data to electrical, optical, or radio signals

**Transmission media:**
| Medium | Type | Example |
|---|---|---|
| Copper cables | Wired | Ethernet (Cat5, Cat6) |
| Fiber optic | Wired | Long distance, high speed |
| Radio waves | Wireless | Wi-Fi, Bluetooth |

**Devices:** Hubs, Repeaters, Cables, Network Interface Cards

**Data unit:** Bit

**Cybersecurity relevance:**
- Physical wiretapping — intercepting signals on physical cables
- Rogue access points — attacker sets up a fake Wi-Fi access point
- Hardware keyloggers — physical device captures keystrokes
- Jamming attacks — disrupts wireless signals

---

## How Data Travels Through the OSI Model

When you send a message, data travels **down** the OSI model on the sender's side and **up** on the receiver's side:

```
SENDER                                    RECEIVER
─────────────────────────────────────────────────────
Layer 7 — Application   →   Data         Layer 7 — Application
Layer 6 — Presentation  →   Data         Layer 6 — Presentation
Layer 5 — Session       →   Data         Layer 5 — Session
Layer 4 — Transport     →   Segment      Layer 4 — Transport
Layer 3 — Network       →   Packet       Layer 3 — Network
Layer 2 — Data Link     →   Frame        Layer 2 — Data Link
Layer 1 — Physical      →   Bits ──────► Layer 1 — Physical
```

**Encapsulation** — as data moves down the layers on the sender's side, each layer adds its own header (and sometimes footer) with control information.

**Decapsulation** — as data moves up the layers on the receiver's side, each layer removes its header and passes the data up.

---

## OSI Model in Cybersecurity

| Layer | Common Attacks | Security Tools/Defences |
|---|---|---|
| 7 — Application | SQLi, XSS, DNS Spoofing, Phishing | WAF, IDS, Antivirus |
| 6 — Presentation | SSL Stripping, Weak Encryption | TLS 1.3, Strong Encryption |
| 5 — Session | Session Hijacking, Session Fixation | Secure session tokens, HTTPS |
| 4 — Transport | SYN Flood, Port Scanning | Firewalls, Rate limiting, SYN Cookies |
| 3 — Network | IP Spoofing, ICMP Flood, MITM | Firewalls, IPSec, ACLs |
| 2 — Data Link | MAC Flooding, ARP Poisoning, VLAN Hopping | Port Security, Dynamic ARP Inspection |
| 1 — Physical | Wiretapping, Rogue APs, Jamming | Physical security, Shielded cables |

---

## Key Mnemonics

**Top to Bottom (Layer 7 → Layer 1):**
> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
> Application → Presentation → Session → Transport → Network → Data Link → Physical

**Bottom to Top (Layer 1 → Layer 7):**
> **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way
> Physical → Data Link → Network → Transport → Session → Presentation → Application

---

## Quick Reference Summary

| Layer | Number | Key Protocol | Device | Data Unit | One Line Job |
|---|---|---|---|---|---|
| Application | 7 | HTTP, DNS, FTP | — | Message | Interface for user applications |
| Presentation | 6 | SSL/TLS | — | Data | Encrypt, encode, compress |
| Session | 5 | NetBIOS, RPC | — | Data | Start, manage, end sessions |
| Transport | 4 | TCP, UDP | — | Segment | End-to-end delivery with ports |
| Network | 3 | IP, ICMP, ARP | Router | Packet | Route packets between networks |
| Data Link | 2 | Ethernet, Wi-Fi | Switch | Frame | Node-to-node delivery via MAC |
| Physical | 1 | — | Hub, Cable | Bit | Transmit raw bits over medium |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
