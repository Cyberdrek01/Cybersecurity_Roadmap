# Bandit Level 17 → Level 18

## 🧩 Level Goal

Find the password for the next level by identifying the **only changed line** between:

* `passwords.old`
* `passwords.new`

---

## 🛠️ Tools Used

* `ls`
* `diff`
* `grep`

---

## 📂 Step 1: List Files

```bash
ls
```

Output:

```
passwords.old
passwords.new
```

---

## 🔍 Step 2: Compare Files

```bash
diff passwords.old passwords.new
```

### Example Output:

```
< oldpassword123
---
> newpassword456
```

* `<` → line from old file
* `>` → line from new file ✅

---

## 🔑 Step 3: Extract Password

The password is the line marked with `>`:

```
newpassword456
```

---

## ⚡ Alternative Method (Efficient)

```bash
grep -Fxv -f passwords.old passwords.new
```

This directly prints the changed line.

---

## 🚀 Step 4: Login to Next Level

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

---

## ⚠️ Note

* Seeing `Byebye!` after login is **normal**.
* This is part of the next level (Level 18 → 19).

---

## 🎯 Key Learning

* File comparison using `diff`
* Filtering unique lines using `grep`
