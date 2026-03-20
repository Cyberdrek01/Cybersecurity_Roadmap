# 🦈 Wireshark — Practical Labs

> This folder contains hands-on Wireshark lab notes from real packet capture sessions. Each file documents what was captured, what was observed, and how it connects to cybersecurity concepts learned in theory.

---

## 📋 Lab Index

| Lab | Topic | Status |
|---|---|---|
| [Lab 01 — Live Traffic Capture](./Lab_01_Live_Traffic_Capture.md) | Interface selection, live packet capture | ✅ Complete |
| [Lab 02 — DNS Traffic Analysis](./Lab_02_DNS_Traffic_Analysis.md) | DNS queries, responses, Transaction ID, TTL | ✅ Complete |
| [Lab 03 — TCP Handshake](./Lab_03_TCP_Handshake.md) | SYN, SYN-ACK, ACK, FIN-ACK capture | ✅ Complete |
| [Lab 04 — HTTP Stream](./Lab_04_HTTP_Stream.md) | HTTP request/response, plain text interception | ✅ Complete |

---

## 🛠️ Setup

- **Tool:** Wireshark
- **OS:** Kali Linux
- **Interface:** eth0
- **Method:** Live capture on local network interface

---

## 🔑 Key Skills Demonstrated

- Live packet capture on a network interface
- Applying Wireshark display filters
- Reading and interpreting DNS query/response packets
- Identifying TCP Three-Way Handshake in real traffic
- Following HTTP streams and reading plain text web traffic
- Understanding why HTTPS encryption is critical

---

## 🔍 Filters Used

| Filter | Purpose |
|---|---|
| `dns` | Show only DNS traffic |
| `tcp` | Show only TCP traffic |
| `tcp.flags.syn == 1` | Show only SYN packets |
| `tcp and ip.addr == [IP]` | Show TCP traffic to/from specific IP |
| `http` | Show only HTTP traffic |

---

*Part of the [Networking](../README.md) practical section.*
