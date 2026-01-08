# 🧩 CTF Challenge Writeup  
**Challenge Name:** EVEN RSA CAN BE BROKEN???   
**Category:** Cryptography 
**Difficulty:** Easy  
**Platform:** picoCTF  
**Author:** Michael Crotty  
**Date:** *08-01-26*

---

## 🔍 1. Challenge Description
TThis challenge provides an **interactive RSA encryption service** accessible via `netcat`.  
Each connection generates a new RSA key pair and encrypts the **same secret flag**.

Due to a **flawed key generation process**, multiple RSA moduli share a common prime factor, making the encryption vulnerable.

The objective is simple: 
➡️ **CBreak the RSA encryption and recover the flag.**

---

## 🧠 2. Initial Thoughts / Approach
Given the *Cryptography* category and *Easy* difficulty, I expected a weakness in the **RSA implementation**, not in the mathematical algorithm itself.

The challenge hint suggested:
> *Try comparing N across multiple requests*

This indicated a potential issue with **key reuse or poor randomness** during prime generation.

---

## 🛠️ 3. Steps to Solve

### 1. Connect to the challenge instance
```bash
nc verbal-sleep.picoctf.net 52950
```
Each connection outputs:
- RSA modulus N
- Public exponent e = 65537
- Ciphertext
### 2. Collect multiple RSA moduli
By reconnecting multiple times, different values of N and ciphertexts are collected.

Example:
```yaml
N1 = 14961025568381707387125353663146219912463269589348203844543270881382698093275622786467613135938450563505308234381110192495342502418154152456915922476359234
N2 = 23009412474569982810478714478331318094379137425528126094776672667566416745074149839363037418991398641163834232141077767313934392237905788266194149296099978
```
### 3. Identify the vulnerability (Common Prime Factor)
```ini
N1 = p × q1
N2 = p × q2
```
Then:
```py
gcd(N1, N2) = p
```
This completely breaks RSA security.
### 4. Recover the private key and decrypt
Once the shared prime is found:
- Factor N
- Compute Euler’s totient φ(N)
- Recover the private key d
- Decrypt the ciphertext

**Exploit script:**
```py
import math
from Crypto.Util.number import long_to_bytes, inverse

N1 = 14961025568381707387125353663146219912463269589348203844543270881382698093275622786467613135938450563505308234381110192495342502418154152456915922476359234
N2 = 23009412474569982810478714478331318094379137425528126094776672667566416745074149839363037418991398641163834232141077767313934392237905788266194149296099978

C1 = 3058402194863139100174207027458377277545281503848871528781068757470243877200776031303614328327451432459911442288472651709120925839315053828724871701782879

e = 65537

p = math.gcd(N1, N2)
q = N1 // p
phi = (p - 1) * (q - 1)
d = inverse(e, phi)

m = pow(C1, d, N1)
print(long_to_bytes(m))
```

---
## 🧩  4. Final Flag 
```bash
picoCTF{tw0_1$_pr!m3df98b648}
```

---
## 📚 5. Key Learnings
- RSA security relies heavily on proper key generation
- Sharing even one prime between moduli breaks RSA completely
- Always check gcd(N1, N2) when multiple RSA keys are available
- Cryptographic failures usually stem from implementation flaws, not weak math

---
## 🚀 6. Improvements for Next Time
- Automate GCD checks when analyzing multiple RSA keys
- Study real-world RSA failures (e.g., Debian OpenSSL bug)
- Practice identifying implementation-level crypto flaws
- Build a reusable RSA attack checklist for CTFs

---
## 🔗 7. References
- picoCTF Cryptography Challenges
- RSA Common Prime Factor Attack
- OWASP Cryptographic Failures