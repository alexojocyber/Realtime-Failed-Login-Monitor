#  Real-Time Failed Login Monitoring Script (Bash)

A lightweight real-time monitoring script written in **Bash** to detect and alert on failed login attempts by watching the system authentication log (`/var/log/auth.log`).

This project is part of my ongoing **Cybersecurity Automation Learning Journey**, following my completion of the **Google Cybersecurity Professional Certificate** and building my GitHub portfolio with practical security tools.

---

##  Project Overview  

This Bash script continuously monitors failed login attempts on a Linux system using `tail -f` and highlights:

-  ❗ Username involved  
- 🌐 IP address of the attempt  
- 🕒 Exact timestamp  
- 🚨 Real-time alerts for each failed attempt  

It’s a beginner-friendly but **real-world** automation project inspired by log monitoring practices used in SOC environments.

---

##  Features  

- Reads authentication logs in real-time  
- Detects failed SSH/login attempts  
- Extracts username, IP address, and timestamp  
- Displays clean, formatted alerts  
- Works like a **mini SIEM component** on Linux  
- Great stepping stone to building larger log analysis tools or alerts systems  

---

##  Technologies Used  
- **Bash scripting**  
- **Linux file permissions**  
- **rsyslog** (for generating and managing auth logs)  
- **tail, awk, grep, cut**  

---

#  Screenshots  

### **1️⃣ Script Initialization**

<img width="1812" height="363" alt="start-monitoring png" src="https://github.com/user-attachments/assets/813f8dcd-acab-4b41-b1b4-2af84dd99ef9" />


---

### **2️⃣ Log File Missing Error (Before Fix)**

<img width="1592" height="297" alt="log-not-found" src="https://github.com/user-attachments/assets/c39cab7d-eb52-4811-88d2-908f1caf752a" />


---

### **3️⃣ Installing and Enabling rsyslog**

<img width="1920" height="772" alt="rsyslog-install png" src="https://github.com/user-attachments/assets/7c5333e1-0aae-4001-8924-0ffb8663aa47" />



---

### **4️⃣ Successful Real-Time Alerts (Final Output)**

<img width="1178" height="807" alt="alerts-working png" src="https://github.com/user-attachments/assets/74fe8096-73e5-4e6d-b02f-351176d2f1ae" />



---

#  How the Script Works  

```bash
#!/bin/bash

logfile="/var/log/auth.log"

echo "[+] Starting Failed Login Monitor..."
echo "[+] Watching $logfile"
echo

tail -Fn0 "$logfile" | \
while read line; do
    if echo "$line" | grep -qi "failed password"; then
        user=$(echo "$line" | awk '{print $(NF-5)}')
        ip=$(echo "$line" | awk '{print $(NF-3)}')
        time=$(echo "$line" | awk '{print $1, $2, $3}')

        echo "🚨 ALERT: Failed login attempt!"
        echo "User: $user"
        echo "IP: $ip"
        echo "Time: $time"
        echo "---------------------------------------------"
    fi
done
