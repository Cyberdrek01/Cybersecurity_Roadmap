# 🌍 DNS — Domain Name System

> DNS is often called the "phone book of the internet." It is one of the most critical and most attacked protocols in networking. A deep understanding of DNS is essential for both web hacking and network defence.

---

## 📋 Table of Contents
- [What is DNS?](#what-is-dns)
- [Why DNS Exists](#why-dns-exists)
- [DNS Resolution Process](#dns-resolution-process)
- [DNS Record Types](#dns-record-types)
- [DNS Caching & TTL](#dns-caching--ttl)
- [Security Relevance](#security-relevance)
- [DNS Attacks](#dns-attacks)
- [DNS in Cybersecurity Tools](#dns-in-cybersecurity-tools)
- [Quick Reference](#quick-reference)

---

## What is DNS?

**DNS (Domain Name System)** is a hierarchical distributed system that translates human-readable domain names (like `google.com`) into machine-readable IP addresses (like `142.250.190.14`).

Without DNS, you would need to memorize the IP address of every website you want to visit.

**DNS operates at Layer 7 (Application Layer)** of the OSI model and uses **UDP port 53** for standard queries (TCP port 53 for large responses and zone transfers).

---

## Why DNS Exists

Computers communicate using IP addresses — but humans remember names, not numbers. DNS bridges this gap:

```
Human types:    google.com
DNS translates: google.com → 142.250.190.14
Computer uses:  142.250.190.14 to connect
```

DNS also allows websites to change their server IP addresses without users noticing — the domain name stays the same even if the IP changes.

---

## DNS Resolution Process

When you type a domain name in your browser, the following happens:

```
Step 1: Browser Cache Check
        Has this domain been visited recently?
        If YES → use cached IP (skip the rest)
        If NO  → proceed to Step 2

Step 2: OS Cache / hosts file Check
        Check local system cache and /etc/hosts file
        If found → use cached IP
        If not  → proceed to Step 3

Step 3: DNS Resolver (Recursive Resolver)
        Your device contacts the DNS Resolver
        (usually provided by your ISP or configured manually e.g. 1.1.1.1)
        The resolver does the heavy lifting on your behalf

Step 4: Root DNS Server
        Resolver contacts one of 13 Root DNS Servers worldwide
        Root server doesn't know google.com's IP
        But knows which TLD server handles .com
        Returns: "Ask the .com TLD server"

Step 5: TLD (Top Level Domain) Server
        Resolver contacts the .com TLD server
        TLD server knows which Authoritative server handles google.com
        Returns: "Ask Google's Authoritative server"

Step 6: Authoritative DNS Server
        Resolver contacts Google's Authoritative DNS Server
        This server has the actual DNS records for google.com
        Returns the IP address: 142.250.190.14

Step 7: Resolver Returns IP
        Resolver caches the result (for future queries)
        Returns IP to your browser

Step 8: Browser Connects
        Browser uses the IP to connect to Google's web server
```

**Visual diagram:**
```
Browser → Resolver → Root Server
                   ↓
              TLD Server (.com)
                   ↓
         Authoritative Server (google.com)
                   ↓
              IP: 142.250.190.14
                   ↓
         Resolver → Browser → Google
```

---

## DNS Record Types

DNS records are entries stored in Authoritative DNS servers that define how a domain behaves.

| Record | Full Name | Purpose | Example |
|---|---|---|---|
| **A** | Address | Maps domain to IPv4 address | `google.com → 142.250.190.14` |
| **AAAA** | IPv6 Address | Maps domain to IPv6 address | `google.com → 2607:f8b0::200e` |
| **CNAME** | Canonical Name | Aliases one domain to another | `www.google.com → google.com` |
| **MX** | Mail Exchange | Specifies mail servers for domain | `google.com → smtp.google.com` |
| **TXT** | Text | Stores text info (SPF, DMARC, verification) | Domain ownership proof |
| **NS** | Name Server | Identifies authoritative DNS servers | `google.com → ns1.google.com` |
| **PTR** | Pointer | Reverse DNS — maps IP to domain | `142.250.190.14 → google.com` |
| **SOA** | Start of Authority | Admin info about the DNS zone | Zone serial, refresh intervals |

**Cybersecurity relevance of each:**
- **A / AAAA** — used in reconnaissance to find server IPs
- **CNAME** — can reveal internal infrastructure aliases
- **MX** — targeted in email spoofing attacks
- **TXT** — SPF/DMARC records help prevent email spoofing
- **NS** — targeted in DNS enumeration to find all nameservers
- **PTR** — used in reverse DNS lookups during reconnaissance

---

## DNS Caching & TTL

### Caching
DNS caching stores the results of DNS queries temporarily so the full resolution process doesn't repeat on every visit. Caching happens at multiple levels:
- Browser cache
- Operating system cache
- DNS Resolver cache

**Benefits:**
- Faster page load times
- Reduced load on DNS servers
- Lower internet traffic

### TTL (Time To Live)
Every DNS record has a TTL value — the number of seconds the record should be cached before being refreshed.

```
A Record: google.com → 142.250.190.14  TTL: 300
(This record is valid for 300 seconds / 5 minutes)
```

**Low TTL** — record refreshes frequently, good for dynamic environments
**High TTL** — record cached longer, better performance but slower to update

### Security risk of caching:
If an attacker can inject a malicious DNS record into a resolver's cache, all users querying that resolver get directed to the malicious IP — until the TTL expires. This is **DNS Cache Poisoning**.

---

## Security Relevance

DNS is one of the most abused protocols in cybersecurity because:
- It is largely unencrypted (plain text by default)
- It is trusted implicitly by almost every device
- It is rarely blocked by firewalls (port 53 is almost always open)
- It can be used to bypass security controls entirely

---

## DNS Attacks

### 1. DNS Spoofing

**What it is:**
An attacker sends forged DNS responses to a victim, pointing a legitimate domain to a malicious IP address.

**How it works:**
```
Normal:   victim types bank.com → DNS returns real IP → victim connects to real bank
Spoofed:  victim types bank.com → attacker's fake DNS response → victim connects to fake site
```

**Impact:**
- Credential theft — victim enters login details on fake site
- Malware delivery — fake site serves malicious downloads
- Phishing at scale

**Defence:**
- **DNSSEC** — digitally signs DNS records so fake responses can be detected
- Use **HTTPS** — even if redirected to a fake site, the SSL certificate mismatch warns the user
- Use trusted DNS servers: Cloudflare `1.1.1.1`, Google `8.8.8.8`

---

### 2. DNS Cache Poisoning

**What it is:**
An attacker injects a malicious DNS record into a resolver's cache. Unlike DNS Spoofing which targets one user, Cache Poisoning affects **all users** of that resolver until the TTL expires.

**How it works:**
1. Attacker floods the resolver with fake DNS responses for a target domain
2. If a fake response arrives before the real one and with the correct transaction ID, the resolver accepts it
3. The fake record is cached
4. Every user querying that resolver for the domain gets the attacker's IP

**Famous example:** The Kaminsky Attack (2008) — a critical vulnerability discovered in DNS that made cache poisoning much easier.

**Defence:**
- **DNSSEC** — validates DNS responses with cryptographic signatures
- **Source port randomization** — makes it harder to guess transaction IDs
- **0x20 encoding** — randomizes letter case in queries to validate responses

---

### 3. DNS Enumeration

**What it is:**
A reconnaissance technique where an attacker queries DNS records to map out a target organisation's infrastructure.

**What attackers discover:**
- All subdomains (mail.company.com, vpn.company.com, dev.company.com)
- IP addresses of servers
- Mail server infrastructure
- Internal naming conventions

**Tools used:**
```bash
# Basic DNS lookup
nslookup google.com

# Detailed DNS query
dig google.com ANY

# Subdomain enumeration
dnsenum google.com
sublist3r -d google.com
```

**Defence:**
- Disable **DNS Zone Transfers** to unauthorised sources
- Use **split-horizon DNS** — internal and external DNS serve different records

---

### 4. DNS Tunneling

**What it is:**
An attacker encodes data inside DNS queries and responses to smuggle data in and out of a network — bypassing firewalls that block other protocols but allow DNS (port 53).

**How it works:**
```
Attacker's malware on internal machine encodes stolen data as:
data.chunk1.attacker.com (DNS query)
data.chunk2.attacker.com (DNS query)

Attacker's DNS server receives queries, extracts the data
```

**Why it works:** Most firewalls allow DNS traffic (port 53) because blocking it would break internet access.

**Defence:**
- **DNS traffic analysis** — look for unusually long domain names or high DNS query volume
- **DNS filtering** — block known malicious domains
- **SIEM monitoring** — alert on abnormal DNS patterns

---

### 5. DNS Amplification Attack (DDoS)

**What it is:**
An attacker uses open DNS resolvers to amplify a DDoS attack against a target.

**How it works:**
1. Attacker spoofs the victim's IP as the source of DNS queries
2. Attacker sends small DNS queries to many open resolvers requesting large responses (ANY record type)
3. All resolvers send large responses to the victim's IP
4. Victim gets flooded with traffic it never asked for

**Amplification factor:** A small 40-byte query can generate a 4000-byte response — 100x amplification.

**Defence:**
- **Rate limiting** on DNS servers
- **Disable open DNS resolvers** — only answer queries from your own network
- **BCP38** — ISP-level ingress filtering to prevent IP spoofing

---

## DNS in Cybersecurity Tools

| Tool | How it uses DNS |
|---|---|
| **nslookup** | Basic DNS lookup — find IP for a domain |
| **dig** | Detailed DNS queries — all record types |
| **dnsenum** | Automated DNS enumeration |
| **Sublist3r** | Subdomain discovery using DNS |
| **Wireshark** | Capture and analyse DNS traffic |
| **Burp Suite** | Intercept DNS-related web requests |

**Useful DNS servers for security research:**
| Server | IP | Provider |
|---|---|---|
| Cloudflare | 1.1.1.1 | Privacy-focused, fast |
| Google | 8.8.8.8 | Reliable, global |
| OpenDNS | 208.67.222.222 | Filtering capabilities |

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| DNS stands for | Domain Name System |
| DNS protocol/port | UDP port 53 (TCP for large/zone transfers) |
| OSI layer | Layer 7 — Application |
| DNS resolution order | Browser cache → OS cache → Resolver → Root → TLD → Authoritative |
| A record | Domain → IPv4 |
| AAAA record | Domain → IPv6 |
| MX record | Mail servers for domain |
| NS record | Authoritative name servers |
| TTL | How long a record is cached |
| DNS Spoofing | Fake DNS response to single user |
| DNS Cache Poisoning | Fake record injected into resolver cache |
| DNS Tunneling | Data exfiltration through DNS queries |
| DNSSEC | Cryptographic validation of DNS records |
| Trusted DNS | Cloudflare 1.1.1.1 / Google 8.8.8.8 |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
