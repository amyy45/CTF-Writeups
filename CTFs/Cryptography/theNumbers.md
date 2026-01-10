# 🧩 CTF Challenge Writeup  
**Challenge Name:** The Numbers  
**Category:** Cryptography  
**Difficulty:** Easy  
**Platform:** picoCTF  
**Author:** Danny  
**Date:** *08-01-26*

---

## 🔍 1. Challenge Description
This challenge provides a **PNG image containing a sequence of numbers**.
The goal is to **interpret what the numbers represent**, decode them correctly, and recover the hidden flag.

The description is intentionally minimal:
> *The numbers... what do they mean?*

The objective is simple:  
➡️ **Decode the numbers shown in the image to retrieve the flag.**

---

## 🧠 2. Initial Thoughts / Approach
Given the *Cryptography* category and *Easy* difficulty, I expected:

- A classical cipher rather than modern encryption  
- A visual clue embedded in the image  
- Numbers likely mapping directly to letters  

Since the numbers were within the range **1–26**, this strongly suggested the **A1Z26 cipher**, where each number corresponds to a letter of the alphabet.

---

## 🛠️ 3. Steps to Solve

### 1. Download the image using the terminal
The image was downloaded to the local system using the terminal.

### 2. Open the image using the system file manager
The downloaded image was then opened manually using the file manager (Finder) to view its contents clearly.

### 3. Extract the numbers from the image
The image displayed the following sequence:
```yaml
16 9 3 15 3 20 6 { 20 8 5
14 21 13 2 5 18 19 13 1
19 15 14 }
```
### 4. Identify the cipher
The numbers fall between **1 and 26**, matching the positions of letters in the English alphabet:
```yaml
1  → A
2  → B
...
26 → Z
```
Output:
```yaml
picoCTF{caesar_d3cr9pt3d_86de32d2}
```
This confirms the use of the A1Z26 cipher.
### 5. Decode the numbers
Each number was converted to its corresponding letter:
```yaml
16 → P
9  → I
3  → C
15 → O
3  → C
20 → T
6  → F
```
```yaml
20 → T
8  → H
5  → E
```
```yaml
14 → N
21 → U
13 → M
2  → B
5  → E
18 → R
19 → S
13 → M
1  → A
```
```yaml
19 → S
15 → O
14 → N
```
Combining all decoded characters forms a valid picoCTF flag.

---
## 🧩  4. Final Flag 
```bash
picoCTF{thenumbersmason}
```

---
## 📚 5. Key Learnings
- Numbers between 1 and 26 often indicate the A1Z26 cipher
- Image-based challenges may hide very simple encodings
- Opening files in the correct viewer is critical
- Classical ciphers are common in Easy cryptography challenges
- Minimal descriptions often rely on pattern recognition

---
## 🚀 6. Improvements for Next Time
- AQuickly test common classical ciphers when numbers are involved
- Automate A1Z26 decoding using a short script
- Maintain a checklist for identifying classical crypto patterns
- Avoid overthinking when the challenge difficulty is Easy

---
## 🔗 7. References
- picoCTF Cryptography Challenges
- A1Z26 Cipher (Classical Cryptography)
- Introduction to Substitution Ciphers