# Bandit Level 01 → Level 02

## 🎯 Objective
The password for the next level is stored in a file called `-` located in the home directory.

---

## 💡 Concepts Covered

### The Problem With `-`
In Linux, `-` is a **special character**. When used with commands it is interpreted as an argument flag rather than a filename:
- `cat -` tells the terminal to read from **standard input** (your keyboard) instead of a file
- The terminal just waits forever for keyboard input — it does not read the file

This is a fundamental Linux concept — certain characters have special meanings and must be handled carefully.

### How to Handle Special Characters
To tell Linux "this is a filename, not a flag" you can:

**Method 1 — Use `./` prefix (relative path)**
```bash
cat ./-
```
The `./` means "in the current directory" — Linux now treats `-` as a filename not a flag.

**Method 2 — Use full path**
```bash
cat /home/bandit1/-
```

---

## 🔧 Solution

### Commands Used
```bash
ls
cat ./-
```

### Steps
1. Logged in as `bandit1`
2. Ran `ls` — saw a file named `-`
3. Tried `cat -` — terminal hung waiting for input (special character issue)
4. Used `cat ./-` to specify it as a filename
5. Password displayed

### What the Output Looks Like
```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```

---

## 🧠 What I Learned

- `-` is a special character in Linux — means standard input when used alone
- Prefix `./` tells Linux to treat something as a filename in the current directory
- Always use `./filename` when a filename starts with a special character

---

## 🔐 Cybersecurity Relevance

Understanding special characters is critical in cybersecurity:
- **Command injection attacks** exploit special characters to break out of intended commands
- **Pentesters** use special characters like `;`, `|`, `&`, `-` to inject additional commands
- Knowing how Linux interprets special characters helps both in exploiting and defending systems

---

*Part of [Bandit Writeups](./README.md)*
