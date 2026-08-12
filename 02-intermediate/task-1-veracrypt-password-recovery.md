# Task 1: VeraCrypt Password Recovery

## Objective

Recover the password required to access a VeraCrypt-encrypted container by cracking the provided MD5 hash, use the recovered password to unlock the container, and retrieve the secret code stored inside.

---

# Task Overview

The task provided:

- An encrypted password stored in `encoded.txt`
- A VeraCrypt-encrypted container
- A VeraCrypt setup executable (`.exe`)

The assessment was divided into two stages:

1. Recover the plaintext password from the supplied hash.
2. Use the recovered password to mount the VeraCrypt container and retrieve the secret code.

---

# Tools & Environment

### Password Recovery

- Kali Linux
- John the Ripper
- RockYou wordlist

### VeraCrypt 

- Windows
- VeraCrypt

---

# 🔍 Step 1: Inspect the Provided Hash

The contents of `encoded.txt` were inspected to determine the value that needed to be recovered.

### Hash

```
482c811da5d5b4bc6d497ffa98491e38
```

The value consists of 32 hexadecimal characters, which is consistent with an MD5 hash.

### Screenshot

**encoded.txt showing the supplied hash***

![encoded.txt](/assets/screenshots/02-intermediate/task-1/inspect-file-hash.png)

---

# Step 2: Crack the Hash

For the cracking stage;

- John the Ripper was used with the RockYou password wordlist.

- I explicitly instructed John the Ripper to treat the input as a raw MD5 hash.

### Command
```
john encoded.txt --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt
```

### Result

John the Ripper successfully recovered the password:

```
password123
```

The recovered password was confirmed using:

### Command

```
john --show --format=Raw-MD5 encoded.txt
```

### Result
```
?:password123

1 password hash cracked, 0 left
```

### Screenshot

**John the Ripper showing the successful password recovery**

![Password Recovery](/assets/screenshots/02-intermediate/task-1/john-cracked-hashed.png)
---

# Step 3: Launch VeraCrypt on Windows

Proceeded to download and install the provided VeraCrypt setup executable to my Windows machine.

The Windows environment was used specifically for the VeraCrypt portion of the task because the provided setup file was a Windows executable.

### Screenshot

**VeraCrypt main interface on Windows**

![Veracrypt](/assets/screenshots/02-intermediate/task-1/veracrypt-interface.png)

--- 

# Step 4: Mount the Encrypted Container

Selected the encrypted container through the VeraCrypt interface.

### Procedure
1. Open VeraCrypt.
2. Select an available drive letter.
3. Select the provided encrypted container using `Select File` option.
4. Click Mount.
5. Enter the password recovered during the hash-cracking stage.
6. Confirm the mount operation.

The recovered password successfully unlocked the encrypted container.

Screenshot

**VeraCrypt mount operation with the encrypted container selected**

![Veracrypt mount operation](/assets/screenshots/02-intermediate/task-1/veracrypt-mount-operation.png)

**Successful mounting of the VeraCrypt volume**

![Successful mount operation](/assets/screenshots/02-intermediate/task-1/veracrypt-succesful-mount-operation.png)

---

Step 5: Access the Encrypted Volume

After successful mounting, the encrypted volume became accessible through Windows File Explorer.

The contents of the mounted volume were inspected for the file containing the required secret code.

### Screenshot

**Mounted VeraCrypt volume visible in Windows File Explorer**

![Windows file explorer](/assets/screenshots/02-intermediate/task-1/mounted-veracrypt-volume.png)

---

# Step 6: Retrieve the Secret Code

Opened the container to retrieve the secret code

The secret code contained within the encrypted volume was successfully recovered.


#### Secret Code
> never giveup

### Screenshot

**File containing the recovered secret code**

![Secret Code](/assets/screenshots/02-intermediate/task-1/secret-code.png)

---

# Results Summary

| Stage               | Result                             |
| ------------------- | ---------------------------------- |
| Hash supplied       | `482c811da5d5b4bc6d497ffa98491e38` |
| Hash format used    | Raw MD5                            |
| Cracking tool       | John the Ripper                    |
| Wordlist            | RockYou                            |
| Password recovery   | Successful                         |
| VeraCrypt container | Successfully mounted               |
| Secret code         | `never giveup`                     |

---

# Security Analysis

The exercise demonstrates the importance of password strength when passwords are represented by hashes.

The supplied MD5 hash was successfully cracked using a commonly available password wordlist. The recovered password was then sufficient to unlock the VeraCrypt container.

This demonstrates that strong encryption can still be undermined when the password protecting the encrypted data is weak or easily guessable.

It also highlights an important distinction:

The VeraCrypt encryption itself was not cracked.

Instead, the password protecting the encrypted volume was recovered from a weak password hash. The recovered password was then legitimately used to unlock the container.

--- 

# Security Considerations

### Weak Passwords

The recovered password was present in the RockYou wordlist, demonstrating that passwords based on common or predictable choices are vulnerable to dictionary-based attacks.

### MD5

MD5 is a legacy cryptographic hash function and not suitable for modern password storage. Passwords should instead be protected using secure and dedicated password-hashing algorithms with appropriate salting.

### Password Hash Exposure

If an attacker obtains password hashes, offline cracking can be performed without interacting with the original authentication system. Strong, unique passwords therefore remain important even when passwords are stored as hashes.

--- 

# Command Log

```
cat encoded.txt
john encoded.txt --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt
john --show --format=Raw-MD5 encoded.txt
```

---

# Conclusion

The VeraCrypt password-recovery task was successfully completed.

The supplied MD5 hash was cracked using John the Ripper and the RockYou wordlist. The recovered password was subsequently used on Windows with VeraCrypt to successfully mount the encrypted container. The required secret code `never giveup` was then retrieved from the mounted volume.

The exercise demonstrated the relationship between password strength, password hashing, offline cracking, and the security of encrypted data.