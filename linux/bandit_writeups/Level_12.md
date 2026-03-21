# Bandit Level 11 → Level 12

## 🎯 Objective
The password is stored in `data.txt` where all lowercase (a-z) and uppercase (A-Z) letters have been **rotated by 13 positions** (ROT13).

---

## 💡 Concepts Covered

### What is ROT13?
**ROT13 (Rotate by 13)** is a simple letter substitution cipher that replaces each letter with the letter 13 positions after it in the alphabet.

```
Original:  A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
ROT13:     N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
```

**Example:**
```
Hello World → Uryyb Jbeyq
```

**Key property:** Since the alphabet has 26 letters, applying ROT13 twice returns the original text:
```
Hello → ROT13 → Uryyb → ROT13 → Hello
```
Encoding and decoding use the exact same operation.

**ROT13 is NOT encryption** — it provides zero security. It is used to obscure text casually (e.g. hiding spoilers) not to protect it.

### The `tr` Command
`tr` (translate) replaces or deletes characters in a stream of text.

```bash
tr 'set1' 'set2'
```
Every character from `set1` is replaced with the corresponding character from `set2`.

### Decoding ROT13 with `tr`
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Breaking this down:
- `A-Za-z` → all uppercase and lowercase letters (input)
- `N-ZA-Mn-za-m` → each letter shifted by 13 positions (output)

`tr` does not read files directly — use `cat` and pipe:
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

---

## 🔧 Solution

### Command Used
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

### Steps
1. Logged in as `bandit11`
2. Ran `cat data.txt` — saw scrambled text that looked like ROT13
3. Used `tr` to rotate all letters back by 13 positions
4. Plain text password was revealed

### What the Output Looks Like
```
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4

bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

---

## 🧠 What I Learned

- ROT13 shifts each letter by 13 positions — simple substitution cipher
- ROT13 is self-reversing — applying it twice returns original text
- `tr` translates characters — replacing one set with another
- Piping `cat` into `tr` applies the translation to file contents
- ROT13 provides no real security — purely obfuscation

---

## 🔐 Cybersecurity Relevance

**Encoding vs Encryption — an important distinction:**

| | ROT13 | Base64 | Encryption (AES) |
|---|---|---|---|
| Type | Cipher | Encoding | Encryption |
| Security | None | None | Strong |
| Key needed | No | No | Yes |
| Purpose | Casual obfuscation | Data transport | Data protection |
| Reversible by anyone? | Yes | Yes | No — key required |

**In malware analysis:**
- Malware sometimes uses ROT13 or simple ciphers to obfuscate strings
- Obscures URLs, function names, and strings from basic signature scanning
- Analysts use `tr` or online tools to quickly decode ROT13 strings found in malware

**The `tr` command in security:**
```bash
# Decode ROT13
echo "Uryyb" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# Remove all spaces from a file
cat file.txt | tr -d ' '

# Convert lowercase to uppercase
cat file.txt | tr 'a-z' 'A-Z'

# Extract only printable characters
cat binary_file | tr -cd '[:print:]'
```

---

*Part of [Bandit Writeups](./README.md)*
