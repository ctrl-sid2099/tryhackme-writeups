# 🎣 Introduction to Phishing — TryHackMe SOC Simulator

<img width="620" height="562" alt="lab" src="https://github.com/user-attachments/assets/213d5acf-51e3-43b0-b4d8-2aa6f4417162" />

## 📌 Scenario Objectives

* Monitor and analyze real-time alerts
* Investigate suspicious emails and links
* Document findings and create case reports
* Determine whether alerts are malicious or false positives

<img width="1006" height="623" alt="1 lab" src="https://github.com/user-attachments/assets/7e2c92ca-61ef-40e2-9cc8-37b9ef60a0c8" />

---

# 🚨 Alert Investigation — ID 8814

## 📋 Alert Details

| Field    | Value                                             |
| -------- | ------------------------------------------------- |
| Alert ID | 8814                                              |
| Rule     | Inbound Email Containing Suspicious External Link |
| Severity | Medium                                            |
| Type     | Phishing                                          |


### 📧 Email Summary

* **Sender:** [onboarding@hrconnex.thm](mailto:onboarding@hrconnex.thm)
* **Recipient:** [j.garcia@thetrydaily.thm](mailto:j.garcia@thetrydaily.thm)
* **Subject:** Action Required: Finalize Your Onboarding Profile
* **Attachment:** None
* **Direction:** Inbound

The email requested the recipient to complete an onboarding profile through an external link associated with `hrconnex.thm`.

<img width="1191" height="685" alt="2 1-alert-info" src="https://github.com/user-attachments/assets/9436967f-3434-4dd5-8e9c-40f7703c7d7e" />

---

## 🔎 Investigation Process

The domain `hrconnex.thm` was searched in the SIEM to validate legitimacy.

### Findings

* Multiple related email logs were identified
* Internal HR communication referenced the same onboarding platform/domain
* No malicious attachments were detected
* No suspicious outbound connections or compromise indicators were observed

<img width="1365" height="710" alt="8814-siem" src="https://github.com/user-attachments/assets/55127db5-6302-4632-8e90-a2f6c6c0c45a" />

### 🧠 Analysis Conclusion

The alert was determined to be a **False Positive**.

The email was part of a legitimate onboarding workflow, and the alert was triggered because the message contained an external hyperlink.

---

## 📝 Case Report

### ⏰ Time of Activity

14:57:13 PM

---

### 👥 Related Entities

* **Sender:** [onboarding@hrconnex.thm](mailto:onboarding@hrconnex.thm)
* **Recipient:** [j.garcia@thetrydaily.thm](mailto:j.garcia@thetrydaily.thm)
* **Domain:** hrconnex.thm

---

### 🔗 URL

```text
https://hrconnex.thm/onboarding/15400654060/j.garcia
```

---

### ✅ Reason for False Positive Classification

Investigation confirmed that the email was legitimate and related to HR onboarding activities.
The sender domain was validated through internal communications, and no indicators of compromise were identified.

The alert was triggered solely due to the presence of an external onboarding link.

---

## 🎯 Final Verdict

✅ False Positive
No malicious activity detected.

---

# 🚨 Phishing Investigation — Alert ID 8815

## 📋 Alert Details

| Field    | Value                                             |
| -------- | ------------------------------------------------- |
| Alert ID | 8815                                              |
| Rule     | Inbound Email Containing Suspicious External Link |
| Severity | Medium                                            |
| Type     | Phishing                                          |

### 📧 Email Summary

* **Sender:** [urgents@amazon.biz](mailto:urgents@amazon.biz)
* **Recipient:** [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm)
* **Subject:** Your Amazon Package Couldn’t Be Delivered – Action Required
* **Attachment:** None
* **Direction:** Inbound

The email attempted to impersonate Amazon and used urgency-based language to pressure the recipient into clicking a shortened URL.

<img width="1191" height="692" alt="3 2-alert" src="https://github.com/user-attachments/assets/8a8f8ca9-d338-49ff-9c86-832690251695" />

---

## 🔎 Investigation Process

The suspicious URL was searched in the SIEM:

```text id="m2w1jk"
http://bit.ly/3sHkX3da12340
```

### 🔥 Firewall Log Findings

| Field            | Value            |
| ---------------- | ---------------- |
| Action           | Blocked          |
| Application      | Web Browsing     |
| Source IP        | 10.20.2.17       |
| Destination IP   | 67.199.248.11    |
| Destination Port | 80               |
| Rule             | Blocked Websites |

The logs confirmed that an outbound connection attempt to the malicious URL occurred, but the firewall successfully blocked the traffic.

<img width="1305" height="379" alt="8815-siem" src="https://github.com/user-attachments/assets/d8e52425-3ce6-46c8-9da2-82bc38a8fcd6" />

---

## 🧪 Threat Validation

The shortened URL was analyzed using:

* [VirusTotal](https://www.virustotal.com)

The URL was confirmed to be associated with phishing activity.

<img width="1239" height="654" alt="3 2-alert-virustotal-" src="https://github.com/user-attachments/assets/929e17c3-9d22-435b-9c04-ef8e867ce97e" />

---

## 📝 Case Report

### ⏰ Time of Activity

05/28/2026 2:01:40 PM

---

### 👥 Affected Entities

* **Sender:** [urgents@amazon.biz](mailto:urgents@amazon.biz)
* **Recipient:** [h.harris@thetrydaily.thm](mailto:h.harris@thetrydaily.thm)
* **Malicious URL:** http://bit.ly/3sHkX3da12340
* **Source IP:** 10.20.2.17
* **Destination IP:** 67.199.248.11

---

### 🚨 Reason for True Positive Classification

The email contained several phishing indicators:

* Suspicious sender domain impersonating Amazon
* Use of a shortened URL to hide the destination
* Urgency-based social engineering tactics
* Delivery-themed phishing lure

Firewall logs confirmed user interaction with the embedded URL, and the outbound request was blocked before a connection to the malicious site could be established.

---

### ⬆️ Escalation Reason

The alert required escalation because the phishing attempt posed a potential risk of:

* Credential theft
* Malware delivery
* Additional phishing attempts targeting other users

Further investigation was necessary to ensure no additional systems or users were impacted.

---

### 🛠️ Recommended Remediation Actions

* Block the sender domain `amazon.biz`
* Add malicious URLs and indicators to blocklists
* Review firewall and email logs for similar activity
* Perform endpoint scans on affected devices
* Continue monitoring for phishing attempts
* Provide phishing awareness guidance to users

---

### 🧩 Indicators of Compromise (IOCs)

* Suspicious sender domain: `amazon.biz`
* Shortened phishing URL: `bit.ly`
* Destination IP: `67.199.248.11`
* Phishing-themed email content
* Outbound connection attempt blocked by firewall

---

## 🎯 Final Verdict

✅ True Positive
✅ Escalated for further investigation

---

# 🚨 Firewall Investigation — Alert ID 8816

## 📋 Alert Details

| Field    | Value                                                  |
| -------- | ------------------------------------------------------ |
| Alert ID | 8816                                                   |
| Rule     | Access to Blacklisted External URL Blocked by Firewall |
| Severity | High                                                   |
| Type     | Firewall                                               |


<img width="1192" height="704" alt="4 alert" src="https://github.com/user-attachments/assets/e5c14297-ef69-4809-8262-cda7ffd4ac45" />

### 🌐 Firewall Event Summary

* **Action:** Blocked
* **Source IP:** 10.20.2.17
* **Destination IP:** 67.199.248.11
* **URL:** http://bit.ly/3sHkX3da12340
* **Application:** Web Browsing
* **Protocol:** TCP

---

## 🔎 Investigation Summary

This alert was directly related to the previously investigated phishing email from Alert **8815**.

The firewall detected and successfully blocked an outbound connection attempt to the malicious phishing URL.

The malicious link had already been confirmed as phishing during the earlier investigation.

---

## 🛡️ Security Analysis

### Findings

* Outbound request was blocked successfully
* No connection to the malicious destination was established
* Firewall protections worked as intended
* Activity matched previously identified phishing indicators

---

## 📝 Case Report

### ⏰ Time of Activity

05/28/2026 15:01:40

---

### 👥 Related Entities

* **Source IP:** 10.20.2.17
* **Destination IP:** 67.199.248.11
* **URL:** http://bit.ly/3sHkX3da12340

---

### ✅ Reason for True Positive Classification

The alert confirmed a real phishing-related outbound connection attempt toward a known malicious URL.

Although the firewall successfully blocked the traffic, the activity represented legitimate malicious behavior and validated the earlier phishing investigation.

---

### ⬆️ Escalation Reason

Escalation was required to:

* Verify whether additional users interacted with the phishing campaign
* Ensure no related malicious activity occurred elsewhere in the environment
* Continue monitoring for similar phishing attempts

---

### 🛠️ Recommended Actions

* Continue monitoring for related phishing activity
* Maintain URL and domain blocklists
* Review email and firewall logs for additional indicators
* Conduct awareness training for users

---

## 🎯 Final Verdict

✅ True Positive
✅ Escalated
🛡️ Firewall successfully blocked the malicious connection attempt

---

# 🚨 Phishing Investigation — Alert ID 8817

## 📋 Alert Details

| Field    | Value                                             |
| -------- | ------------------------------------------------- |
| Alert ID | 8817                                              |
| Rule     | Inbound Email Containing Suspicious External Link |
| Severity | Medium                                            |
| Type     | Phishing                                          |

### 📧 Email Summary

* **Sender:** [no-reply@m1crosoftsupport.co](mailto:no-reply@m1crosoftsupport.co)
* **Recipient:** [c.allen@thetrydaily.thm](mailto:c.allen@thetrydaily.thm)
* **Subject:** Unusual Sign-In Activity on Your Microsoft Account
* **Attachment:** None
* **Direction:** Inbound

The email impersonated Microsoft and attempted to trick the user into reviewing suspicious sign-in activity through a phishing link.

<img width="1191" height="694" alt="5 alert" src="https://github.com/user-attachments/assets/110f914a-fdcc-47ea-8c10-4217a651e4ba" />

---

## 🔎 Investigation Process

The suspicious URL was searched in the SIEM:

```text id="v6f3zr"
https://m1crosoftsupport.co/login
```

### 🔥 Firewall Log Findings

| Field          | Value          |
| -------------- | -------------- |
| Action         | Allowed        |
| Source IP      | 10.20.2.25     |
| Destination IP | 45.148.10.131  |
| Protocol       | TCP            |
| Rule           | Allow-Internet |

The logs confirmed that the phishing URL was accessed from an internal endpoint and the outbound connection was allowed by the firewall.

<img width="1307" height="650" alt="siem" src="https://github.com/user-attachments/assets/3c62059f-da95-415e-bb50-e83cb17c3613" />

---

## 🧪 Threat Analysis

The domain `m1crosoftsupport.co` used **typosquatting** by replacing the letter **"i"** in Microsoft with the number **"1"**.

Additional phishing indicators included:

* Microsoft impersonation
* Fear and urgency-based language
* Suspicious external login page
* User interaction confirmed through firewall logs

The URL was also validated as malicious through:

* [VirusTotal](https://www.virustotal.com)

<img width="1208" height="553" alt="6 VirusTotal" src="https://github.com/user-attachments/assets/904ddff1-7c7d-43b8-9397-879abe0b4a00" />

---

## 📝 Case Report

### ⏰ Time of Activity

05/28/2026 15:02:44

---

### 👥 Affected Entities

* **Sender:** [no-reply@m1crosoftsupport.co](mailto:no-reply@m1crosoftsupport.co)
* **Recipient:** [c.allen@thetrydaily.thm](mailto:c.allen@thetrydaily.thm)
* **Malicious URL:** https://m1crosoftsupport.co/login
* **Source IP:** 10.20.2.25
* **Destination IP:** 45.148.10.131

---

### 🚨 Reason for True Positive Classification

The email was identified as a phishing attempt due to:

* Typosquatted Microsoft-themed domain
* Social engineering tactics involving account security alerts
* Confirmed user interaction with the phishing URL
* Outbound connection successfully allowed by the firewall

The activity created a potential risk of credential theft and account compromise.

---

### ⬆️ Escalation Reason

The alert required escalation because the phishing URL was successfully accessed from an internal endpoint.

Immediate investigation was necessary to:

* Determine whether credentials were submitted
* Investigate the affected endpoint for compromise
* Identify additional phishing activity within the environment

---

### 🛠️ Recommended Remediation Actions

* Block `m1crosoftsupport.co`
* Add phishing indicators to organizational blocklists
* Investigate endpoint `10.20.2.25`
* Reset affected user credentials and enforce MFA
* Review authentication and proxy logs
* Continue monitoring for similar phishing attempts

---

### 🧩 Indicators of Compromise (IOCs)

* Typosquatted domain: `m1crosoftsupport.co`
* Microsoft impersonation
* Phishing login page
* Outbound connection allowed by firewall
* Destination IP: `45.148.10.131`

---

## 🎯 Final Verdict

✅ True Positive
✅ Escalated for further investigation

---

# ✅ Room Completed Successfully

All alerts in the **Introduction to Phishing** SOC simulation room were successfully investigated and documented.

During this exercise, multiple phishing-related alerts were analyzed using SIEM and firewall logs to determine whether the activity was malicious or legitimate.

<img width="1599" height="784" alt="success" src="https://github.com/user-attachments/assets/4bda8269-672b-4592-b4ce-f5ac85d5d17a" />

---

# 📌 Investigation Summary

### 🔹 Alert 8814 — HR Onboarding Email

* Investigated suspicious onboarding email containing an external link
* Validated domain legitimacy through internal HR communications
* Determined to be a **False Positive**

### 🔹 Alert 8815 — Amazon Delivery Phishing Email

* Identified phishing email impersonating Amazon
* Detected shortened malicious URL
* Firewall logs confirmed connection attempt was blocked
* Classified as a **True Positive** and escalated

### 🔹 Alert 8816 — Firewall Blocked Malicious URL

* Confirmed firewall successfully blocked access to the phishing URL from Alert 8815
* Verified protection controls worked as intended
* Classified as a **True Positive** and escalated

### 🔹 Alert 8817 — Microsoft Phishing Attempt

* Identified typosquatted Microsoft phishing domain
* Confirmed user interaction through firewall logs
* Classified as a **True Positive** and escalated

---

# 🏅 Achievements Unlocked

During the completion of this SOC simulation scenario, the following achievements were earned:

* ✅ **100% True Positive Rate**
* 🎯 **First Scenario Completed**
* 🚨 **First Alert Closed**

These achievements highlight successful alert investigation, accurate phishing detection, and proper incident handling throughout the exercise.

<img width="1159" height="237" alt="achivement" src="https://github.com/user-attachments/assets/5845f9a3-53fe-43ca-af92-264f28641e37" />

---

# 🎯 Conclusion

This room provided practical SOC analyst experience involving:

* 📊 SIEM log investigation
* 🔥 Firewall monitoring
* 🎣 Phishing detection & analysis
* 🛡️ Threat validation
* 📝 Incident documentation
* 🚨 Alert escalation workflows

The investigations demonstrated how phishing campaigns use impersonation, shortened URLs, typosquatting, and social engineering techniques to target users, while also showing the importance of firewall controls and log analysis during incident response.

---

🏆 Room Status: Completed Successfully


👩‍💻 Author: ctrl-sid2099




