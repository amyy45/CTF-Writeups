# 🧩 CTF Challenge Writeup  
**Challenge Name:** endianness  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** NANA AMA ATOMBO-SACKEY  
**Date:** *04-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents an interactive netcat service:

> *"Know of little and big endian?"*

Connect via `nc titan.picoctf.net 60260`, convert a given word into both its little endian and big endian hex representations, and receive the flag.

The hint says **"You might want to check the ASCII table to first find the hexadecimal representation of characters before finding the endianness"** — confirming the approach.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- About **endianness** — byte ordering in memory
- The server gives a word and asks for both little and big endian hex representations

Key observations:
- **Big Endian** = bytes stored left to right (most significant byte first) — natural reading order
- **Little Endian** = bytes stored right to left (least significant byte first) — reversed order
- Steps: convert each character to its ASCII hex value, then arrange in big or little endian order
- No spaces in the answer — just concatenated hex bytes

The intended approach is to **look up each character's ASCII hex value**, write them in order for big endian, and reverse the order for little endian.

---

## 🛠️ 3. Steps to Solve

### 3.1 Connect to the Server
```bash
nc titan.picoctf.net 60260
```

Server gives the word: **srkck**

### 3.2 Convert Each Character to ASCII Hex

| Character | ASCII (decimal) | ASCII (hex) |
|-----------|----------------|-------------|
| s | 115 | 73 |
| r | 114 | 72 |
| k | 107 | 6B |
| c | 99  | 63 |
| k | 107 | 6B |

### 3.3 Arrange by Endianness

**Big Endian** — normal left-to-right order (most significant byte first):
```
73 72 6B 63 6B → 73726B636B
```

**Little Endian** — reversed order (least significant byte first):
```
6B 63 6B 72 73 → 6B636B7273
```

### 3.4 Submit Both Answers
```
Enter the Little Endian representation: 6B636B7273
Correct Little Endian representation!
Enter the Big Endian representation: 73726B636B
Correct Big Endian representation!
Congratulations! You found both endian representations correctly!
Your Flag is: picoCTF{3ndi4n_sw4p_su33ess_02999450}
```

---

## 🧩 4. Final Flag

```
picoCTF{3ndi4n_sw4p_su33ess_02999450}
```

---

## 📚 5. Key Learnings

- **Big Endian** stores the most significant byte first — the natural human reading order (left to right)
- **Little Endian** stores the least significant byte first — the reversed order
- Most modern CPUs (x86, x64) use **little endian** — this is why it matters in cybersecurity and reverse engineering
- Network protocols typically use **big endian** (also called "network byte order")
- Converting characters to endian representations: look up ASCII hex → arrange in order (big) or reverse (little)
- The word is treated as a sequence of bytes — each character is one byte in ASCII

---

## 🚀 6. Improvements for Next Time

- Memorize common ASCII hex values: `a=61`, `z=7A`, `A=41`, `Z=5A`, `0=30`, `9=39`
- Write a quick Python one-liner for future endianness challenges:
  ```python
  word = "srkck"
  hex_bytes = [format(ord(c), '02x') for c in word]
  print("Big Endian:   ", ''.join(hex_bytes))
  print("Little Endian:", ''.join(reversed(hex_bytes)))
  ```
- Remember: **little endian = reverse the byte order**, not the bits
- In reverse engineering, always check if a value is little or big endian when reading hex dumps

---

## 🔗 7. References

- [Endianness – Wikipedia](https://en.wikipedia.org/wiki/Endianness)
- [ASCII Table](https://www.asciitable.com/)
- [Computerphile – Endianness Explained](https://www.youtube.com/watch?v=NcaiHcBvDR4)
- [picoCTF General Skills Challenges](https://picoctf.org)