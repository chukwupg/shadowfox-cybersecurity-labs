# Task 3: Traffic Analysis (Credential Interception)

## Objective

Capture and analyze network traffic generated during authentication to determine whether user credentials are transmitted securely or exposed in plaintext.

---

# Target Information

* **Target URL:** http://testasp.vulnweb.com/
* **Endpoint:** `/Login.asp`
* **Protocol:** HTTP
* **Port:** 80

---

# Tools Used

* Kali Linux
* Wireshark (Network Protocol Analyzer)

---

# Methodology

## Step 1: Start Packet Capture

Wireshark was launched and the active network interface (`eth0`) was selected to begin capturing live network traffic.

---

## Step 2: Apply Traffic Filter

To focus only on relevant traffic, I applied the display filter:

```
http
```

This ensured that only HTTP packets were visible during analysis.

---

## Step 3: Perform Authentication

Initiated a login request via the web application:

- URL: http://testasp.vulnweb.com/Login.asp
- Test Credentials Used:

  - **Username:** admin
  - **Password:** admin

---

## Step 4: Stop Capture

Stopped the packet capture immediately after the login request was completed to isolate relevant traffic.

---

## Step 5: Identify Login Request

Captured packets were filtered further to locate HTTP POST requests:

```plaintext
http.request.method == "POST"
```

The following request was identified:

```
POST /Login.asp?RetURL=%2FDefault%2Easp%3F HTTP/1.1
```

---

# Packet Analysis

## HTTP Request Details

- **Request Method:** POST
- **Request URI:** `/Login.asp`
- **Protocol Version:** HTTP/1.1
- **Content-Type:** `application/x-www-form-urlencoded`

---

## Extracted Credentials

Within the packet payload, the following form data was identified:

```text
tfUName=admin
tfUPass=admin
```

This confirms that user credentials were transmitted in plaintext within the HTTP request body.

---

## Additional Observations

- The request includes standard HTTP headers such as:

  - `User-Agent`
  - `Referer`
  - `Cookie (ASPSESSIONID)`
- Session handling is implemented using ASP session cookies.
- No encryption or obfuscation mechanisms were applied to sensitive data.

---

# Security Analysis

The analysis demonstrates that authentication credentials are transmitted over an unencrypted HTTP connection.

### Key Issues Identified

- Credentials are sent in **plaintext**
- No HTTPS/TLS encryption is used
- Sensitive data is visible to any network observer

### Security Implications

An attacker positioned on the same network (e.g., via:

- Man-in-the-Middle (MITM)
- Rogue access point
- Packet sniffing)

can easily intercept login credentials without needing to exploit the application itself.

This represents a **critical security weakness** in real-world applications.

---

# Evidence

**Wireshark capture started** 

![Wireshark Running](/assets/screenshots/01-beginner/task-3-traffic-analysis/wireshark-running.png)

**Wiresharp HTTP filter applied**

![HTTP Filter Applied](/assets/screenshots/01-beginner/task-3-traffic-analysis/applied-http-filter.png)

**HTTP traffic**

![HTTP Traffic](/assets/screenshots/01-beginner/task-3-traffic-analysis/http-traffic.png)

**Identified POST requests (`/Login.asp`)**

![HTTP Post Requests](/assets/screenshots/01-beginner/task-3-traffic-analysis/http-post-request.png)

**Expanded packet showing extracted credentials (`tfUName` and `tfUPass`)**

![Extracted Credentials](/assets/screenshots/01-beginner/task-3-traffic-analysis/credential-extraction.png)

**HTTP Stream**

![HTTP Stream](/assets/screenshots/01-beginner/task-3-traffic-analysis/http-stream.png)

---

# Wireshark Filter Log

```
http
http.request.method == "POST"
```

---

# Conclusion

By analysing the traffic, I observed that login credentials are transmitted in plaintext over HTTP. This insecure practice exposes users to credential theft through simple network interception techniques.

The findings highlight the importance of implementing HTTPS to ensure confidentiality and integrity of sensitive data during transmission.

---

# Next Step

Proceed to **Intermediate Level Tasks**