# 🌐 Routers

> Routers are what connect networks together and make the internet possible. Every packet that travels from your device to any server in the world passes through multiple routers. Understanding how routers work is essential for understanding how attacks are routed, how traffic is intercepted, and how defences are structured.

---

## 📋 Table of Contents
- [What is a Router?](#what-is-a-router)
- [Router vs Switch](#router-vs-switch)
- [IP Addresses](#ip-addresses)
- [How a Router Works](#how-a-router-works)
- [Routing Table](#routing-table)
- [NAT — Network Address Translation](#nat--network-address-translation)
- [What Happens When You Visit a Website](#what-happens-when-you-visit-a-website)
- [Security Relevance](#security-relevance)
- [Attacks on Routers](#attacks-on-routers)
- [Quick Reference](#quick-reference)

---

## What is a Router?

A router is a network device that **connects different networks together** and routes traffic between them. While a switch connects devices within the same network, a router connects entirely separate networks — including connecting your home network to the internet.

Routers operate at **Layer 3 (Network Layer)** of the OSI model and use **IP addresses** to make routing decisions.

---

## Router vs Switch

| Feature | Router | Switch |
|---|---|---|
| OSI Layer | Layer 3 — Network | Layer 2 — Data Link |
| Uses | IP addresses | MAC addresses |
| Connects | Different networks | Devices within same network |
| Traffic scope | Between networks | Within a network (LAN) |
| Examples | Home router, ISP router | Office switch, home switch |

**Simple analogy:**
- A **switch** is like the post room inside a building — delivers mail between rooms within the building
- A **router** is like the postal service — routes mail between entirely different buildings (networks)

---

## IP Addresses

Routers use IP addresses to identify devices and route traffic. There are two types:

### Public IP
- Globally unique — no two devices on the internet share the same public IP
- Assigned by your ISP (Internet Service Provider)
- Visible to the outside world
- Example: `103.21.244.1`

### Private IP
- Used inside local networks (LAN) — not routable on the internet
- Assigned by your router using DHCP
- Multiple networks can reuse the same private IP ranges without conflict

**Private IP ranges:**
```
10.0.0.0     – 10.255.255.255      (Class A)
172.16.0.0   – 172.31.255.255      (Class B)
192.168.0.0  – 192.168.255.255     (Class C — most common in homes)
```

Any IP address outside these ranges is a public IP.

### IP Address Classes

| Class | Range | Default Use |
|---|---|---|
| A | 1.0.0.0 – 126.x.x.x | Large networks |
| B | 128.0.0.0 – 191.255.x.x | Medium networks |
| C | 192.0.0.0 – 223.255.255.x | Small networks (home/office) |

---

## How a Router Works

When a packet arrives at a router, the router:

1. **Reads the destination IP address** from the packet header
2. **Looks up** the destination in its routing table
3. **Determines the best path** (next hop) to forward the packet
4. **Forwards the packet** out of the appropriate interface
5. **Decrements the TTL** (Time To Live) by 1 — if TTL reaches 0, packet is discarded (prevents infinite loops)

```
Your Device ──► Home Router ──► ISP Router ──► Internet Router ──► Google Server
              (checks table)  (checks table)   (checks table)
```

Each router along the path makes an independent forwarding decision based on its own routing table.

---

## Routing Table

A routing table is a database stored in every router that tells it where to send packets for different destination networks.

**Contents of a routing table:**

| Field | Purpose |
|---|---|
| Destination Network | The network the packet is going to |
| Subnet Mask | Defines the size of the destination network |
| Next Hop | The IP address of the next router to forward to |
| Interface | Which physical port to send the packet out from |
| Metric | Cost of the route — lower is better (used to pick best path) |

**Example routing table:**

```
Destination      Subnet Mask       Next Hop         Interface   Metric
0.0.0.0          0.0.0.0           192.168.1.1      eth0        1      ← Default route (internet)
192.168.1.0      255.255.255.0     0.0.0.0          eth1        0      ← Local network
10.0.0.0         255.0.0.0         10.1.1.1         eth2        5      ← Remote network
```

**Default route (0.0.0.0):** If no specific route matches the destination, the packet is sent to the default route — usually the ISP's router. This is how all internet traffic is handled.

---

## NAT — Network Address Translation

NAT is the process by which a router **translates private IP addresses to a public IP address** (and back) so that devices on a private network can communicate with the internet.

**Why NAT exists:**
- Private IPs cannot be routed on the internet
- IPv4 has a limited number of addresses — NAT allows thousands of devices to share one public IP
- Adds a layer of security — internal devices are hidden from the internet

**How NAT works:**

```
Step 1: Your laptop (192.168.1.5) sends a request to Google
Step 2: Router receives the packet
Step 3: Router replaces source IP:
        192.168.1.5:54321 → 103.21.244.1:54321 (public IP)
Step 4: Router records this translation in its NAT table
Step 5: Packet travels across internet to Google
Step 6: Google responds to 103.21.244.1:54321
Step 7: Router receives response, checks NAT table
Step 8: Router translates back: 103.21.244.1:54321 → 192.168.1.5:54321
Step 9: Your laptop receives the response
```

**NAT Translation Table example:**

| Internal IP | Internal Port | External IP | External Port |
|---|---|---|---|
| 192.168.1.5 | 54321 | 103.21.244.1 | 54321 |
| 192.168.1.8 | 60123 | 103.21.244.1 | 60123 |
| 192.168.1.12 | 49876 | 103.21.244.1 | 49876 |

Multiple devices share one public IP using different port numbers to distinguish their connections.

---

## What Happens When You Visit a Website

Full step-by-step journey of a web request:

```
1. You type google.com in your browser

2. DNS Resolution
   Browser checks cache → DNS Resolver → Root Server
   → TLD Server → Authoritative Server → returns IP

3. Browser creates HTTP/HTTPS request

4. Request travels down OSI layers:
   Layer 7: HTTP request created
   Layer 4: TCP segment created, port 443 (HTTPS)
   Layer 3: IP packet created with destination IP
   Layer 2: Frame created with router's MAC address
   Layer 1: Bits sent over cable/WiFi

5. Home router receives frame
   Strips Layer 2 frame, reads Layer 3 IP packet
   Applies NAT: replaces private IP with public IP
   Forwards packet to ISP router

6. Packet hops across internet routers
   Each router checks routing table and forwards

7. Arrives at Google's server
   OSI layers unwrap in reverse
   Google processes the request

8. Google sends response back
   Same journey in reverse
   Your router applies NAT in reverse
   Your browser receives and renders the page
```

---

## Security Relevance

Routers are critical security infrastructure. Compromising a router gives an attacker control over all traffic passing through it.

**What an attacker with router access can do:**
- Intercept all traffic (MITM)
- Redirect DNS queries to malicious servers
- Block legitimate traffic
- Access all devices on the internal network
- Modify routing tables to misdirect traffic

---

## Attacks on Routers

### 1. IP Spoofing

**How it works:**
An attacker forges the source IP address in a packet header to make it appear as if the packet came from a trusted or different source.

**Uses:**
- Bypass IP-based access controls
- Hide the true origin of an attack
- Conduct DDoS amplification attacks

**Defence:**
- **Ingress filtering** — ISPs check that packets entering their network have valid source IPs
- **IPSec** — authenticates the source of packets

---

### 2. Man-in-the-Middle (MITM) via ARP Poisoning

**How it works:**
As covered in switches, an attacker uses ARP poisoning to redirect traffic through their device. Since routers rely on ARP to resolve next-hop MAC addresses, poisoning ARP caches causes traffic intended for the router to go to the attacker first.

**Defence:**
- Use HTTPS — traffic is encrypted even if intercepted
- Dynamic ARP Inspection
- VPN — encrypts all traffic before it leaves your device

---

### 3. Default Credential Attack

**How it works:**
Many routers ship with default admin usernames and passwords (admin/admin, admin/password). Attackers scan for routers and try default credentials to gain admin access.

**Defence:**
- Always change default router credentials immediately
- Disable remote management if not needed
- Use strong, unique passwords

---

### 4. BGP Hijacking

**How it works:**
BGP (Border Gateway Protocol) is the routing protocol that manages how traffic is routed across the internet between ISPs. An attacker or misconfigured router announces fake routes, causing internet traffic to be misdirected through their network.

**Real world impact:**
- Can redirect entire countries' internet traffic
- Used for traffic interception and surveillance

**Defence:**
- **RPKI (Resource Public Key Infrastructure)** — cryptographically validates BGP route announcements

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Router OSI layer | Layer 3 — Network |
| Router uses | IP addresses |
| Connects | Different networks |
| Routing decision based on | Routing table |
| Private IP ranges | 10.x.x.x / 172.16–31.x.x / 192.168.x.x |
| NAT purpose | Translates private IPs to public IP |
| Default route | 0.0.0.0 — sends unmatched traffic to ISP |
| TTL purpose | Prevents packets looping forever |
| IP Spoofing defence | Ingress filtering, IPSec |
| Router compromise impact | Full network traffic interception |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
