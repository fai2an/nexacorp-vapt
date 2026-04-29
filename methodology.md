# Penetration Testing Methodology
## NexaCorp Financial Services — Internal Network VAPT

---

## Overview

This document outlines the step-by-step methodology followed
during the NexaCorp Financial Services VAPT engagement.
The methodology is aligned with industry standards including
PTES (Penetration Testing Execution Standard) and the
OWASP Testing Guide.

---

## Phase 1 — Environment Setup

### Objective
Set up an isolated lab environment for safe and legal testing.

### Steps
- Installed Kali Linux 2025.3 on VMware Workstation
- Downloaded and configured Metasploitable2 as the target VM
- Set both VMs to VMnet1 (NAT) for isolated network communication
- Verified connectivity between attacker and target using ping
- Created project folder structure:

```
nexacorp-vapt/
├── nmap/
├── openvas/
├── evidence/
└── report/
```

### Result
- Kali Linux IP: 192.168.157.128
- Metasploitable2 IP: 192.168.157.131
- Ping successful — 0% packet loss

---

## Phase 2 — Reconnaissance (Host Discovery)

### Objective
Confirm the target host is alive and reachable.

### Tool
Nmap 7.99

### Command
```bash
nmap -sn 192.168.157.131
```

### Flags Explained
| Flag | Meaning |
|------|---------|
| -sn  | Ping scan only — no port scanning |

### Result
- Host is UP
- Latency: 0.00051s
- MAC Address: 00:0C:29:FA:DD:34 (VMware)

---

## Phase 3 — Enumeration (Port & Service Scanning)

### Objective
Discover all open ports and identify running services
and their versions.

### Command
```bash
nmap -sV -p- 192.168.157.131 -oN nmap/full_scan.txt
```

### Flags Explained
| Flag | Meaning |
|------|---------|
| -sV  | Service version detection |
| -p-  | Scan all 65,535 TCP ports |
| -oN  | Save output to normal text file |

### Result
- 30+ open ports discovered
- Critical services identified:
  vsftpd 2.3.4, Samba 3.0.20, UnrealIRCd,
  Apache 2.2.8, MySQL 5.0.51a
- Scan completed in 135.11 seconds

---

## Phase 4 — OS Fingerprinting

### Objective
Identify the operating system and kernel version
of the target.

### Command
```bash
nmap -O -sV 192.168.157.131 -oN nmap/os_scan.txt
```

### Flags Explained
| Flag | Meaning |
|------|---------|
| -O   | Enable OS detection |
| -sV  | Service version detection |
| -oN  | Save output to file |

### Result
- OS: Linux 2.6.9 - 2.6.33
- Device Type: General Purpose
- Network Distance: 1 hop

---

## Phase 5 — Vulnerability Scanning

### Objective
Automatically detect known vulnerabilities using
Nmap NSE (Nmap Scripting Engine) vulnerability scripts.

### Command
```bash
nmap --script vuln 192.168.157.131 -oN nmap/vuln_scan.txt
```

### Flags Explained
| Flag          | Meaning |
|---------------|---------|
| --script vuln | Run all vulnerability detection NSE scripts |
| -oN           | Save output to file |

### Key Findings
- vsftpd 2.3.4 backdoor — CVE-2011-2523 (EXPLOITABLE)
- RMI registry RCE — State: VULNERABLE
- MySQL authentication bypass — CVE-2012-2122
- phpMyAdmin exposed — unauthorized DB access risk
- DVWA login page exposed — vulnerable web app
- phpinfo.php exposed — server information leak
- Directory listing enabled on multiple paths
- JSESSIONID httponly flag not set

### Scan Duration
346.68 seconds

---

## Phase 6 — Aggressive Scanning

### Objective
Perform comprehensive scanning combining OS detection,
version detection, script scanning and traceroute
in a single command.

### Command
```bash
nmap -A 192.168.157.131 -oN nmap/aggressive_scan.txt
```

### Flags Explained
| Flag | Meaning |
|------|---------|
| -A   | Aggressive scan: OS + version + scripts + traceroute |
| -oN  | Save output to file |

### Key Findings
- Samba 3.0.20-Debian confirmed
- SMB guest account enabled (anonymous access)
- SMB message signing disabled (relay attacks possible)
- FQDN: metasploitable.localdomain
- VNC Authentication version 2 on port 5900
- Apache Tomcat 5.5 on port 8180
- Anonymous FTP login allowed
- SSL certificate expired (PostgreSQL)

### Scan Duration
62.69 seconds

---

## Phase 7 — OpenVAS Configuration

### Objective
Set up OpenVAS/GVM for deep vulnerability assessment
with CVE database correlation and CVSS scoring.

### Steps Completed
- Installed OpenVAS/GVM 25.04.0 on Kali Linux
- Upgraded PostgreSQL from version 17 to 18
  (required by libgvmd)
- Ran gvm-setup — downloaded 95,086 NVTs
- Successfully synced all vulnerability feeds:
  - Notus files
  - NASL files
  - SCAP data
  - CERT-Bund data
  - gvmd data
- Created scan target: NexaCorp-Metasploitable
  (192.168.157.131)
- Logged into OpenVAS web interface
  at https://127.0.0.1:9392
- GVM installation verified with gvm-check-setup

### Note
Scan config population encountered a feed sync delay
during the engagement. All vulnerability findings were
correlated manually using CVE databases and Nmap NSE
script results, which confirmed exploitable
vulnerabilities with the same accuracy.

---

## Phase 8 — Analysis & Risk Rating

### Objective
Correlate scan findings with CVE databases,
assign CVSS scores, and rate risk levels.

### CVSS Score Reference
| Score Range | Risk Level |
|-------------|------------|
| 9.0 - 10.0  | CRITICAL   |
| 7.0 - 8.9   | HIGH       |
| 4.0 - 6.9   | MEDIUM     |
| 0.1 - 3.9   | LOW        |

### CVE Sources Used
- https://cve.mitre.org
- https://nvd.nist.gov
- https://www.exploit-db.com

---

## Phase 9 — Reporting

### Objective
Produce a professional VAPT report suitable for
client delivery and portfolio presentation.

### Report Contents
- Executive Summary with Risk Overview
- Scope and Objectives
- Detailed Methodology
- 10 Vulnerability Findings with CVE, CVSS,
  Evidence, Impact and Remediation
- Risk Summary Matrix
- Prioritized Remediation Roadmap
- Conclusion and Recommendations

### Report Format
- PDF — professional client-ready format
- Word (.docx) — editable format

---

## Tools & Versions Summary

| Tool               | Version | Purpose                  |
|--------------------|---------|--------------------------|
| Nmap               | 7.99    | Network scanning         |
| OpenVAS / GVM      | 25.04.0 | Vulnerability assessment |
| Kali Linux         | 2025.3  | Attacker platform        |
| Metasploitable2    | 2.0     | Vulnerable target VM     |
| VMware Workstation | Latest  | Lab virtualization       |

---

*All testing was conducted in a controlled lab
environment with proper authorization.
For educational purposes only.*
