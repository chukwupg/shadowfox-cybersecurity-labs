# Task 1: Port Scanning

## 📌 Objective

Identify all open TCP ports and exposed services on the target web application.

---

# Target Information

* **Target URL:** http://testasp.vulnweb.com/
* **Resolved IP Address:** 44.238.29.244

---

# Tools Used

* Kali Linux
* Ping
* Nmap 7.99

---

# Methodology

## Step 1: Verify Target Reachability

Before initiating any scans, the target hostname was tested for network reachability. This step also confirmed successful DNS resolution and identified the IP address associated with the target.

### Command

```bash
ping -c 4 testasp.vulnweb.com
```

### Result

* DNS successfully resolved the hostname.
* Target IP identified as **44.238.29.244**.
* All four ICMP echo requests received replies.
* 0% packet loss observed.
* Average round-trip time: approximately **274 ms**.

### Screenshot

**Successful ping showing hostname resolution, IP address, and ICMP replies.**

![ping result](/assets/screenshots/01-beginner/task-1-port-scan/ping-result.png)

---

## Step 2: Perform a Full TCP Port Scan

After confirming the host was reachable, a full TCP scan was performed to identify all exposed ports.

### Command

```bash
nmap -p- 44.238.29.244
```

### Result

The scan identified two open TCP ports while the remaining ports were filtered.

| Port | State | Service           |
| ---- | ----- | ----------------- |
| 80   | Open  | HTTP              |
| 587  | Open  | Submission (SMTP) |

### Screenshot

**Full TCP port scan results**

![Full TCP port scan result](/assets/screenshots/01-beginner/task-1-port-scan/full-tcp-port-scan-result.png)

---

## Step 3: Enumerate Services and Versions

A follow-up scan was performed against the discovered ports to identify running services, software versions, and execute Nmap's default NSE scripts.

### Command

```bash
sudo nmap -sV -sC -p 80,587 44.238.29.244
```

---

### Scan Results

---

#### Port 80/TCP

**State:** Open

**Service:** HTTP

**Server:**

* Microsoft IIS 8.5

#### Additional Findings

* HTTP Title:

  * IIS Windows Server
* Server Header:

  * Microsoft-IIS/8.5
* Supported HTTP Methods:

  * TRACE method enabled (reported by the Nmap HTTP Methods script as potentially risky)

---

#### Port 587/TCP

**State:** Open

**Service:**

* Submission (SMTP)

#### Additional Findings

Nmap detected the port as open but was unable to establish an SMTP session during service detection.

```
Couldn't establish connection on port 587
```

---

### Screenshot 

**Service and Version Detection**

![Service and Version Enumeration](/assets/screenshots/01-beginner/task-1-port-scan/service-version-detection.png)

# Summary of Findings

| Port | Service         | Version           | Notes                                     |
| ---- | --------------- | ----------------- | ----------------------------------------- |
| 80   | HTTP            | Microsoft IIS 8.5 | TRACE method enabled                      |
| 587  | SMTP Submission | Unknown           | Service did not complete SMTP negotiation |

---

# Security Analysis

The assessment identified two exposed TCP ports.

The primary attack surface is the HTTP service running on Microsoft IIS 8.5. The default Nmap scripts also reported that the HTTP **TRACE** method is enabled. While this does not necessarily constitute a vulnerability by itself, the TRACE method is commonly considered unnecessary in production environments and may contribute to certain web attacks, such as Cross-Site Tracing (XST), if combined with other weaknesses.

Port 587, commonly used for secure SMTP message submission, was also found to be open. However, the service did not complete an SMTP handshake during enumeration, preventing further identification at this stage.

These findings provide a foundation for the next phase of the assessment, where the exposed web service will be enumerated further through directory discovery.

---

# Evidence

**Nmap scan results**

![Nmap scan result](/assets/screenshots/01-beginner/task-1-port-scan/nmap-scan-result.png)

---

# Conclusion

The reconnaissance phase successfully confirmed network connectivity to the target, identified its resolved IP address, discovered the exposed TCP ports, and enumerated the primary services running on those ports. The information gathered establishes the initial attack surface and serves as the basis for the next task: **Directory Bruteforcing**, where hidden files and directories within the web application will be identified.

## Next Step

**Task 2: Directory Bruteforcing**, to uncover hidden endpoints and expand the attack surface.