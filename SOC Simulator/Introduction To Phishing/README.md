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

<img width="1239" height="484" alt="2" src="https://github.com/user-attachments/assets/23031c17-3b58-4936-bd7b-63fe416892b4" />

### 📧 Email Summary

* **Sender:** [onboarding@hrconnex.thm](mailto:onboarding@hrconnex.thm)
* **Recipient:** [j.garcia@thetrydaily.thm](mailto:j.garcia@thetrydaily.thm)
* **Subject:** Action Required: Finalize Your Onboarding Profile
* **Attachment:** None
* **Direction:** Inbound

The email requested the recipient to complete an onboarding profile through an external link associated with `hrconnex.thm`.

<img width="1219" height="684" alt="2 1-alert-info" src="https://github.com/user-attachments/assets/68874a83-4183-4f46-b806-2acf922120e0" />

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

# 📝 Case Report

## ⏰ Time of Activity

14:57:13 PM

## 👥 Related Entities

* **Sender:** [onboarding@hrconnex.thm](mailto:onboarding@hrconnex.thm)
* **Recipient:** [j.garcia@thetrydaily.thm](mailto:j.garcia@thetrydaily.thm)
* **Domain:** hrconnex.thm

## 🔗 URL

```text
https://hrconnex.thm/onboarding/15400654060/j.garcia
```

## ✅ Reason for False Positive Classification

Investigation confirmed that the email was legitimate and related to HR onboarding activities.
The sender domain was validated through internal communications, and no indicators of compromise were identified.

The alert was triggered solely due to the presence of an external onboarding link.

---

## 🎯 Final Verdict

✅ False Positive
No malicious activity detected.

---

