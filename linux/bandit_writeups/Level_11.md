# Bandit Level 10 → Level 11

## 🎯 Objective
The password is stored in `data.txt` which contains **Base64 encoded data**.

---

## 💡 Concepts Covered

### What is Base64?
Base64 is an encoding scheme that converts binary data into ASCII text using 64 printable characters (A-Z, a-z, 0-9, +, /). It is NOT encryption — it is just a different representation of the same data.

**Key points:**
- Base64 encoded text often ends with `=` or `==` padding
- It looks like: `aGVsbG8gd29ybGQ=`
- Anyone can decode it — no key needed
- Used to safely transmit binary data over text-based systems

**Base64 vs Encryption:**
| | Base64 | Encryption |
|---|---|---|
| Purpose | Encoding for transport | Protecting data |
| Reversible? | Yes — by anyone | Only with the key |
| Security | None | Strong |
| Example use | Email attachments, data URLs | HTTPS, passwords |

### Decoding Base64 in Linux
```bash
base64 -d filename         # decode a file
echo "encoded" | base64 -d # decode a string
```

---

## 🔧 Solution

### Command Used
```bash
base64 -d data.txt
```

### Steps
1. Logged in as `bandit10`
2. Ran `cat data.txt` — saw a long Base64 string
3. Used `base64 -d data.txt` to decode it
4. Password was revealed in plain text

### What the Output Looks Like
```
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIDZ6VFRJV3JOd05OZVRrNTVvWVVMYzFPdTBTeUJkTHFOCg==
bandit10@bandit:~$ base64 -d data.txt
The password is 6zTTIWrNwNNeTk55oYULc1Ou0SyBdLqN
```

---

## 🧠 What I Learned

- Base64 is encoding not encryption — anyone can decode it
- `-d` flag decodes Base64 (without `-d` it encodes)
- Base64 strings are recognisable by their character set and `=` padding at the end
- Base64 is used legitimately in many places but also by attackers to obfuscate payloads

---

## 🔐 Cybersecurity Relevance

Base64 appears constantly in cybersecurity:

**Attackers use Base64 to:**
- Obfuscate malicious PowerShell commands
- Hide payloads in scripts to bypass basic detection
- Encode shellcode in exploits

**Common attack example — PowerShell:**
```powershell
powershell -EncodedCommand VwByAGkAdABlAC0ASABvAHMAdAAgACIASABlAGwAbABvACIA
```
The `-EncodedCommand` flag accepts Base64 encoded commands — attackers use this to hide what the command actually does.

**SOC detection:**
- Base64 in PowerShell command line arguments is a red flag
- SIEM rules often alert on `EncodedCommand` or long Base64 strings in process arguments
- Always decode Base64 found in suspicious scripts or logs

**Quick decode in terminal:**
```bash
echo "VGhlIHBhc3N3b3Jk" | base64 -d
```

---

*Part of [Bandit Writeups](./README.md)*
