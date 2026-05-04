# 🧪 RootMe Lab Writeup (With Explanation)

## 📌 Overview

This lab demonstrates a typical penetration testing workflow:

1. Reconnaissance (finding attack surface)
2. Initial access (getting a shell)
3. Privilege escalation (becoming root)

---

# 🔍 Task 2: Reconnaissance

## 🎯 Goal:

Identify open services and possible entry points.

---

## Step 1: Port Scanning

```bash
nmap -sV <Target-IP>
```

<img width="944" height="506" alt="1 scan" src="https://github.com/user-attachments/assets/73685cf8-95d9-4ebe-b61f-d11723950473" />

### ❓ Why we did this:

We need to know **what services are exposed** on the machine. Open ports = possible attack vectors.

### ⚙️ What the command does:

* `nmap` → Network scanner
* `-sV` → Detects service versions running on open ports

### 📊 Results:

* Port 22 → OpenSSH 8.2p1
* Port 80 → Apache httpd 2.4.41

### ✅ Questions Solved:

* Scan the machine, how many ports are open? → **2**
* What version of Apache is running? → **2.4.41**
* What service is running on port 22? → **SSH**

---

## Step 2: Web Enumeration

```bash
gobuster dir -u http://<Target-IP> -w /usr/share/wordlists/dirb/common.txt
```

<img width="1076" height="704" alt="3 panel-port-80" src="https://github.com/user-attachments/assets/2c7ebb77-1370-4fcc-a9dc-12c4b9fa22f5" />

### ❓ Why we did this:

Websites often have **hidden directories** not linked on the homepage. These can expose sensitive functionality (like uploads, admin panels).

### ⚙️ What the command does:

* `gobuster dir` → Brute forces directories
* `-u` → Target URL
* `-w` → Wordlist of possible directory names

### 📂 Findings:

* `/Hidden1` → Upload page (entry point!)
* `/Hidden2` → Stores uploaded files

### ✅ Questions Solved:

* Find directories using GoBuster → **No answer needed**
* What is the hidden directory? → **/Hidden1**

---

# 💻 Task 3: Getting a Shell

## 🎯 Goal:

Upload and execute malicious code to gain system access.

---

## Step 1: Upload Reverse Shell

Navigate to:

```
http://<Target-IP>/Hidden1
```

### ❓ Why we did this:

File upload functionality can be abused to upload **malicious scripts**.

We used:

* PHP reverse shell (pentestmonkey)

<img width="1110" height="783" alt="4 php" src="https://github.com/user-attachments/assets/bc3d32da-a767-4364-af7f-693605303619" />

### ❌ Problem:

PHP files were blocked.

<img width="612" height="636" alt="5 php-not-allowed" src="https://github.com/user-attachments/assets/e1fe458e-47fd-494a-a97b-0a40d780fc67" />

### ✅ Bypass:

```bash
php-reverse-shell.php → php-reverse-shell.php5
```

<img width="603" height="668" alt="6 success" src="https://github.com/user-attachments/assets/4580eb98-a091-4678-aa8c-1eba197ab0db" />

### ❓ Why this works:

Some servers only block `.php` but still execute `.php5`.

---

## Step 2: Start Listener

```bash
nc -nvlp 4444
```

<img width="718" height="354" alt="7 listen" src="https://github.com/user-attachments/assets/cb6c9ce3-6f49-46a1-908a-2e094dccdef7" />

### ❓ Why we did this:

A reverse shell connects **back to us**, so we must listen for incoming connections.

### ⚙️ Command breakdown:

* `nc` → Netcat (network tool)
* `-n` → No DNS resolution
* `-v` → Verbose (shows connection details)
* `-l` → Listen mode
* `-p 4444` → Port to listen on

---

## Step 3: Trigger Shell

Go to:

```
http://<Target-IP>/Hidden2
```

Click the uploaded file.

<img width="633" height="343" alt="8 uploads" src="https://github.com/user-attachments/assets/56f013c8-5fd5-4f91-8ff1-3ad56172f805" />

### 🎯 Result:

Shell connection received.

<img width="787" height="487" alt="9 success-rsh" src="https://github.com/user-attachments/assets/746e3ae2-0c1f-4e14-8a08-69bd9276926a" />

---

## Step 4: Find User Flag

```bash
find / -name user.txt 2>/dev/null
```

<img width="782" height="472" alt="10 user-flag" src="https://github.com/user-attachments/assets/32b80a2c-a15c-43cc-a7ee-84e7e8d33420" />

### ❓ Why we did this:

We search the entire system for the flag file.

### ⚙️ Command breakdown:

* `find /` → Search from root directory
* `-name user.txt` → Look for file
* `2>/dev/null` → Hide permission errors

```bash
cat /var/www/user.txt
```

### ❓ What it does:

Displays file contents.

### ✅ Question Solved:

* user.txt → **THM{xyz}**

---

# 🔐 Task 4: Privilege Escalation

## 🎯 Goal:

Gain root access (highest privilege).

---

## Step 1: Find SUID Files

```bash
find / -name -4000 2>/dev/null
```

<img width="480" height="315" alt="11 sudo-permision" src="https://github.com/user-attachments/assets/df01fb65-cad9-4054-a2e0-b76d3fdf795e" />

### ❓ Why we did this:

SUID files run with **owner's privileges (often root)** even when executed by normal users. Misconfigured ones can be exploited.

### 📌 Interesting Result:

```
/usr/bin/python2.7
```

### ✅ Question Solved:

* Weird SUID file → **/usr/bin/python2.7**

---

## Step 2: Exploit SUID Python

```bash
/usr/bin/python2.7 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

### ❓ Why this works:

* `setuid(0)` → Switches to root user (UID 0)
* Because Python has SUID, it executes with root privileges

### 🎯 Result:

Root shell obtained.

---

## Step 3: Find Root Flag

```bash
find / -name root.txt 2>/dev/null
```

```bash
cat /root/root.txt
```

<img width="688" height="135" alt="12 1 root-flag" src="https://github.com/user-attachments/assets/58111286-ede9-4815-aba1-ac5fb48c376c" />

### ✅ Questions Solved:

* Find a form to escalate privileges → **No answer needed**
* root.txt → **THM{abc}**

<img width="1042" height="629" alt="12 success" src="https://github.com/user-attachments/assets/12701770-6ee6-4ad1-8259-d09f07a944aa" />

---

# 🏁 Conclusion

### 🔑 Key Learnings:

* Always start with **enumeration**
* Hidden directories often lead to entry points
* File upload = high-risk vulnerability
* Reverse shells give remote control
* SUID misconfigurations are powerful for privilege escalation


