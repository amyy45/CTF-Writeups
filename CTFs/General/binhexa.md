# 🧩 CTF Challenge Writeup  
**Challenge Name:** binhexa  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** NANA AMA ATOMBO-SACKEY  
**Date:** *09-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents an interactive binary operations quiz over netcat:

> *"How well can you perform basic binary operations?"*

Connect via `nc titan.picoctf.net 64628`, answer 6 binary operation questions correctly, convert the final result to hexadecimal, and receive the flag.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- Binary and hexadecimal operations
- 6 questions covering: multiplication, addition, left shift, OR, AND, right shift

Key observations:
- The two binary numbers given are fixed for the session:
  - **Binary Number 1:** `01101001` = 105 decimal
  - **Binary Number 2:** `01110011` = 115 decimal
- Operations use standard binary arithmetic and bitwise operators
- Final answer must be converted to hexadecimal
- `*` means **multiplication** (not AND as one might assume)
- `+` means **addition**
- `<<` means **left shift**
- `|` means **bitwise OR**
- `&` means **bitwise AND**
- `>>` means **right shift**

---

## 🛠️ 3. Steps to Solve

### 3.1 Connect
```bash
nc titan.picoctf.net 64628
```

Given numbers:
- Binary Number 1: `01101001` = **105**
- Binary Number 2: `01110011` = **115**

---

### 3.2 Question 1 — Multiplication (`*`)
```
105 × 115 = 12075
12075 in binary = 10111100101011
```
**Answer:** `10111100101011` ✅

---

### 3.3 Question 2 — Addition (`+`)
```
  01101001  (105)
+ 01110011  (115)
----------
  11011100  (220)
```
**Answer:** `11011100` ✅

---

### 3.4 Question 3 — Left Shift (`<<`) Binary 1 by 1
```
01101001 << 1 = 11010010
(shift all bits left by 1, drop MSB, add 0 on right)
```
**Answer:** `11010010` ✅

---

### 3.5 Question 4 — Bitwise OR (`|`)
```
  01101001
| 01110011
----------
  01111011
```
**Answer:** `01111011` ✅

---

### 3.6 Question 5 — Bitwise AND (`&`)
```
  01101001
& 01110011
----------
  01100001
```
**Answer:** `01100001` ✅

---

### 3.7 Question 6 — Right Shift (`>>`) Binary 2 by 1
```
01110011 >> 1 = 00111001
(shift all bits right by 1, add 0 on left, drop LSB)
```
**Answer:** `00111001` ✅

---

### 3.8 Convert Final Result to Hex
```
00111001 = 57 decimal = 0x39
```
**Answer:** `39` ✅

---

### 3.9 Flag Received
```
The flag is: picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}
```

---

## 🧩 4. Final Flag

```
picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_aeaf4b09}
```

---

## 📚 5. Key Learnings

- **Binary operations cheat sheet:**

| Operation | Symbol | Description |
|-----------|--------|-------------|
| AND | `&` | 1 only if both bits are 1 |
| OR | `\|` | 1 if either bit is 1 |
| XOR | `^` | 1 if bits are different |
| Left shift | `<<` | Multiply by 2 per shift |
| Right shift | `>>` | Divide by 2 per shift |
| Multiplication | `*` | Standard binary multiplication |
| Addition | `+` | Standard binary addition |

- `*` in binary context means **multiplication**, not AND — `&` is AND
- Left shift by 1 = multiply by 2; Right shift by 1 = integer divide by 2
- To convert binary to hex: group bits into 4s from right, convert each group
- `00111001` → `0011` = 3, `1001` = 9 → `0x39`

---

## 🚀 6. Improvements for Next Time

- Use Python to quickly verify binary operations:
  ```python
  a = 0b01101001  # 105
  b = 0b01110011  # 115
  print(bin(a * b))   # multiplication
  print(bin(a + b))   # addition
  print(bin(a << 1))  # left shift
  print(bin(a | b))   # OR
  print(bin(a & b))   # AND
  print(bin(b >> 1))  # right shift
  print(hex(b >> 1))  # final hex answer
  ```
- Don't assume `*` = AND — it literally means multiplication here
- Always verify binary→hex conversion: `00111001` → group as `0011 1001` → `3` and `9` → `0x39`
- For addition, watch out for carry bits — the result can be longer than 8 bits

---

## 🔗 7. References

- [Bitwise Operations – Wikipedia](https://en.wikipedia.org/wiki/Bitwise_operation)
- [Binary Arithmetic](https://www.electronics-tutorials.ws/binary/bin_7.html)
- [Binary to Hex Converter](https://www.rapidtables.com/convert/number/binary-to-hex.html)
- [Python Bitwise Operators](https://wiki.python.org/moin/BitwiseOperators)
- [picoCTF General Skills Challenges](https://picoctf.org)