# Task 4: Wi-Fi Security Assessment

## ⚠️ Educational & Authorized Use Disclaimer

> **This exercise was conducted strictly for educational and cybersecurity training purposes within a controlled laboratory environment. The Wi-Fi auditing activities were performed against the author's own router and an authorized device.**
>
> The techniques, commands, and procedures documented in this report are provided for **educational and defensive security research purposes only**. They should only be performed against networks, devices, and systems for which explicit authorization has been obtained.
>
> Unauthorized deauthentication, wireless traffic capture, or password recovery against networks or devices without permission may disrupt services, violate privacy, and be illegal.

---

## Objective

Perform a controlled Wi-Fi security assessment by:

- Auditing a wireless network in a controlled environment
- Identifying the wireless access point and connected client
- Capturing a WPA/WPA2 authentication handshake
- Creating a targeted password wordlist for the authorized lab network
- Performing offline password recovery against the captured authentication data
- Documenting the results and security implications

---

# Task Overview

This assessment was performed against a personally controlled wireless network using an authorized client device.

The workflow consisted of:

1. Preparing the wireless interface
2. Identifying nearby wireless networks
3. Identifying the target access point and client
4. Capturing wireless authentication traffic
5. Obtaining a WPA handshake
6. Preparing a targeted wordlist
7. Performing offline password recovery
8. Verifying the recovered password against the authorized network

These entire workflow was conducted within my personal controlled lab environment.

---

# Tools & Environment

### Attacker / Audit Machine

- Kali Linux
- Aircrack-ng suite
- Wireless adapter supporting monitor mode (Alfa AWUS036ACH)

### Target Environment

- Personal Wi-Fi router
- Authorized client device
- WPA/WPA2-protected wireless network

---

# Lab Scope

| Component | Description |
|---|---|
| Wireless Network | Personal/authorized network |
| Access Point | Own router |
| Client | Authorized personal device |
| Audit Machine | Kali Linux |
| Purpose | Educational wireless security assessment |

No third-party networks or unauthorized devices were intentionally targeted.

---

# Step 1: Identify the Wireless Interface

Identify available wireless interfaces.

### Command

```
iwconfig
```

This command displays wireless interfaces available to the Linux system.

### Result

- The wireless adapter used for the assessment (wlan0) was identified and selected for the subsequent wireless auditing activities.

# Evidence

**Output of `iwconfig` showing the wireless interface**

![Output of `iwconfig`](/assets/screenshots/02-intermediate/task-4/check-for-wireless-card.png)

---

# Step 2: Kill Conflicting Network Management Processes

Some network management processes may conflict with the process and prevent it from working as expected. 

To prevent that, you may have to check and kill the process.

### Command

```
sudo airmon-ng check kill
```

### Result

- Found the process wpa_supplicant
- Killed the process wpa_supplicant with pid 16446

### Evidence

**Kill conflicting processes**

![Kill conflicting processes](/assets/screenshots/02-intermediate/task-4/kill-conflicting-processes.png)

---

# Step 3: Enable Monitor Mode

Place the appropriate wireless adapter into monitor mode to allow the capture and analysis of 802.11 wireless traffic.

### Command

```
sudo airmon-ng start <interface>
```

Replace `interface` with the wireless interface identified in the previous step.

### Result

- The wireless adapter was successfully placed into monitor mode.

- The resulting monitor-mode interface was used for wireless traffic capture.

### Evidence

**Terminal output showing the wireless interface in monitor mode**

![Monitor mode](/assets/screenshots/02-intermediate/task-4/enable-monitor%20mode.png)

---

# Step 4: Discover Wireless Networks

Scan the wireless environment to identify the authorized access point.

### Command

```
sudo airodump-ng --band abg <monitor-interface>
```

Without `--band abg` the command would still run; it only ensures that the network adapter scans all the wireless protocols. By default airodump only scans in the 2.4 GHz range

Replace `monitor-interface` with the wireless interface with monitore mode enabled. You can find it under 'interface' column in the output of the previous command.

This displays nearby wireless networks and relevant information such as:

- BSSID
- Channel
- Encryption type
- ESSID
- Signal information
- Associated clients

### Result

- The authorized wireless network was identified from the scan results.
- The relevant network information was recorded for the controlled assessment.
    Relevant information included: 
    - Access Point BSSID
    - Client MAC address
    - Wireless channel
    - Network ESSID

### Evidence

**airodump-ng output showing the authorized access point**

![Authorized access point](/assets/screenshots/02-intermediate/task-4/discover-networks.png)

---

# Step 5: Monitor for WPA Handshake

Monitor the wireless traffic on the target channel to capture the authentication exchange generated when client reconnects to the router.

Send the captured traffic to file.

### Command

```
sudo airodump-ng -c <channel> --bssid <target-bssid> -w <output-filename> <monitor-interface>
```

### Explanation

| Option                | Purpose                                            |
| --------------------- | -------------------------------------------------- |
| `-c`           | Specifies the wireless channel                     |
| `--bssid`             | Restricts monitoring to the specified access point |
| `-w`             | Saves captured traffic to files                    |
| `<monitor-interface>` | Specifies the monitor-mode interface               |

The capture was restricted to the authorized access point and its operating channel.

### Result

Packet capture started

> Note the client mac address listed under the station column in the result, it will be used for the next step

### Evidence

**Monitoring the target wireless traffic**

![Handshake monitor](/assets/screenshots/02-intermediate/task-4/monitor-the-target-network.png)

---

# Step 6: Send a De-authentication Packet

While monitoring the target wireless traffic, open a new tab and send a de-authentication packet to a connected client using it's mac address from the previous step to trigger a reconnection.

### Command

```
sudo aireplay-ng --deauth 1 -a <target-bssid> -c <client-mac-address> <monitor-interface>

And/Or

sudo aireplay-ng --deauth 2 -a <target-bssid> -c <client-mac-address> <monitor-interface>
```

Either of the command should work but if you didn’t get a handshake from sending 1 deauth packet `--deauth 1` then try sending 2 which is the second command with `--deauth 2` 

Using `--deauth -0` will send de-authentication packet continously until you stop it which could cause a DOS attack. 

To send a deauthentication packet to every client connected to the target network, do not include the `-c` option which targets a particular MAC address. 

Note: high number of deauthentication messages are noisy on the network but 1 or 2 can go unnoticed.

### Result

- The disconnected client will attempt to reconnect

### Evidence

**Sending deauthentication packet** 

![Deauthentication](/assets/screenshots/02-intermediate/task-4/send-deauth-packet.png)

---

# Step 7: Verify Captured Authentication Data

Inspect the capture file to verify that the required authentication exchange had been captured.

### Commands

```
ls | grep <search term>
```
To list files in the working directory matching the capture filename

```
aircrack-ng <capture-file>
```
To check the capture file for valid WPA authentication data.

### Result
- The capture file is present is the working directory

- The capture was successfully recognized as containing WPA authentication information suitable for password verification.

### Evidence

**Confirmed handshake has been captured from the monitoring terminal**

![Handshake captured](/assets/screenshots/02-intermediate/task-4/handshake-captured.png)

**Confirmed handshake file exist**

![Handshake file](/assets/screenshots/02-intermediate/task-4/capture-file.png)

**Aircrack-ng identifying the captured WPA authentication data**

![WPA authentication data](/assets/screenshots/02-intermediate/task-4/wpa-authentication-data.png)

---

# Step 8: Disable Monitor Mode

Take the Wifi Adapter out of monitor mode

### Command

```
sudo airmon-ng stop wlan0
```

### Result

- Monitor mode disabled on the wireless adapter

### Evidence

**Wireless adapter monitor mode disabled**

![Disable monitor mode](/assets/screenshots/02-intermediate/task-4/monitor-mode-disabled.png)

---

# Step 9: Create a Targeted Wordlist

Create a custom wordlist for the authorized network.

> The purpose of the wordlist was to provide possible password candidates relevant to the controlled lab environment rather than relying exclusively on a large generic dictionary.

### Command

```
nano wordlist.txt
```

Confirm presence of the targeted wordlist for offline password recovery.

```
ls | grep wordlist
```
### Result

- Targeted wordlist created successfully
- `wordlist.txt` present in the current working directory

### Evidence

**Create targeted wordlist**

![Targeted wordlist](/assets/screenshots/02-intermediate/task-4/create-targeted-wordlist.png)

**Confirm targeted wordlist file exist**

![Wordlists](/assets/screenshots/02-intermediate/task-4/confirm-targeted-wordlist-exist.png)

---

# Step 10: Perform Offline Password Recovery

The captured authentication data was tested against the authorized wordlist.

Command

```
aircrack-ng -w <path-to-wordlist> <capture-file>
```

### Explanation

| Option            | Purpose                                             |
| ----------------- | --------------------------------------------------- |
| `-w` | Specifies the location of the wordlist          |
| `<capture-file>`  | Specifies the captured wireless authentication data (handshake file)|

### Evidence

**Successful password recovery showing the recovered key**

![Password recovery](/assets/screenshots/02-intermediate/task-4/password-recovery.png)

--- 

# Results Summary

| Assessment Stage                   | Result     |
| ---------------------------------- | ---------- |
| Wireless interface identified      | Successful |
| Monitor mode enabled               | Successful |
| Authorized access point identified | Successful |
| Authorized client identified       | Successful |
| Wireless traffic captured          | Successful |
| WPA authentication data captured   | Successful |
| Wordlist                           | Present    |
| Offline password recovery          | Successful |
| Recovered password verified        | Successful |

---

# Technical Analysis

### WPA/WPA2 Authentication

The WPA/WPA2 4-way handshake is used to establish the cryptographic keys required for secure communication between a wireless client and access point.

The wireless password is not transmitted directly as plaintext during the handshake.

Instead, authentication data captured during the handshake can be used to guess the password offline.

### Deauthentication Concept

A deauthentication event causes a wireless client to disconnect from an access point.

In a controlled assessment, this can cause the authorized client to reconnect, generating a new authentication exchange that can potentially be captured.

This security implication stems from the historical lack of cryptographic protection for Wi-Fi management frames introduces critical vulnerabilities, including:

1. Susceptibility to spoofing and forgery, enabling DoS attacks. 
2. Absence of integrity verification, allowing malicious modification of network control messages. 
3. Enabling man-in-the-middle and rogue AP attacks, even on encrypted networks.

Modern wireless security features such as Protected Management Frames (PMF / 802.11w) can help mitigate this class of attack.

---

# Security Findings

## Finding 1: Password Exposure and Strength

The wireless password was recoverable using a targeted wordlist.

This demonstrates that passwords based on predictable or discoverable information can be vulnerable to offline password-guessing attacks.

Risk: Medium

### Recommendation:

Use a long, unique Wi-Fi password
Avoid dictionary words and predictable patterns
Avoid personally identifiable information
Use randomly generated credentials where practical

## Finding 2: WPA/WPA2 Handshake Exposure

Captured authentication data can provide an attacker with material for offline password testing.

Risk: Depends on password strength and wireless security configuration.

### Recommendation:

Use WPA3 where supported
Use a strong, unique wireless password
Keep router firmware updated
Disable obsolete wireless security protocols

## Finding 3: Management Frame Protection

Where supported, Protected Management Frames (PMF / 802.11w) should be enabled.

PMF helps protect certain wireless management traffic from unauthorized spoofing.

### Recommendation:

Configure PMF according to the router's capabilities, preferably using WPA3/PMF where compatible with the environment.

---

# Defensive Recommendations

Based on the findings, the following measures are recommended:

1. Use WPA3 where supported.
2. Use a long, random, unique Wi-Fi password.
3. Enable Protected Management Frames (PMF / 802.11w).
4. Keep router firmware updated.
5. Disable outdated wireless security protocols.
6. Regularly review connected clients.
7. Monitor unexpected authentication and disconnection activity.
8. Avoid reusing wireless passwords across different networks.

---

# Command Log

### Identify wireless interfaces
```
iwconfig
```

### Kill Conflicting Network Management Processes
```
sudo airmon-ng check kill
```

### Enable monitor mode
```
sudo airmon-ng start <interface>
```

### Discover wireless networks
```
sudo airodump-ng --band abg <monitor-interface>
```

### Monitor for WPA Handshake
```
sudo airodump-ng -c <channel> --bssid <target-bssid> -w <output-filename> <monitor-interface>
```

### Send a De-authentication Packet
```
sudo aireplay-ng --deauth 1 -a <target-bssid> -c <client-mac-address> <monitor-interface>

And/Or

sudo aireplay-ng --deauth 2 -a <target-bssid> -c <client-mac-address> <monitor-interface>
```

### Disable Monitor Mode
```
sudo airmon-ng stop wlan0
```

### Verify captured authentication data
```
ls | grep <search term>
aircrack-ng <capture-file>
```

### Create wordlist
```
nano wordlist.txt
ls | grep wordlist
```

### Perform offline password recovery
```
aircrack-ng -w wordlist.txt <capture-file>
```

---

# Lessons learned

This exercise demonstrated several important wireless security concepts:

- Wireless network reconnaissance
- 802.11 monitoring
- WPA/WPA2 authentication
- 4-way handshake capture
- Offline Wireless password guessing
- Password strength assessment
- Wireless management-frame security
- Defensive Wi-Fi hardening

The most important lesson was that the security of a WPA/WPA2 network depends not only on the encryption protocol but also significantly on the strength and uniqueness of the password protecting the network.

---

# Conclusion

This Wi-Fi security assessment task was completed against an authorized personal wireless network in a controlled laboratory environment.

The assessment covered wireless network discovery, identification of the authorized access point and client, authentication traffic capture, handshake verification, and offline password recovery.

The exercise demonstrated how weak wireless credentials can be vulnerable to offline password-guessing techniques and reinforced the importance of strong passwords, modern wireless security protocols, Protected Management Frames, and regular network security monitoring.