# 🦈 Lab 02 — DNS Traffic Analysis

> **Objective:** Capture and analyse real DNS queries and responses using Wireshark to understand how domain names are resolved to IP addresses in real traffic.

---

## 📋 Lab Details

| Field | Details |
|---|---|
| Tool | Wireshark |
| OS | Kali Linux |
| Interface | eth0 |
| Filter Used | `dns` |
| Target Domain | www.google.com |
| DNS Server | 14.139.5.5 |
| Date | March 2026 |
| Difficulty | Beginner |

---

## 🎯 Objective

- Filter Wireshark to show only DNS traffic
- Capture a real DNS query going out from the machine
- Find and read the DNS response coming back
- Identify Transaction ID, TTL, record type, and resolved IP address

---

## 🔧 Steps Performed

### Step 1 — Apply DNS Filter
Typed `dns` in the Wireshark filter bar and pressed Enter. This filtered out all non-DNS traffic showing only DNS queries and responses.

### Step 2 — Generate DNS Traffic
Opened the browser and visited `www.google.com`. This triggered a DNS query from the machine to the configured DNS server.

### Step 3 — Identify the Query Packet
Located the outgoing DNS query packet in the packet list:

```
Source:      10.0.2.15       (my machine)
Destination: 14.139.5.5      (DNS server)
Info:        Standard query 0xd7c4 A www.google.com
```

### Step 4 — Read the Query Packet Details
Expanded **Domain Name System (query)** in the packet detail panel:

| Field | Value | Meaning |
|---|---|---|
| Transaction ID | 0xd7c4 | Unique ID linking query to response |
| Type | Standard query | This is a question, not an answer |
| Answer RRs | 0 | No answer yet — this is just the query |
| Query Name | www.google.com | Domain being looked up |
| Record Type | A (Host Address) | Asking for IPv4 address |
| Class | IN (Internet) | Internet class query |
| Response In | Packet 222 | Wireshark pointer to the matching response |

### Step 5 — Find and Read the Response Packet
Navigated to **packet 222** (as indicated by the `[Response In: 222]` pointer):

```
Source:      14.139.5.5      (DNS server replying)
Destination: 10.0.2.15       (my machine)
Info:        Standard query response 0xd7c4 A www.google.com
```

Expanded **Domain Name System (response)** → **Answers**:

| Field | Value | Meaning |
|---|---|---|
| Transaction ID | 0xd7c4 | Matches the query — confirmed this is the response |
| Type | Standard query response | This is the answer |
| Answer RRs | 1 | One DNS record returned |
| Name | www.google.com | Domain that was resolved |
| Type | A | IPv4 address record |
| Class | IN | Internet |
| TTL | 31 seconds | Cache this result for 31 seconds |
| Address | 142.251.223.174 | Google's actual IP address |

---

## 🔍 Observations

### DNS Resolution Flow Observed Live
```
My machine (10.0.2.15)
        ↓  Standard query 0xd7c4 — "what is www.google.com?"
DNS Server (14.139.5.5)
        ↓  Standard query response 0xd7c4 — "it's 142.251.223.174"
My machine (10.0.2.15)
        ↓  Browser now connects to 142.251.223.174
```

### Transaction ID
The same Transaction ID `0xd7c4` appeared in both the query and response packets. This is how the machine matches responses to requests — especially important when multiple DNS queries are in flight simultaneously.

### TTL of 31 Seconds
Google uses a very low TTL (31 seconds) deliberately. This allows Google to redirect traffic quickly between their global server infrastructure. After 31 seconds the cached record expires and a fresh DNS lookup is triggered.

### DNS Server
The DNS server `14.139.5.5` is a BSNL/government DNS server — the default configured on this network. In a real security scenario, using a more trusted DNS server like Cloudflare `1.1.1.1` or Google `8.8.8.8` is recommended.

---

## 🧠 Security Relevance

### What an Attacker Can Do With This
- **DNS Spoofing** — an attacker on the network could intercept the DNS query and send a fake response before `14.139.5.5` does — redirecting the victim to a malicious IP instead of Google's real IP
- **DNS Surveillance** — since DNS traffic is unencrypted (plain UDP), anyone capturing packets can see every domain you visit even if the actual web traffic is HTTPS encrypted
- **DNS Cache Poisoning** — inject a fake record into the DNS server's cache so all users of that server get redirected

### Real Example From This Lab
```
Normal:   My machine asks 14.139.5.5 "what is google.com?"
          14.139.5.5 replies "142.251.223.174"
          Browser connects to real Google ✅

Spoofed:  My machine asks 14.139.5.5 "what is google.com?"
          Attacker intercepts and replies "192.168.1.100" (fake server)
          Browser connects to attacker's fake site ❌
```

This attack works because DNS has **no authentication by default** — your machine accepts the first response it receives.

### Protection
- **DNSSEC** — cryptographically signs DNS records so fake responses can be detected
- **DNS over HTTPS (DoH)** — encrypts DNS queries so they cannot be intercepted
- **Trusted DNS servers** — Cloudflare `1.1.1.1`, Google `8.8.8.8`

---

## 📌 Key Takeaways

> 1. DNS traffic is unencrypted by default — every domain you visit is visible to anyone on the network
> 2. Transaction IDs link queries to responses — a security weakness that DNS Spoofing exploits
> 3. TTL controls how long DNS records are cached — low TTL means frequent re-lookups
> 4. The DNS server IP is the destination of all outgoing DNS queries

---

## 🔍 Filter Used

```
dns
```

---

*Part of [Wireshark Practical Labs](./README.md)*
