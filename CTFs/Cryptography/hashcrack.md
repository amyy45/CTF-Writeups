# 🧩 CTF Challenge Writeup  
**Challenge Name:** hashcrack  
**Category:** Cryptography 
**Difficulty:** Easy  
**Platform:** picoCTF  
**Author:** Nana Ama Atombo-Sackey  
**Date:** *08-01-26*

---

## 🔍 1. Challenge Description
This challenge presents an **interactive netcat service**  that sequentially provides cryptographic hashes.
The goal is to **identify each hash type**, crack it to recover the plaintext password, and input it correctly to progress.

The challenge increases in difficulty by:

- Upgrading hash algorithms 
- Keeping passwords weak but realistic
- Testing understanding of hash identification and cracking workflows

The objective is simple:  
➡️ **Crack all provided hashes and retrieve the final flag.**

---

## 🧠 2. Initial Thoughts / Approach
Given the Cryptography category and Easy difficulty, I expected:

- Common unsalted hash algorithms  
- Weak, leaked-style passwords
- Hash identification based on length  
- Manual cracking using logic and small wordlists

Since the service was interactive, accuracy and careful input were important.


---

## 🛠️ 3. Steps to Solve

### 1. Connect to the challenge instance
```bash
nc verbal-sleep.picoctf.net 53485
```
### 2. Crack Hash #1
```yaml
482c811da5d5b4bc6d497ffa98491e38
```
- Length: 32 hex characters 
- Identified as MD5

Cracking revealed the password:
```yaml
password123
```
Entered the password successfully to proceed.
### 3. Crack Hash #2
```yaml
b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```
- Length: 40 hex characters 
- Identified as SHA-1

Using a small wordlist and local verification:
```bash
echo -n "letmein" | sha1sum
```
Password:
```yaml
letmein
```
Entered correctly to advance to the final stage.
### 4. Crack Hash #3
```yaml
916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```
- Length: 64 hex characters 
- Identified as SHA-256

The password was not found in basic wordlists, so an external hash database was used to crack the hash.
Recovered password:
```yaml
qwerty098
```
Entered the password revealed the flag.

---
## 🧩  4. Final Flag 
```bash
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_eb2f8459}
```

---
## 📚 5. Key Learnings
- Hash length is a reliable way to identify common algorithms 
- Strong hashes do not protect weak passwords
- Unsalted hashes are vulnerable to dictionary attacks
- Manual cracking has limits — knowing when to escalate tools is important
- Verification of hashes is critical; never rely on assumptions

---
## 🚀 6. Improvements for Next Time
- Use larger wordlists (e.g., rockyou) when available
- Practice hashcat modes for different algorithms
- Automate cracking workflows for efficiency
- Be cautious of terminal input errors in interactive services

---
## 🔗 7. References
- picoCTF Cryptography Challenges
- man nc
- man sha1sum / man sha256sum
- OWASP Password Storage Guidelines