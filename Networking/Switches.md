# 🔀 Switches

> A switch is one of the most fundamental devices in any network. Understanding how switches work — and how attackers exploit them — is essential for both offensive and defensive cybersecurity.

---

## 📋 Table of Contents
- [What is a Switch?](#what-is-a-switch)
- [Switch vs Hub](#switch-vs-hub)
- [MAC Addresses](#mac-addresses)
- [How a Switch Works](#how-a-switch-works)
- [MAC Address Table (CAM Table)](#mac-address-table-cam-table)
- [Security Relevance](#security-relevance)
- [Attacks on Switches](#attacks-on-switches)
- [Quick Reference](#quick-reference)

---

## What is a Switch?

A switch is a network device that connects multiple devices within a **LAN (Local Area Network)**. Its main job is to receive incoming data and forward it **only to the correct destination device** — rather than sending it to everyone on the network.

Switches operate at **Layer 2 (Data Link Layer)** of the OSI model and use **MAC addresses** to make forwarding decisions.

---

## Switch vs Hub

| Feature | Switch | Hub |
|---|---|---|
| OSI Layer | Layer 2 | Layer 1 |
| Addressing | Uses MAC addresses | No addressing |
| Traffic forwarding | Sends to specific port only | Broadcasts to all ports |
| Security | More secure | Less secure |
| Efficiency | High | Low |
| Used today? | Yes | Mostly obsolete |

**Why this matters in cybersecurity:**
A hub broadcasts all traffic to every device — meaning any device on the network can see everyone else's traffic. A switch eliminates this by forwarding only to the correct port. However, certain attacks can force a switch to behave like a hub (see MAC Flooding below).

---

## MAC Addresses

A **MAC (Media Access Control) address** is a unique physical identifier assigned to every network interface card (NIC).

**Format:** `00:1A:2B:3C:4D:5E` — 6 pairs of hexadecimal numbers separated by colons

**Key properties:**
- Assigned by the manufacturer and burned into hardware
- Unique to every device in the world (theoretically)
- Operates at Layer 2 — only used within a local network
- Does **not** travel across routers onto the internet (unlike IP addresses)

**MAC vs IP:**
| Property | MAC Address | IP Address |
|---|---|---|
| Layer | Layer 2 | Layer 3 |
| Scope | Local network only | Global (internet) |
| Assigned by | Manufacturer | Network/ISP |
| Changes? | Fixed in hardware* | Changes across networks |
| Format | 00:1A:2B:3C:4D:5E | 192.168.1.1 |

*MAC addresses can be temporarily changed in software — this is called **MAC Spoofing**.

---

## How a Switch Works

When a device sends data through a switch, the following happens:

```
Step 1: Device A sends a frame destined for Device C
        Frame contains:
        - Source MAC:      AA:AA:AA:AA:AA:AA  (Device A)
        - Destination MAC: CC:CC:CC:CC:CC:CC  (Device C)

Step 2: Switch receives the frame on Port 1
        Switch records: Port 1 → AA:AA:AA:AA:AA:AA (learns Device A)

Step 3: Switch looks up CC:CC:CC:CC:CC:CC in its MAC address table
        - If FOUND → forwards frame only to that specific port
        - If NOT FOUND → floods frame to all ports (except source port)
                         and waits to learn where Device C is

Step 4: Device C receives the frame
        Switch records: Port 3 → CC:CC:CC:CC:CC:CC (learns Device C)

Step 5: All future frames from A to C go directly Port 1 → Port 3
        No other device sees this traffic
```

---

## MAC Address Table (CAM Table)

The **MAC Address Table** (also called the **CAM Table — Content Addressable Memory**) is a database stored inside the switch that maps MAC addresses to physical ports.

**Example CAM Table:**

| Port | MAC Address | VLAN |
|---|---|---|
| Port 1 | AA:AA:AA:AA:AA:AA | 1 |
| Port 2 | BB:BB:BB:BB:BB:BB | 1 |
| Port 3 | CC:CC:CC:CC:CC:CC | 1 |
| Port 4 | DD:DD:DD:DD:DD:DD | 1 |

**How the switch builds it:**
- Every time a device sends data, the switch reads the **source MAC address** and the **port it arrived on**
- It stores this mapping in the CAM table
- Entries expire after a set time if no traffic is seen (typically 300 seconds)

**What happens if a MAC is unknown:**
The switch **floods** the frame to all ports except the source. This is called an **unknown unicast flood**. Once the destination device responds, the switch learns its port and adds it to the table.

---

## Security Relevance

Switches are more secure than hubs because traffic is isolated per port — but they are not immune to attack. Several techniques can force a switch to expose traffic it should be hiding.

**ARP and switches:**
Switches use MAC addresses, but devices on the network communicate using IP addresses. **ARP (Address Resolution Protocol)** is used to resolve IP addresses to MAC addresses within a LAN. This resolution process can be abused (see ARP Poisoning).

---

## Attacks on Switches

### 1. MAC Flooding (CAM Table Overflow Attack)

**How it works:**
1. Attacker uses a tool (Macof) to flood the switch with thousands of fake frames containing random source MAC addresses
2. The CAM table has a limited size — it fills up rapidly
3. Once full, the switch **cannot store new MAC entries**
4. The switch enters **fail-open mode** — it starts flooding all traffic to all ports (behaves like a hub)
5. Attacker's machine now receives all traffic on the network and can capture sensitive data

```
Normal switch:   A → [Switch] → C only
After flooding:  A → [Switch] → ALL ports (attacker sees everything)
```

**Tool used:** `macof` (part of the dsniff suite)

**Defence:**
- **Port Security** — limit the number of MAC addresses allowed per port
- Set violation action to shutdown or restrict when limit is exceeded

---

### 2. MAC Spoofing

**How it works:**
An attacker temporarily changes their device's MAC address to impersonate another device on the network. Since switches forward traffic based on MAC addresses, the switch sends traffic intended for the victim to the attacker instead.

**Why it's possible:**
While MAC addresses are burned into hardware, operating systems allow them to be overridden in software.

**Linux command to spoof MAC:**
```bash
ip link set dev eth0 down
ip link set dev eth0 address 00:11:22:33:44:55
ip link set dev eth0 up
```

**Use cases for attackers:**
- Bypass MAC-based access controls
- Impersonate another device to receive its traffic
- Evade network monitoring that tracks by MAC address

**Defence:**
- **Dynamic ARP Inspection (DAI)**
- **802.1X Port Authentication** — requires credentials, not just MAC address

---

### 3. ARP Poisoning (ARP Spoofing)

**How it works:**
ARP has no authentication — any device can send an ARP reply claiming any IP-to-MAC mapping. An attacker sends fake ARP replies to poison the ARP cache of victims, redirecting their traffic through the attacker's machine.

**Steps:**
1. Attacker sends fake ARP reply to Victim: "The router's IP is at MY MAC address"
2. Victim updates its ARP cache with the fake mapping
3. All traffic from Victim intended for the router now goes to the attacker
4. Attacker forwards it to the real router (so victim doesn't notice)
5. Attacker is now silently intercepting all traffic — **MITM attack**

**Defence:**
- **Dynamic ARP Inspection (DAI)** on managed switches
- **Static ARP entries** for critical devices
- Use **HTTPS** so intercepted traffic is encrypted and useless to attacker

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Switch OSI layer | Layer 2 — Data Link |
| Switch uses | MAC addresses |
| CAM table maps | MAC address → Port |
| Unknown MAC behaviour | Flood to all ports |
| Hub difference | Hub broadcasts; switch forwards to correct port only |
| MAC format | 00:1A:2B:3C:4D:5E |
| MAC Flooding tool | macof |
| MAC Flooding defence | Port Security |
| MAC Spoofing | Temporarily changing MAC in software |
| ARP Poisoning defence | Dynamic ARP Inspection (DAI) |

---

*Part of the [Networking](./README.md) section of my cybersecurity learning journey.*
