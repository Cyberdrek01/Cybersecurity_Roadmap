# 📡 Networking

> This folder contains my complete networking notes and revision material built as part of my cybersecurity learning journey. The content covers both theory and security relevance of each topic — not just definitions, but how each concept is exploited by attackers and defended against in real environments.

---

## 📂 Files in This Folder

| File | What it covers |
|---|---|
| [`OSI_model.md`](./OSI_model.md) | All 7 OSI layers, protocols per layer, security relevance |
| [`Switches.md`](./Switches.md) | How switches work, MAC address tables, MAC Flooding, MAC Spoofing |
| [`Routers.md`](./Routers.md) | Routing, NAT, routing tables, how packets travel across the internet |
| [`DNS.md`](./DNS.md) | DNS resolution chain, record types, DNS Spoofing, Cache Poisoning |
| [`Firewall.md`](./Firewall.md) | Firewall types, stateful vs stateless, whitelist vs blacklist, C2 bypass |
| [`HTTP_or_HTTPS.md`](./HTTP_or_HTTPS.md) | HTTP request/response cycle, status codes, MITM, Session Hijacking |
| [`TCP_VS_UDP.md`](./TCP_VS_UDP.md) | TCP vs UDP, Three-Way Handshake, SYN Flood attack |
| [`Questions_on_networking.md`](./Questions_on_networking.md) | 42 interview-style Q&A with model answers across all topics |

---

## 📖 What Each File Contains

Each topic file is structured the same way:

- **Theory & explanation** — what the concept is and how it works
- **Step by step breakdowns** — e.g. full DNS resolution chain, TCP handshake flow
- **Security relevance** — how attackers exploit it, how defenders protect against it
- **Key terms & tools** — attack names, protocol details, real tools used

The `Questions_on_networking.md` file is a standalone revision sheet — it contains interview-style questions on every topic in this folder with model answers written the way a junior cybersecurity candidate should answer them.

---

## ⚔️ Attacks Covered

| Attack | Topic | What it does |
|---|---|---|
| MAC Flooding | Switches | Overflows CAM table, turns switch into a hub |
| MAC Spoofing | Switches | Temporarily changes MAC to impersonate a device |
| DNS Spoofing | DNS | Redirects a legitimate domain to a malicious IP |
| DNS Cache Poisoning | DNS | Injects fake DNS records into a resolver's cache |
| MITM Attack | HTTP/HTTPS | Intercepts unencrypted HTTP traffic |
| Session Hijacking | HTTP/HTTPS | Steals session ID to impersonate a logged-in user |
| SYN Flood | TCP/UDP | Exhausts server resources with half-open connections |
| Phishing + C2 Bypass | Firewalls | Uses outgoing traffic to bypass inbound firewall rules |

---

## 🔧 Tools Referenced

| Tool | Used For |
|---|---|
| **Wireshark** | Packet capture and traffic analysis |
| **Nmap** | Port scanning and service discovery |
| **Macof** | MAC Flooding attack tool |
| **Ettercap** | MITM attacks and session hijacking |

---

*Part of my [cybersecurity_roadmap](../README.md) repository*
