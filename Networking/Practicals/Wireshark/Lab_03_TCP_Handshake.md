# 🦈 Lab 03 — TCP Three-Way Handshake

> **Objective:** Capture and analyse a real TCP Three-Way Handshake in Wireshark — observing the SYN, SYN-ACK, ACK sequence and the FIN-ACK connection termination in live traffic.

---

## 📋 Lab Details

| Field | Details |
|---|---|
| Tool | Wireshark |
| OS | Kali Linux |
| Interface | eth0 |
| Filter Used | `tcp.flags.syn == 1` |
| Date | March 2026 |
| Difficulty | Beginner |

---

## 🎯 Objective

- Filter Wireshark to show TCP SYN packets
- Identify a complete TCP Three-Way Handshake in real traffic
- Read sequence numbers, flags, and port numbers
- Observe the full TCP connection lifecycle including FIN-ACK termination
- Connect observations back to SYN Flood attack theory

---

## 🔧 Steps Performed

### Step 1 — Apply SYN Filter
Typed `tcp.flags.syn == 1` in the Wireshark filter bar. This filtered the packet list to show only packets with the SYN flag set — the very first step of every TCP handshake.

### Step 2 — Observe SYN Packets
Immediately visible in the packet list:
- Multiple SYN packets from the machine to various servers
- SYN-ACK packets from servers back to the machine
- Source ports were all high ephemeral ports (above 49152)

### Step 3 — Capture a Clean Full Handshake
Cleared the capture, applied filter and visited `http://example.com`. The following sequence was captured:

```
No.   Source        Destination    Info
───   ──────────    ───────────    ────────────────────────
1     10.0.2.15     104.18.26.120  [SYN]        Seq=0
2     104.18.26.120 10.0.2.15      [SYN, ACK]   Seq=0 Ack=1
3     10.0.2.15     104.18.26.120  [ACK]         Seq=1 Ack=1
      ─────────────────── DATA TRANSFER ───────────────────
4     10.0.2.15     104.18.26.120  [FIN, ACK]   Seq=1 Ack=1
```

---

## 🔍 Packet Analysis

### Packet 1 — SYN (Connection Request)

| Field | Value | Meaning |
|---|---|---|
| Source IP | 10.0.2.15 | My machine initiating connection |
| Destination IP | 104.18.26.120 | Remote web server |
| Source Port | 39702 | Ephemeral port — randomly assigned |
| Destination Port | 80 | HTTP service on server |
| Flag | SYN | "I want to connect" |
| Sequence Number | 0 (relative) | Starting sequence number |
| Acknowledgment | 0 | Nothing to acknowledge yet |

**Port analysis:**
- Source port **39702** — ephemeral port (above 49152), randomly assigned by OS for this connection, discarded after session ends
- Destination port **80** — well-known port for HTTP, always the same

### Packet 2 — SYN-ACK (Connection Accepted)

| Field | Value | Meaning |
|---|---|---|
| Source IP | 104.18.26.120 | Server now responding |
| Destination IP | 10.0.2.15 | Back to my machine |
| Flag | SYN, ACK | "I received your SYN, I'm ready" |
| Sequence Number | 0 (relative) | Server's starting sequence number |
| Acknowledgment | 1 | Acknowledges my SYN |

**Note:** Source and Destination are now reversed — the server is replying. This is expected behavior in a TCP handshake.

### Packet 3 — ACK (Connection Confirmed)

| Field | Value | Meaning |
|---|---|---|
| Source IP | 10.0.2.15 | My machine confirming |
| Destination IP | 104.18.26.120 | Back to server |
| Flag | ACK | "Confirmed — connection established" |
| Sequence Number | 1 | Incremented after SYN |
| Acknowledgment | 1 | Acknowledges server's SYN-ACK |

**Connection is now fully established. Data transfer begins.**

### Packet 4 — FIN-ACK (Connection Termination)

| Field | Value | Meaning |
|---|---|---|
| Source IP | 10.0.2.15 | My machine closing connection |
| Flag | FIN, ACK | "I'm done — closing connection" |

**My machine sent FIN-ACK first** — meaning the browser finished receiving data and initiated the graceful close.

---

## 📊 Full TCP Lifecycle Captured

```
CLIENT (10.0.2.15)              SERVER (104.18.26.120)
        │                               │
        │──────── SYN ────────────────►│  Step 1: Request connection
        │                               │
        │◄─────── SYN-ACK ────────────│  Step 2: Accept connection
        │                               │
        │──────── ACK ────────────────►│  Step 3: Confirm — CONNECTED
        │                               │
        │════════ DATA TRANSFER ════════│  Web page data flows
        │                               │
        │──────── FIN, ACK ───────────►│  Step 4: Close connection
        │                               │
```

---

## 🧠 Security Relevance

### SYN Flood Attack — Theory Confirmed by Lab

Looking at this capture, the SYN Flood attack makes perfect sense:

```
Normal handshake:
SYN → SYN-ACK → ACK ✅ (connection completes, resources freed)

SYN Flood:
SYN → SYN-ACK → ??? (attacker never sends ACK)
SYN → SYN-ACK → ???
SYN → SYN-ACK → ???  × 10,000
```

After sending SYN-ACK, the server allocates memory for each half-open connection and waits for the final ACK. The attacker never sends it. The server's connection table fills up — it cannot accept any new legitimate connections.

**CIA Triad impact:** SYN Flood directly attacks **Availability** — one third of the CIA Triad. The server is still running but legitimate users cannot connect.

**Defence — SYN Cookies:**
Instead of allocating resources after SYN-ACK, the server encodes connection state in the SYN-ACK packet itself. Resources are only allocated when the valid ACK arrives — making SYN Flood attacks ineffective.

### Port Scanning with Nmap
Nmap's SYN scan (`nmap -sS`) works by sending SYN packets and reading the response:
- **SYN-ACK response** → port is open (service is running)
- **RST response** → port is closed
- **No response** → port is filtered by firewall

This is exactly what was captured in this lab — just like Nmap, Wireshark shows who responds to SYN packets and on which ports.

### Ephemeral Ports
Every connection uses a random high port (39702 in this lab) as the source. This means:
- Each new connection gets a new random port
- Makes it harder for attackers to predict or spoof connections
- After the session ends, the ephemeral port is released

---

## 📌 Key Takeaways

> 1. TCP always starts with SYN → SYN-ACK → ACK before any data flows
> 2. Source and Destination IPs flip in the SYN-ACK — the server is now replying
> 3. Ephemeral source ports (above 49152) are randomly assigned per connection
> 4. FIN-ACK gracefully closes the connection — different from RST which forcefully terminates
> 5. SYN Flood exploits the gap between SYN-ACK and ACK — the server waits forever

---

## 🔍 Filters Used

```
tcp.flags.syn == 1                    ← Show only SYN packets
tcp and ip.addr == [destination IP]   ← Show full conversation with specific host
```

---

*Part of [Wireshark Practical Labs](./README.md)*
