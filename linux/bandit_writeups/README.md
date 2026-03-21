# 🐧 OverTheWire — Bandit Writeups

> Bandit is a wargame designed for absolute beginners to learn Linux commands and basic cybersecurity concepts. Each level requires finding a password hidden somewhere on the system using Linux commands — the password unlocks the next level.

---

## 📋 What is Bandit?

**Platform:** [overthewire.org/wargames/bandit](https://overthewire.org/wargames/bandit)

Bandit teaches:
- Linux command line navigation
- File permissions and ownership
- Encoding and decoding (Base64, ROT13, Hex)
- SSH keys and connections
- Port scanning and networking
- Privilege escalation basics
- File compression and archiving

Every concept learned here directly applies to real cybersecurity work — SOC analysts, pentesters, and security engineers use these skills daily.

---

## 📊 Progress

```
Level 00 → 01   ✅
Level 01 → 02   ✅
Level 02 → 03   ✅
Level 03 → 04   ✅
Level 04 → 05   ✅
Level 05 → 06   ✅
Level 06 → 07   ✅
Level 07 → 08   ✅
Level 08 → 09   ✅
Level 09 → 10   ✅
Level 10 → 11   ✅
Level 11 → 12   ✅
Level 12 → 13   ✅
Level 13 → 14   ✅
Level 14 → 15   ✅
Level 15 → 16   ⏳
Level 16 → 17   ⏳
...
Level 33 → 34   🔒
```

**Current:** Level 15 complete — 15/34

---

## 🔗 Level Index

| Level | Key Concept | Status |
|---|---|---|
| [Level 00 → 01](./Level_00.md) | SSH connection basics | ✅ |
| [Level 01 → 02](./Level_01.md) | Reading files with special names | ✅ |
| [Level 02 → 03](./Level_02.md) | Files with spaces in name | ✅ |
| [Level 03 → 04](./Level_03.md) | Hidden files | ✅ |
| [Level 04 → 05](./Level_04.md) | Finding human-readable files | ✅ |
| [Level 05 → 06](./Level_05.md) | Finding files by properties | ✅ |
| [Level 06 → 07](./Level_06.md) | Finding files by owner/group | ✅ |
| [Level 07 → 08](./Level_07.md) | Searching inside files with grep | ✅ |
| [Level 08 → 09](./Level_08.md) | Finding unique lines | ✅ |
| [Level 09 → 10](./Level_09.md) | Strings in binary files | ✅ |
| [Level 10 → 11](./Level_10.md) | Base64 decoding | ✅ |
| [Level 11 → 12](./Level_11.md) | ROT13 cipher | ✅ |
| [Level 12 → 13](./Level_12.md) | Hex dump and compression | ✅ |
| [Level 13 → 14](./Level_13.md) | SSH private keys | ✅ |
| [Level 14 → 15](./Level_14.md) | Connecting to local ports | ✅ |
| Level 15 → 16 | SSL/TLS connections | ⏳ |
| Level 16 → 17 | Port scanning with Nmap | ⏳ |

---

## 💡 How to Connect to Bandit

```bash
# Connect to any level
ssh bandit[level]@bandit.labs.overthewire.org -p 2220

# Example — connect to level 5
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

---

*Part of the [Linux](../README.md) section of my cybersecurity learning journey.*
