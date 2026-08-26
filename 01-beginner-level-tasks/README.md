# Beginner Level: Web Reconnaissance & Traffic Analysis

The beginner level tasks of the internship focuses on foundational web application security techniques, covering reconnaissance, enumeration, and basic traffic analysis. The objective is to understand how exposed services and insecure communication channels can lead to real security risks.

---

## Target

- **Web Application:** http://testasp.vulnweb.com/
- **IP Address:** 44.238.29.244
- **Environment:** Intentionally vulnerable web application (for security testing and training)

---

## Documentation Structure

```
01-beginner/
│
├── task-1-port-scan.md
├── task-2-directory-bruteforcing.md
└── task-3-traffic-analysis.md
```

---

## Tasks Overview

### Task 1: Port Scanning

**Objective:** Identify open ports and exposed services on the target system.

- Performed full TCP port scan using Nmap
- Identified active services and server technologies
- Detected HTTP service running on Microsoft IIS

📄 **Documentation:** [`task-1-port-scan.md`](./task-1-port-scan.md)

---

### Task 2: Directory Bruteforcing

**Objective:** Discover hidden directories and files to expand the attack surface.

- Conducted directory enumeration using Gobuster
- Identified multiple ASP endpoints and application directories
- Discovered authentication pages, backend-related files, and configuration files.

📄 **Documentation:** [`task-2-directory-bruteforcing.md`](./task-2-directory-bruteforcing.md)

---

### Task 3: Traffic Analysis (Credential Interception)

**Objective:** Analyze network traffic to determine how credentials are transmitted.

- Captured HTTP traffic using Wireshark
- Identified login POST request
- Extracted credentials transmitted in plaintext

📄 **Documentation:** [`task-3-traffic-analysis.md`](./task-3-traffic-analysis.md)

---

## Hands-on Practice & Applied Knowledge

- **Attack Surface Mapping:**
  Understanding how open ports and directories expose entry points

- **Web Enumeration:**
  Identifying hidden endpoints and application structure

- **Network Traffic Analysis:**
  Observing how data flows across the network

- **Security Misconfiguration Insight:**
  Recognizing the risks of transmitting sensitive data over HTTP

---

## Security Insight

Even without advanced exploitation techniques, significant vulnerabilities can be identified through:

- Misconfigured services
- Exposed application endpoints
- Unencrypted communication channels

This phase demonstrates that **basic reconnaissance and analysis alone can reveal critical security weaknesses**.

---

## Next

**Intermediate Level Tasks**, which introduces:

- Password cracking
- Binary analysis or Reverse Engineering
- Exploitation techniques
- Wireless security attacks

---
