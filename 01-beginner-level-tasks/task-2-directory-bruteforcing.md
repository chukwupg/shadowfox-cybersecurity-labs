# Task 2: Directory Bruteforcing

## Objective

Discover hidden directories and files exposed by the target web application through directory enumeration. This process helps identify additional attack surfaces that may not be directly accessible through the application's navigation.

---

# Target Information

- **Target URL:** http://testasp.vulnweb.com/
- **Protocol:** HTTP
- **Port:** 80

---

# Tools Used

- Kali Linux
- Gobuster v3.8.2
- SecLists wordlist (`common.txt`)

---

# Methodology

## Step 1: Select a Wordlist

A commonly used web content wordlist from SecLists was selected to enumerate standard directories and files.

```text
/usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

---

## Step 2: Perform Directory Enumeration

Gobuster was used to brute force directories and files on the target web server.

### Command

```
gobuster dir \
-u http://testasp.vulnweb.com \
-w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt \
-x asp,aspx,txt,bak \
-t 30
```

### Command Options Used

* `-u` : Target URL
* `-w` : Wordlist for brute forcing
* `-x asp,aspx,txt,bak` : Checks for common file extensions relevant to IIS/ASP applications and backup files
* `-t 30` : Uses 30 concurrent threads for faster enumeration

These options improve coverage by targeting both directories and common file types while maintaining efficient scan speed.

---

# Enumeration Results

## Directories and Files Discovered

| Path            | Status Code | Observation                                      |
| --------------- | ----------- | ------------------------------------------------ |
| /Default.asp    | 200         | Main landing page                                |
| /Login.asp      | 200         | Authentication endpoint                          |
| /Search.asp     | 200         | Search functionality                             |
| /register.asp   | 200         | User registration page                           |
| /logout.asp     | 302         | Redirects to default page                        |
| /showthread.asp | 302         | Likely forum-related endpoint                    |
| /DB.asp         | 200         | Potential backend/database-related file (size 0) |
| /robots.txt     | 200         | Contains crawler directives                      |
| /Images/        | 403         | Access denied (permission-controlled directory)  |
| /HTML/          | 403         | Access denied (restricted static content)        |
| /jscripts/      | 403         | Access denied (restricted scripts directory)     |
| /templates/     | 403         | Access denied (restricted template files)        |
| /avatars/       | 403         | Access denied (user content likely protected)    |
| /aspnet_client/ | 403         | Access denied (ASP.NET client resources)         |
| /cgi-bin/       | 301/403     | Restricted server-side scripts                   |
| /_vti_cnf/      | 200         | Configuration files exposed to browser           |
| /T/             | 301         | Unclear purpose (requires further inspection)    |

---

# Analysis of Findings

## Authentication Endpoints

### `/Login.asp`

This endpoint represents the login interface of the application and is critical for authentication testing.

### `/register.asp`

Allows user registration and may be useful for creating test accounts for further analysis.

### `/logout.asp`

Redirect behavior suggests session handling is implemented.

---

## Application Functionality

### `/Search.asp`

Search functionality is often vulnerable to input-based attacks such as SQL Injection.

### `/showthread.asp`

Indicates forum or dynamic content functionality.

---

## Potentially Sensitive Files

### `/DB.asp`

This file is particularly interesting due to its name suggesting database interaction. Although it returned an empty response (size 0), it may still represent backend logic or misconfiguration.

### `/robots.txt`

The file returned:

```
User-agent: *
```

This indicates that all crawlers are allowed, and no specific restrictions or hidden paths were defined. While not directly revealing hidden directories, it confirms a lack of defensive crawling controls, which may increase exposure to automated enumeration.

---

## Static and Resource Directories

The following directories returned **403 Forbidden (Access Denied)**:

* `/Images/`
* `/HTML/`
* `/jscripts/`
* `/templates/`
* `/avatars/`
* `/aspnet_client/`

### Security Interpretation

The 403 responses indicate that these directories exist but are protected by server-side access controls. This suggests:

* Permission-based restrictions are enforced at the web server level
* Access may be controlled by user roles or authentication state
* Directory structure is still exposed, even if content is not accessible

This is important because directory visibility alone can assist attackers in mapping application architecture, even without direct access.

---

## Server & Configuration Indicators

### `/aspnet_client/`

Although access is denied (403), its presence confirms ASP.NET components are installed and in use.

### `/_vti_cnf/`

This directory returned **configuration files exposed to the browser**, which is a significant finding. It is associated with Microsoft FrontPage extensions and may contain metadata or configuration details that should not be publicly accessible.

Exposure of this directory suggests:

- Misconfigured legacy components
- Potential leakage of internal file structure or metadata
- Increased reconnaissance value for attackers

### `/cgi-bin/`

Returned **403 Forbidden**, indicating restricted execution of server-side scripts.

---

## Anomalous Findings

### `/T/`

Unclear functionality. Requires manual browsing or further testing.

### Query-Based Endpoints

```text
render?url=https://www.google.com.aspx
dns-query?name=google.com&type=A.aspx
```

These returned **400 Bad Request**, suggesting possible input-handling endpoints that may be useful in advanced testing scenarios.

---

# Security Analysis

Directory enumeration significantly expanded the visible attack surface of the application.

Key observations:

- Multiple dynamic ASP endpoints are exposed
- Authentication and user interaction features are accessible
- Legacy or configuration-related directories exist
- Potential backend-related file (`DB.asp`) identified
- Input-driven endpoints detected (possible injection points)
- Directory structure is partially exposed even when access is restricted (403 responses)

### Additional Security Insight

The presence of multiple **403 Forbidden directories** is itself a security-relevant finding. While access is denied, the server still discloses:

- Valid directory names
- Application structure
- Technology stack components

This supports **information disclosure through enumeration**, which can assist attackers in targeting specific components.

The exposure of `/_vti_cnf/` is particularly significant, as it may leak configuration metadata from legacy Microsoft FrontPage extensions, which are known to introduce security risks when improperly secured.

The `robots.txt` file showing only:

```
User-agent: *
```

indicates a lack of defensive crawling restrictions, meaning automated tools can freely enumerate the application without guidance or limitation.

---

# Evidence

**Selected Wordlist (SecLists `common.txt`)**

![Selected wordlist](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/wordlist.png)

**Gobuster command execution**

![Gobuster command execution](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/gobuster-command-execution.png)

**Full enumeration output**

![Gobuster enumeration output](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/gobuster-enumeration-result.png)

**Browser access to `/Login.asp`**  

![Browser Access to Login page](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/endpoint-login-asp.png)

**Browser access to `/Search.asp`**

![Browser access to search](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/endpoint-search-asp.png)

**Browser access to `/_vti_cnf/`**

![Browser access to config file](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/endpoint-vti-cnf.png)

**Browser access to `robot.txt`**

![Robot.txt content](/assets/screenshots/01-beginner/task-2-directory-bruteforcing/endpoint-robots-txt.png)

---

# Command Log

```
ls /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
---
gobuster dir \
-u http://testasp.vulnweb.com \
-w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt \
-x asp,aspx,txt,bak \
-t 30
```

---

# Conclusion

The directory bruteforcing phase successfully revealed numerous endpoints and directories that are not immediately visible through standard navigation. While several directories are protected via access controls (403 Forbidden), their exposure still contributes to application fingerprinting and attack surface mapping.

Additionally, the discovery of exposed configuration-related content in `/_vti_cnf/` increases the severity of information disclosure concerns.

---

## Next Step

**Task 3: Traffic Analysis**, where I will be capturing login activity using Wireshark to analyze how credentials are transmitted across the network.
