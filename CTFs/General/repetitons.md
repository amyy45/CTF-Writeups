# 🧩 CTF Challenge Writeup

**Challenge Name:** repetitions  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2023  
**Author:** THEONESTE BYAGUTANGAZA   
**Date:** *10-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a file with the following description:

> *"Can you make sense of this file? Download the file here."*

A file named `enc_flag` is provided for download.  
The hint **"Multiple decoding is always good"** confirms that the file is encoded more than once using Base64.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- A file containing a Base64-looking string
- Hint mentions **multiple decoding**

Key observations:
- The file contents look like Base64 — long string of alphanumeric characters ending with `==`
- The hint "Multiple decoding is always good" strongly suggests **repeated Base64 encoding**
- Base64 encoding can be applied multiple times — each decode reveals another encoded layer
- The process is: decode → still Base64? → decode again → repeat until plaintext appears
- The flag format `picoCTF{...}` tells us when to stop decoding

The intended approach is a **chain of Base64 decodes**: keep piping `| base64 -d` until the flag appears.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the File
```bash
wget https://artifacts.picoctf.net/c/476/enc_flag
```

Output:
```
enc_flag saved [349/349]
```

### 3.2 Inspect the File
```bash
cat enc_flag
```

Output:
```
VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbFpPYWxKdFVsaFpWVlUxWVZaS1ZWWnVh
RmRXZWtab1dWWmtSMk5yTlZWWApiVVpUVm10d1VWZFdVa2RpYlZaWFZtNVdVZ3BpU0VKeldWUkNk
...
```

The contents are clearly Base64 encoded.

### 3.3 Attempt Single Decode
```bash
base64 -d enc_flag
```

Output is still Base64 — not plaintext yet. The hint says **multiple decoding**, so keep going.

### 3.4 Chain Multiple Base64 Decodes
```bash
cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
```

Output:
```
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_4557ec3e}
```

The flag appears after **6 rounds** of Base64 decoding! ✅

---

## 🧩 4. Final Flag

```
picoCTF{base64_n3st3d_dic0d!n8_d0wnl04d3d_4557ec3e}
```

---

## 📚 5. Key Learnings

- **Base64 can be applied multiple times** — a single decode is not always enough
- The hint "Multiple decoding is always good" is a direct clue to chain decodes
- Piping `| base64 -d` repeatedly is an efficient way to peel off encoding layers
- The flag format `picoCTF{...}` tells you when you've reached the final plaintext
- Always inspect file contents with `cat` before attempting any decoding
- The file was encoded **6 times** with Base64 — each layer hides the next

---

## 🚀 6. Improvements for Next Time

- When a file looks like Base64, **try decoding in a loop** instead of manually chaining:
  ```bash
  data=$(cat enc_flag); for i in {1..10}; do data=$(echo "$data" | base64 -d 2>/dev/null); echo "$data" | grep -q "picoCTF" && echo "$data" && break; done
  ```
- Always read the hint — "multiple decoding" directly tells you to chain decodes
- Count the layers: this file had **6 Base64 layers**
- For unknown encoding depth, start with 1 decode and add more until plaintext appears
- `base64 -d` silently fails on non-Base64 input — watch for `picoCTF{` to know when to stop

---

## 🔗 7. References

- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [Linux base64 man page](https://linux.die.net/man/1/base64)
- [picoCTF General Skills Challenges](https://picoctf.org)
- [CTF101 – Encoding](https://ctf101.org/forensics/what-is-encodingsencryption/)