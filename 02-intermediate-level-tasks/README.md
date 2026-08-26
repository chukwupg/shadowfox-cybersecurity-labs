# Intermediate Level

## Phase Overview

The Intermediate level tasks of the Internship builds on the foundational skills developed during the Beginner phase by introducing **password recovery, executable analysis, post-exploitation, and wireless security assessment**.

The tasks were performed in controlled lab environments with the objective of developing practical cybersecurity skills while documenting the methodology, commands, evidence, findings, and security implications of each exercise.

---

## Documentation Structure

```
02-intermediate/
│
├── task-1-veracrypt-password-recovery.md
├── task-2-pe-analysis.md
├── task-3-metasploit-reverse-shell.md
└── task-4-wifi-security-assessment.md
```
---

## Tasks Overview

### Task 1: VeraCrypt Password Recovery

**Objective:**  
Recover the password required to access a VeraCrypt-encrypted container, use the recovered password to unlock the container, and retrieve the secret code stored inside.

**Key Activities:**

- Identified the supplied hash as an MD5 hash
- Used John the Ripper with the RockYou wordlist
- Recovered the password from the supplied hash
- Used VeraCrypt on Windows to mount the encrypted container
- Retrieved the secret code from the mounted volume
- Documented the security implications of weak passwords and legacy hashing algorithms

📄 **Documentation:** [`task-1-veracrypt-password-recovery.md`](./task-1-veracrypt-password-recovery.md)

---

### Task 2: PE Analysis

**Objective:**  
Analyze the provided VeraCrypt executable using PE Explorer and identify its **Address of Entry Point**.

**Key Activities:**

- Loaded the provided VeraCrypt executable into PE Explorer
- Examined the PE header information
- Located the `Address of Entry Point` field
- Documented the entry point value
- Reviewed the relevance of entry points to reverse engineering and malware analysis

📄 **Documentation:** [`task-2-pe-analysis.md`](./task-2-pe-analysis.md)

---

### Task 3: Metasploit Reverse Shell

**Objective:**  
Create a payload using Metasploit and establish a reverse shell connection from a Windows 10 virtual machine to the Kali Linux attacker machine.

**Key Activities:**

- Identified the Kali Linux attacker IP address
- Generated a Windows Meterpreter reverse TCP payload using `msfvenom`
- Transferred the payload to the Windows 10 VM using a Python HTTP server
- Configured a Metasploit multi/handler
- Executed the payload in the controlled Windows environment
- Successfully established a Meterpreter session
- Documented the security implications of reverse-shell payloads and endpoint compromise

📄 **Documentation:** [`task-3-metasploit-reverse-shell.md`](./task-3-metasploit-reverse-shell.md)

---

### Task 4: Wi-Fi Security Assessment

**Objective:**  
Perform a controlled Wi-Fi security assessment involving wireless reconnaissance, WPA/WPA2 handshake capture, password recovery, and defensive analysis.

> ⚠️ **Educational Use:** This assessment was conducted against the author's own router and authorized device within a controlled lab environment. The documented techniques are intended for authorized cybersecurity education and security testing only.

**Key Activities:**

- Identified the wireless interface
- Killed conflicting network management processes
- Enabled monitor mode
- Discovered wireless networks
- Identified the authorized access point and client
- Captured WPA/WPA2 authentication traffic
- Sent a de-authentication packet to the client
- Verified the captured handshake
- Created a targeted wordlist
- Performed offline password recovery
- Analyzed wireless security weaknesses and recommended defensive controls

📄 **Documentation:** [`task-4-wifi-security-assessment.md`](./task-4-wifi-security-assessment.md)

---

## Learning Insight

The Intermediate phase provided practical exposure to several areas of cybersecurity:

- **Password Security:** Hash identification, dictionary-based password recovery, and the security implications of weak credentials
- **Cryptography:** Understanding the relationship between password hashes and encrypted data
- **PE Analysis:** Examining Windows executable structure and identifying the entry point
- **Reverse Engineering Fundamentals:** Understanding the significance of PE headers and execution flow
- **Post-Exploitation:** Understanding payload generation, reverse connections, and Meterpreter sessions
- **Network Security:** Wireless reconnaissance, WPA/WPA2 authentication, handshake analysis, and offline password testing
- **Security Assessment:** Translating technical observations into security findings and recommendations
- **Evidence & Reporting:** Documenting commands, results, screenshots, findings, and security implications in a reproducible format

---

## Phase Summary

| Task | Area | Primary Tool(s) | Status |
|---|---|---|---|
| Task 1 | VeraCrypt Password Recovery | John the Ripper, VeraCrypt | ✅ Completed |
| Task 2 | PE Analysis | PE Explorer | ✅ Completed |
| Task 3 | Reverse Shell | Metasploit, msfvenom | ✅ Completed |
| Task 4 | Wi-Fi Security Assessment | Aircrack-ng suite | ✅ Completed |

---

## Next 

Proceed to the **Advanced Level Task**, where a I will be carrying out a structured penetration test.

---