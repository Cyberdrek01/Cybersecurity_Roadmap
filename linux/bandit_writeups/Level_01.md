# Bandit Level 00 → Level 01

## 🎯 Objective
The password for the next level is stored in a file called `readme` located in the home directory.

---

## 💡 Concepts Covered

### Key Commands
| Command | Purpose |
|---|---|
| `ls` | List files and directories |
| `cat` | Read and display file contents |
| `cd` | Change directory |

### What is the Home Directory?
When you log into a Linux system, you land in your **home directory** — represented as `~`. For user bandit0 this is `/home/bandit0`.

---

## 🔧 Solution

### Commands Used
```bash
ls
cat readme
```

### Steps
1. Logged in as `bandit0`
2. Ran `ls` to list files in the home directory
3. Saw a file called `readme`
4. Ran `cat readme` to read its contents
5. Password for Level 1 was displayed

### What the Output Looks Like
```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```

---

## 🧠 What I Learned

- `ls` lists all visible files in the current directory
- `cat` reads and prints file contents to the terminal
- Linux home directory is represented as `~`
- Passwords in Bandit are random strings used to SSH into the next level

---

## 🔐 Cybersecurity Relevance

In real penetration testing and incident response, reading files is one of the first things done after gaining access to a system:
- **Pentesters** look for config files, credential files, and notes left by admins
- **SOC analysts** read log files to investigate incidents
- Common sensitive files to check: `/etc/passwd`, `~/.bash_history`, config files with hardcoded passwords

---

*Part of [Bandit Writeups](./README.md)*
