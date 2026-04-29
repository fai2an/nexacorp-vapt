# nexacorp-vapt
Full VAPT engagement on an internal network lab using Nmap &amp; OpenVAS. Covers host discovery, port scanning, service enumeration, OS fingerprinting, and vulnerability detection. 10 findings identified including 4 Critical CVEs. Includes a professional penetration testing report.

# Network Vulnerability Assessment & Penetration Testing (VAPT)
### NexaCorp Financial Services — Internal Network Security Assessment


## Project Overview

This project demonstrates a full **Vulnerability Assessment and Penetration Testing (VAPT)** engagement conducted against a simulated corporate internal network. The assessment follows industry-standard methodology used by professional penetration testers and security consultants.

The project covers the complete lifecycle of a real-world VAPT engagement:
- Lab environment setup
- Network reconnaissance
- Port scanning and service enumeration
- OS fingerprinting
- Automated vulnerability detection
- Risk analysis and professional report writing


##  Scenario

**Client:** NexaCorp Financial Services (simulated)  
**Engagement Type:** Internal Network Penetration Test  
**Objective:** Identify and assess vulnerabilities on an internal server before an upcoming security audit  
**Target:** Metasploitable2 (intentionally vulnerable VM)  
**Attacker Machine:** Kali Linux 2025.3  


##  Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Nmap** | 7.99 | Port scanning, service detection, OS fingerprinting, NSE vuln scripts |
| **OpenVAS / GVM** | 25.04.0 | Vulnerability assessment with 95,000+ NVT checks |
| **Kali Linux** | 2025.3 | Penetration testing operating system |
| **VMware Workstation** | Latest | Lab virtualization platform |
| **Metasploitable2** | 2.0 | Intentionally vulnerable target VM |


##  Methodology

This assessment followed the **PTES (Penetration Testing Execution Standard)** and **OWASP Testing Guide**.

```
Phase 1 → Reconnaissance      (Host Discovery)
Phase 2 → Enumeration         (Port + Service Scanning)
Phase 3 → OS Fingerprinting   (OS Detection)
Phase 4 → Vulnerability Scan  (NSE Scripts + Aggressive Scan)
Phase 5 → Analysis & Report   (CVE Correlation + Risk Rating)
```

---

##  Lab Setup

```
┌─────────────────────┐         ┌──────────────────────┐
│   Kali Linux 2025.3 │───────▶ │   Metasploitable2    │
│   192.168.157.128   │  scan   │   192.168.157.131    │
│   (Attacker)        │         │   (Target)           │
└─────────────────────┘         └──────────────────────┘
         Both on VMware VMnet1 (NAT) — Isolated Lab Network
```

---

##  Nmap Commands Used

### 1. Host Discovery
```bash
nmap -sn 192.168.157.131
```

### 2. Full Port Scan + Service Detection
```bash
nmap -sV -p- 192.168.157.131 -oN nmap/full_scan.txt
```

### 3. OS Detection
```bash
nmap -O -sV 192.168.157.131 -oN nmap/os_scan.txt
```

### 4. Vulnerability Scripts
```bash
nmap --script vuln 192.168.157.131 -oN nmap/vuln_scan.txt
```

### 5. Aggressive Scan
```bash
nmap -A 192.168.157.131 -oN nmap/aggressive_scan.txt
```

---

##  Key Findings Summary

| # | Vulnerability | CVE | CVSS | Risk |
|---|--------------|-----|------|------|
| 1 | vsftpd 2.3.4 Backdoor RCE | CVE-2011-2523 | 10.0 | 🔴 CRITICAL |
| 2 | Samba 3.0.20 Username Map Script RCE | CVE-2007-2447 | 10.0 | 🔴 CRITICAL |
| 3 | UnrealIRCd 3.2.8.1 Backdoor RCE | CVE-2010-2075 | 10.0 | 🔴 CRITICAL |
| 4 | Bindshell Root Backdoor (Port 1524) | N/A | 10.0 | 🔴 CRITICAL |
| 5 | Telnet Cleartext Authentication | CVE-1999-0619 | 7.5 | 🟠 HIGH |
| 6 | Apache 2.2.8 + Exposed Web Apps | CVE-2009-1195 | 7.8 | 🟠 HIGH |
| 7 | MySQL 5.0.51a End-of-Life RCE | CVE-2009-4484 | 7.2 | 🟠 HIGH |
| 8 | Java RMI Registry RCE | CVE-2011-3556 | 6.8 | 🟡 MEDIUM |
| 9 | VNC Weak Authentication (v3.3) | CVE-2006-2369 | 6.4 | 🟡 MEDIUM |
| 10 | SMTP Open Relay | N/A | 4.3 | 🔵 LOW |

**Total: 4 Critical | 3 High | 2 Medium | 1 Low**

---

##  Repository Structure

```
nexacorp-vapt/
├── README.md
├── nmap/
│   ├── full_scan.txt
│   ├── os_scan.txt
│   ├── vuln_scan.txt
│   └── aggressive_scan.txt
├── report/
│   └── NexaCorp_VAPT_Report.pdf
├── screenshots/
└── methodology.md
```

---

## Full Report

The complete professional VAPT report is available in the `/report` folder. It includes:

- Executive Summary with Risk Overview
- Scope & Objectives
- Detailed Methodology
- 10 Vulnerability Findings (each with CVE, CVSS, Evidence, Impact, Remediation)
- Risk Summary Matrix
- Prioritized Remediation Roadmap
- Conclusion & Recommendations

---

##  What I Learned

- Setting up an isolated penetration testing lab using VMware
- Using Nmap for comprehensive network reconnaissance
- Interpreting service version information to identify vulnerable software
- Correlating scan results with CVE databases and CVSS scoring
- Writing professional VAPT reports following industry standards
- Understanding real-world vulnerabilities like backdoors, cleartext protocols, and outdated software risks

---

##  Disclaimer

> **This project was conducted entirely in a controlled, isolated lab environment against intentionally vulnerable virtual machines (Metasploitable2) for educational and portfolio purposes only.**
>
> All IP addresses used are private lab addresses. No real systems, networks, or organizations were targeted. Performing penetration testing against systems without explicit written authorization is illegal.
>
> This project is for **educational purposes only.**



[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://linkedin.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black)](https://github.com)
