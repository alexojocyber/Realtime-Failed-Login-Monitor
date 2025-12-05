Real-Time Failed Login Monitoring Script (Bash)

A lightweight real-time monitoring script written in Bash to detect and alert on failed login attempts by watching the system authentication log (/var/log/auth.log).

This project is part of my ongoing Cybersecurity Automation Learning Journey, following my completion of the Google Cybersecurity Professional Certificate and building my GitHub portfolio with practical security tools.

Project Overview

This Bash script continuously monitors failed login attempts on a Linux system using tail -f and highlights:

Username involved

IP address of the attempt

Exact timestamp

Real-time alerts for each failed attempt

It’s a beginner-friendly but real-world automation project inspired by log monitoring practices used in SOC environments.

Features

Reads authentication logs in real-time

Detects failed SSH/login attempts

Extracts username, IP address, and timestamp

Displays clean, formatted alerts

Works like a mini SIEM component on Linux

Great stepping stone to building larger log analysis tools or alerts systems

Technologies Used

Bash scripting

Linux file permissions

rsyslog (for generating and managing auth logs)

tail, awk, grep, cut


Screenshots


1 Script Initialization
<img width="1812" height="363" alt="start-monitoring png" src="https://github.com/user-attachments/assets/99a7b69a-9e83-4595-a655-3170591e7460" />


ALT TEXT:
Terminal screenshot showing the command sudo ./realtime-monitor.sh being executed, with messages “[+] Starting Failed Login Monitor…” and “[+] Watching /var/log/auth.log”.

2 Log File Missing Error (Before Fix)
<img width="1592" height="297" alt="log-not-found" src="https://github.com/user-attachments/assets/f712ed03-3d18-43b7-bb98-b6c203b5c02d" />


ALT TEXT:
Terminal screenshot displaying the error “tail: cannot open '/var/log/auth.log' for reading: No such file or directory”, indicating missing authentication logs.

3 Installing and Enabling rsyslog
<img width="1920" height="772" alt="rsyslog-install png" src="https://github.com/user-attachments/assets/6eb09846-cd19-4930-8c13-f060c86d2116" />


ALT TEXT:
Terminal output showing rsyslog installation and setup steps, resolving the missing auth.log issue.


4 Successful Real-Time Alerts (Final Output)
<img width="1178" height="807" alt="alerts-working png" src="https://github.com/user-attachments/assets/55037710-0547-4b7e-9182-6081cc4dd376" />


ALT TEXT:
Terminal screenshot showing multiple failed login alerts detected by the script, each including username ::1, IP 57652, and timestamps.

How the Script Works
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

Installation
Clone the repository
git clone https://github.com/alexojocyber/Realtime-Failed-Login-Monitor.git
cd Realtime-Failed-Login-Monitor

Make the script executable
chmod +x realtime-monitor.sh

Run it with sudo
sudo ./realtime-monitor.sh

Troubleshooting
auth.log missing?

Run this:

sudo apt install rsyslog -y
sudo systemctl enable --now rsyslog


Then check again:

ls /var/log/auth.log

sudo password not working?

Remember: Kali Linux does NOT show password characters while typing.
Just type your password and press Enter.

 What I Learned

✔ How Linux logs authentication events
✔ How to parse logs using Bash (awk, grep, cut)
✔ How rsyslog manages and stores system logs
✔ Understanding of failed login patterns
✔ Real automation used in security monitoring

This was my first real-time monitoring tool — and a major milestone in my cybersecurity automation journey.

Future Improvements

Add email/SMS alerts on failed attempts

Detect brute-force thresholds (5+ attempts = HIGH ALERT)

Log alerts to a separate JSON or CSV file

Integrate with a dashboard (Grafana/ELK Stack)

Convert into a Python-based monitoring agent

Show Support

If you find this project useful, please star the repo ⭐ — it helps visibility and supports my journey toward cybersecurity automation and SOC analysis.

Connect With Me

LinkedIn: https://linkedin.com/in/alexojocyber

GitHub: https://github.com/alexojocyber
