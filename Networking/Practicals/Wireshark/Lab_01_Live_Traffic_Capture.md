# 🦈 Lab 01 — Live Traffic Capture

> **Objective:** Open Wireshark, select the correct network interface, and observe live packets flowing through the network.

---

## 📋 Lab Details

| Field | Details |
|---|---|
| Tool | Wireshark |
| OS | Kali Linux |
| Interface | eth0 |
| Date | March 2026 |
| Difficulty | Beginner |

---

## 🎯 Objective

- Open Wireshark and identify the active network interface
- Start a live packet capture
- Observe real network traffic flowing in real time

---

## 🔧 Steps Performed

### Step 1 — Open Wireshark
Launched Wireshark on Kali Linux.

### Step 2 — Identify Active Interface
On the Wireshark home screen, a list of available network interfaces is displayed. The active interface shows a **wavy line** next to it indicating live traffic is flowing.

**Active interface found:** `eth0`

### Step 3 — Start Capture
Double clicked on `eth0` to begin live capture. Packets immediately began flowing in — visible as rows of green, blue, and black entries in the packet list.

---

## 🔍 Observations

- Packets began appearing immediately after starting capture on `eth0`
- Multiple protocols visible — DNS, TCP, UDP, ARP
- Each packet row shows: Number, Time, Source IP, Destination IP, Protocol, Length, Info
- Traffic was continuous — showing the machine is constantly communicating even without any browser activity

---

## 🧠 What This Demonstrates

Even when you are not actively browsing, your machine is constantly sending and receiving packets — background services, DNS lookups, keep-alive signals, ARP requests, and more. This is important in cybersecurity because:

- An attacker on the same network can capture all this traffic passively
- Background traffic can reveal information about your OS, services, and network
- This is why network monitoring (blue team / SOC) is a full-time job — there is always traffic to analyse

---

## 📌 Key Takeaway

> Wireshark captures every single packet passing through the interface — whether you initiated it or not. On an unencrypted network, this means anyone running Wireshark can see everything.

---

*Part of [Wireshark Practical Labs](./README.md)*
