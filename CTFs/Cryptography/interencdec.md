# 🧩 CTF Challenge Writeup  
**Challenge Name:** Einternecdec  
**Category:** Cryptography 
**Difficulty:** Easy  
**Platform:** picoCTF  
**Author:** NGIRIMANA Schadrack
**Date:** *09-01-26*

---

## 🔍 1. Challenge Description
This challenge provides an **encoded flag stored in a file**.
The goal is to **identify multiple layers of encoding**, decode them in the correct order, and recover the final flag.

Based on the challenge name (*interencdec*) and tags, the flag is encoded using **more than one cryptographic technique**.

The objective is simple: 
➡️ **Decode all layers and retrieve the flag.**

---

## 🧠 2. Initial Thoughts / Approach
Given the *Cryptography* category and *Easy* difficulty, I expected:

- Simple classical encodings rather than complex cryptography  
- Multiple encoding layers stacked together  
- Use of common techniques such as:
  - Base64
  - Caesar (ROT) cipher  

The challenge name suggested an **encode → decode → encode → decode** pattern, so decoding repeatedly until readable text appeared was the correct strategy.

---

## 🛠️ 3. Steps to Solve

### 1. View the encoded file
```bash
cat enc_flag
```
Output:
```yaml
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==
```
### 2. Decode Base64 (First Layer)
The string ends with ==, which strongly indicates Base64 encoding.
```bash
echo "YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==" | base64 -d
```
Output:
```yaml
b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ=='
```
### 3. Decode Base64 (Second Layer)
The output is still Base64-encoded.
```bash
echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ==" | base64 -d
```
Output:
```yaml
wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}
```
This resembles a flag format but contains scrambled letters, indicating another encoding layer.
### 4. Decode Caesar Cipher (Final Layer)
Only alphabetic characters are shifted while numbers and symbols remain unchanged, indicating a **Caesar cipher**.

A Python script was used to shift characters backward by 7:
**Exploit script:**
```bash
python3 - << 'EOF'
import string

s = "wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}"
out = ""
for c in s:
    if c.isalpha():
        alpha = string.ascii_lowercase if c.islower() else string.ascii_uppercase
        out += alpha[(alpha.index(c) - 7) % 26]
    else:
        out += c
print(out)
EOF
```
Output:
```yaml
picoCTF{caesar_d3cr9pt3d_86de32d2}
```

---
## 🧩  4. Final Flag 
```bash
picoCTF{caesar_d3cr9pt3d_86de32d2}
```

---
## 📚 5. Key Learnings
- Base64 encoding can be identified by its character set and padding (=)
- Encoded outputs may contain multiple encoding layers
- Caesar ciphers affect only alphabetic characters
- Python is more reliable than shell utilities for Caesar decoding
- Challenge tags often directly indicate the solution path

---
## 🚀 6. Improvements for Next Time
- Automate layered decoding using scripts
- Programmatically brute-force Caesar shifts
- Build a reusable multi-encoding decoder
- Practice faster identification of encoding patterns

---
## 🔗 7. References
- picoCTF Cryptography Challenges
- Base64 Encoding (RFC 4648)
- Caesar Cipher (Classical Cryptography)