# Bandit Level 03 → Level 04

## 🎯 Objective
The password for the next level is stored in a hidden file in the `inhere` directory.

---

## 💡 Concepts Covered

### Hidden Files in Linux
In Linux, any file or directory whose name **starts with a dot (.)** is hidden. Hidden files do not appear in a normal `ls` listing.

Examples of hidden files:
```
.bashrc         ← shell configuration
.bash_history   ← command history
.ssh/           ← SSH keys directory
.env            ← environment variables (often contains secrets)
```

### How to See Hidden Files
```bash
ls -a        # show ALL files including hidden ones
ls -la       # show ALL files with detailed info (permissions, size, owner)
```

The `-a` flag stands for **all** — it reveals hidden files.

---

## 🔧 Solution

### Commands Used
```bash
cd inhere
ls -a
cat .hidden
```

### Steps
1. Logged in as `bandit3`
2. Ran `ls` — saw a directory called `inhere`
3. Ran `cd inhere` to enter the directory
4. Ran `ls` — appeared empty
5. Ran `ls -a` — revealed a hidden file called `.hidden`
6. Ran `cat .hidden` to read the password

### What the Output Looks Like
```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
bandit3@bandit:~/inhere$ cat .hidden
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```

---

## 🧠 What I Learned

- Files starting with `.` are hidden in Linux
- `ls` does not show hidden files by default
- `ls -a` reveals all hidden files
- `.` means current directory, `..` means parent directory
- Hidden files are commonly used for configuration and storing sensitive data

---

## 🔐 Cybersecurity Relevance

Hidden files are extremely important in cybersecurity:

**For attackers:**
- Malware often hides itself in hidden directories to avoid detection
- Attackers store tools, scripts, and stolen data in hidden files
- Common hiding spots: `/tmp/.hidden/`, `~/.cache/`, inside hidden directories

**For defenders/SOC:**
- During incident response, always run `ls -la` not just `ls`
- Check `.bash_history` for commands run by attackers
- Check `.ssh/authorized_keys` for backdoor SSH keys added by attackers
- Hidden files in web directories can be webshells left by attackers

---

*Part of [Bandit Writeups](./README.md)*
