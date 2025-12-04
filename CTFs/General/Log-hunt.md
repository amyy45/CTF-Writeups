# 🧩 CTF Challenge Writeup  
**Challenge Name: Log Hunt**  
**Category: Forensics**  
**Difficulty: Easy**  
**Platform: picoCTF**  
**Date: 04-12-25**  

---

## 🔍 1. Challenge Description
The server logs seem to leak pieces of a secret flag.
The fragments are scattered, repeated, and must be reconstructed to get the final flag.

Objective:
Find all flag fragments inside the logs and combine them in the correct order.
---

## 🧠 2. Initial Thoughts / Approach
This looked like a typical "flag fragments inside logs" challenge.
I expected the flag to be broken into multiple FLAGPART entries that appear in sequential order.

Plan:
Search for keywords like picoCTF, FLAGPART, or braces

Collect all fragment pieces

Remove duplicates

Combine in correct order to form the final flag
---

## 🛠️ 3. Steps to Solve

### 1. Download the logs
```bash
wget https://challenge-files.picoctf.net/c_amiable_citadel/49cec6157142f24a599f4164d5b63322c2494f801390d6f22eb91b3aa592bc66/server.log
```
### 2. Search for picoCTF in the logs
```bash
grep -i "picoCTF" server.log
```
I found multiple repeated fragments:
```bash
picoCTF{us3_
```
### 3. Search for all flagged parts
```bash
grep -i "FLAGPART" server.log
```
This revealed four unique segments repeating throughout:
```bash
picoCTF{us3_
y0urlinux_
sk1lls_
cedfa5fb}
```
### 4. Combine fragments in logical order
Flag parts clearly form a readable phrase:

1. picoCTF{us3_

2. y0urlinux_

3. sk1lls_

4. cedfa5fb}
Putting them together:
```bash
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```
## 🧩  4. Final Flag 
```bash
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```
## 📚 5. Key Learnings
Extracting repeated patterns from logs using grep

Understanding how CTF flags are often fragmented in logs

Recognizing unique vs repeated entries

Assembling flag fragments logically

Importance of sequential analysis in forensics challenges
## 🚀 6. Improvements for Next Time
Automate fragment extraction using a small Python script

Use regex to capture all alphanumeric sequences in one pass

Search for patterns more efficiently with grep -Eo
## 🔗 7. References
grep manual

picoCTF Forensics category documentation
