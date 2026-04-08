# ⚔️ Brute Force Attack (T1110)

---

## 🎯 Objective

Simulate a brute force attack against a Windows system using Hydra and detect failed authentication attempts in Splunk.

---

## 🧠 MITRE ATT&CK Mapping

- Technique: T1110  
- Name: Brute Force  

---

## 🛠️ Tools Used

- Kali Linux (Hydra)  
- Windows 10 (Target)  
- Splunk SIEM  

---

## ⚙️ Attack Execution

Run from Kali Linux:

```bash
hydra -l user1 -P rockyou.txt rdp://192.168.56.100
```

---

## 🧩 Steps Performed

1. Open Kali Linux attacker machine  
2. Ensure connectivity to target (192.168.56.100)  
3. Use Hydra with username and password list  
4. Launch brute force attack  
5. Monitor failed login attempts  

---

## 📊 Telemetry Generated

### Windows Security Logs

- Event ID 4625 → Failed Logon  
- Source IP → 192.168.56.250  

---

## 🔍 Detection in Splunk

### Basic Detection

```spl
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address
| where count > 10
```

---

### Advanced Detection

```spl
index=wineventlog EventCode=4625
| bucket _time span=1m
| stats count by _time, Account_Name, Source_Network_Address
| where count > 5
```

---

## 🧠 Detection Logic

- Identify multiple failed logins in short time  
- Same source IP targeting same account  
- High frequency authentication failures  

---

## ⚠️ False Positives

- User entering wrong password repeatedly  
- Misconfigured services  

### Mitigation

- Set threshold (e.g., >10 attempts)  
- Exclude service accounts  

---

## 📸 Screenshots Required

- Hydra attack running  
- Windows Event Viewer logs (4625)  
- Splunk detection results  

Paths:

- screenshots/attacks/bruteforce.png  
- screenshots/detections/bruteforce-splunk.png  

---

## 🚀 Outcome

- Successfully simulated brute force attack  
- Generated failed login logs  
- Detected activity using Splunk  

---
