# Task 2: PE Analysis (Entry Point Identification)

## Objective

Identify the Address of Entry Point (AEP) of a Windows executable using a PE analysis tool and document the findings with supporting evidence.

---

# Task Overview

The task involved analyzing a provided VeraCrypt executable file to determine the address at which execution begins when the program is launched.

This was achieved using a static analysis approach with a Portable Executable (PE) analysis tool.

---

# Tools & Environment

- Windows Operating System  
- PE Explorer  

---

# Step 1: Launch PE Explorer

Launch the provided PE Analysis software

### Procedure

1. Launch PE Explorer  

### Screenshot

**PE Explorer interface**

![Software Interface](/assets/screenshots/02-intermediate/task-2/pe-explorer-interface.png)

# Step 2: Load the Executable

The provided executable file (`VeraCrypt Setup 1.26.7.exe`) was opened in PE Explorer.

### Procedure

1. Navigate to: 
```
File
 Select
Open
```

2. Select the provided executable file  

Upon loading, PE Explorer automatically displayed the header information.

### Screenshot

**Executable loaded in PE Explorer showing header information**

![Loaded Executable](/assets/screenshots/02-intermediate/task-2/file-loaded-in-pe-explorer.png)

---

# Step 3: Analyze PE Headers

After loading the executable, the **PE Header information** was displayed.

The relevant section for this task is the **HEADER INFO**, which contains key execution-related fields.

---

# Step 4: Identify Address of Entry Point

Within the header information, the following field was located:

```
Address of Entry Point: 004237B0
```

This value represents the location in memory where execution begins when the executable is run.

### Screenshot

**Address of Entry Point**

![AEP](/assets/screenshots/02-intermediate/task-2/address-of-entry-point.png)

---

# Understanding the Entry Point

The Address of Entry Point (AEP) is:

- The first instruction executed by the program
- Defined in the Header Info / PE Optional Header
- Used by the operating system loader to transfer control to the application

It is a critical value in:

- Reverse engineering
- Malware analysis
- Binary debugging

---

# Results Summary

| Field               | Value                      |
| ------------------- | -------------------------- |
| Executable          | VeraCrypt Setup 1.26.7.exe |
| Tool Used           | PE Explorer                |
| Entry Point Address | `004237B0`                 |

---

# Procedure Summary

1. Open executable in PE Explorer
2. Navigate to PE Header / Optional Header / Header Info
3. Locate "Address of Entry Point"
4. Record the value

--- 

# Security & Analysis Insight

The entry point is a key location for analysts because:

1. It marks where program execution begins
2. It is often used as a starting point for debugging or reverse engineering
3. In malware analysis, it may be manipulated or obfuscated to evade detection

Understanding the entry point helps analysts trace program execution and identify suspicious behavior.

---

# Conclusion

The entry point of the provided VeraCrypt executable was successfully identified using PE Explorer.

The Address of Entry Point was determined to be:

```
004237B0
```

This confirms the initial execution address of the binary and demonstrates the use of PE analysis tools for inspecting executable structure.
