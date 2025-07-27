# 🔍 NMAP Network Scanning Project

## 🧠 Project Goals

This project demonstrates core competencies in network reconnaissance using **NMAP**, a powerful and widely-used network scanning tool. The goal is to:

* Identify live hosts, open ports, services, OS information, and potential vulnerabilities
* Practice ethical, permission-based scanning
* Produce professional reports with actionable insights
* Showcase real-world cybersecurity investigation skills

---

## 🛠️ Methodology

Each phase of the project covers a specific NMAP scanning technique. All scans were performed on a private, controlled lab environment.

> ⚠️ **Ethical Disclaimer**: All scans were conducted on networks and systems that I own or have explicit permission to test. Unauthorized scanning is illegal and unethical.

---

### 1. 📡 Basic Network Discovery

```
nmap -sn 192.168.233.0/24
```

* **Purpose**: Discover all live hosts within the local subnet.
* **Result**: Returned list of active IP addresses and associated MAC addresses.

*Sample Output Screenshot:*
[Network Discovery](Screenshots/Network_Discovery.png)

---

### 2. 🔍 Port Scanning (SYN Scan)

```
nmap -sS -T4 192.168.233.137
```

* **Purpose**: Detect open TCP ports on 192.168.233.137
* **Scan Type**: Stealth SYN scan with aggressive timing (`-T4`)
* **Result**: Identified services like SSH (22), HTTP (80), and HTTPS (443)

*Sample Output Screenshot:*
[Port Scanning](Screenshots/Port_Scanning.png)

---

### 3. 🧬 Service and OS Detection

```
nmap -sV -O 192.168.233.137
```

* **Purpose**: Determine running service versions and OS fingerprint
* **Result**: Services with versions (e.g., Apache 2.2.8), OS guess (e.g., Linux 2.6.X)

*Security Insight*: Older versions of services might be vulnerable to known CVEs.

*Sample Output Screenshot:*
[Service & OS Detection](Screenshots/Service_and_OS_Detection.png)

---

### 4. 🚨 Vulnerability Scanning

```
nmap --script vuln 192.168.233.137
```

* **Purpose**: Run vulnerability NSE scripts against target host
* **Result**: Identified potential CVEs tied to outdated services

*Security Insight*: Detected vulnerabilities included possible CVEs related to outdated SSL versions.

*Sample Output Screenshot:*
[Vulnerability Scanning](Screenshots/Vulnerability_Scanning.png)

---

### 5. ⚔️ Advanced Scanning Techniques (Aggressive Scan)

```
nmap -A -T4 192.168.233.137
```

* **Purpose**: Perform a full aggressive scan combining:

  * OS detection
  * Version detection
  * Script scanning
  * Traceroute
* **Result**: Comprehensive fingerprint of the target

*Security Insight*: This scan offers a full attacker-view of the system; very useful for red-teaming.

*Sample Output Screenshot:*
[Advanced Scanning Techniques](Screenshots/Advanced_Scanning_Techniques.png)

---

## 6. ⏰ Automated Scanning with Cron (New Feature)

Automate daily scans for continuous monitoring of services and system drift.
Script

     #!/bin/bash
     TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
     OUTPUT_DIR="/home/user/nmap-logs"
     mkdir -p $OUTPUT_DIR
     nmap -sV -O 192.168.233.137 -oN $OUTPUT_DIR/scan_$TIMESTAMP.txt

Cron Job Setup

    crontab -e

Then add:

    0 2 * * * /home/user/scripts/daily_nmap_scan.sh

🔄 Runs every day at 2 AM

Benefits:
- Continuous visibility of the target system
- Detects unexpected changes
- Supports historical change tracking

---

🛰️ Live HTML Report Generation (New Feature)

Automatically convert Nmap scan results into a professional HTML report using xsltproc.

    nmap -sV -O -oX scan.xml 192.168.233.137
    xsltproc scan.xml -o scan_report.html

🧩 Benefits:
- Human-readable web format
- Easier to present in reports or security documentation
- Enables browser-based post-analysis
- Looks more professional for client delivery or team reviews

*Sample Output Screenshot:*
- [HTML Report Generation](screenshots/xml.png)
- [HTML Report Result](<screenshots/html.png>
)

---

## 📊 Findings Summary

| IP Address      | Service      | Detected Version | Vulnerability (CVE) | Description                              |
| --------------- | ------------ | ---------------- | ------------------- | ---------------------------------------- |
| 192.168.233.137 | vsFTPd       | 2.3.4            | CVE-2011-2523       | Backdoor                                 |
| 192.168.233.137 | SSL          | 3.0              | CVE-2014-2455       | Information Leak                         |


---

## 🔐 Security Implications

* Unnecessary open ports increase attack surface
* Service version exposure aids attackers in fingerprinting
* Outdated services can be targeted with known exploits
* Lack of segmentation could allow lateral movement

---

## 🧾 Conclusion

Through this project, I demonstrated key scanning techniques using NMAP. Each stage provided practical insights into host discovery, service enumeration, and vulnerability detection. The hands-on experience enhanced my understanding of how attackers probe networks—and how defenders can detect and mitigate exposure.

---




