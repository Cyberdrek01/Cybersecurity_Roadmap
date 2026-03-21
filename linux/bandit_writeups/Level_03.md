# Bandit Level 02 → Level 03

## 🎯 Objective
The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.

---

## 💡 Concepts Covered

### The Problem With Spaces in Filenames
In Linux, **spaces are used to separate command arguments**. If you type:
```bash
cat --spaces in this filename--
```
Linux reads this as: run `cat` with arguments `--spaces`, `in`, `this`, `filename--` — four separate arguments, not one filename. This causes an error.

### How to Handle Spaces in Filenames

**Method 1 — Wrap in quotes**
```bash
cat "--spaces in this filename--"
```
Single quotes also work:
```bash
cat '--spaces in this filename--'
```

**Method 2 — Escape each space with backslash**
```bash
cat --spaces\ in\ this\ filename--
```
The `\` before each space tells Linux "this space is part of the filename, not a separator."

**Method 3 — Use Tab autocomplete**
Type the first few characters and press `Tab` — Linux autocompletes the filename and handles spaces automatically. This is the fastest method in practice.

---

## 🔧 Solution

### Commands Used
```bash
ls
cat "--spaces in this filename--"
```

### Steps
1. Logged in as `bandit2`
2. Ran `ls` — saw file with spaces and dashes in the name
3. Initially confused by the dashes AND spaces in the filename
4. Searched online — found that wrapping in quotes handles both spaces and dashes
5. Used `cat "--spaces in this filename--"` to read the file
6. Password displayed

### What the Output Looks Like
```
bandit2@bandit:~$ ls
--spaces in this filename--
bandit2@bandit:~$ cat "--spaces in this filename--"
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```

---

## 🧠 What I Learned

- Spaces in filenames break Linux commands unless handled correctly
- Three ways to handle spaces: quotes, backslash escaping, or Tab autocomplete
- Dashes in filenames combined with spaces require the same quote wrapping
- Tab autocomplete is the fastest practical solution

---

## 💭 What Was Tricky
The filename had both **dashes and spaces** which was confusing at first. A quick Google search revealed that wrapping the entire filename in quotes handles both issues at once.

---

## 🔐 Cybersecurity Relevance

- Attackers sometimes name malicious files with spaces or special characters to confuse administrators and security tools
- Understanding how shells interpret filenames is essential for reading logs and investigating files during incident response
- Tab autocomplete is a practical skill that speeds up work significantly in real SOC and pentesting environments

---

*Part of [Bandit Writeups](./README.md)*
