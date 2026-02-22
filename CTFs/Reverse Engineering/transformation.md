# 🧩 CTF Challenge Writeup  
**Challenge Name:** Transformation  
**Category:** Reverse Engineering  
**Difficulty:** Easy  
**Platform:** picoCTF 2021  
**Author:** MADSTACKS  
**Date:** 22-02-26  

---

## 🔍 1. Challenge Description

The challenge presents an encoding scheme with the following description:

> *"I wonder what this really is..."*

The encoding formula is shown directly in the challenge:

```python
enc = "".join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
```

An encoded file `enc` is provided containing the result of running this transformation on the flag.  
The hint **"You may find some decoders online"** suggests this is a known encoding pattern that can be reversed mathematically.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Reverse Engineering** category  
- **Easy** difficulty  
- The encoding formula is provided directly — no black-box reversing needed
- The encoded output is a Unicode string

Key observations:
- The formula takes **2 characters at a time** from the flag
- It shifts the first character's ASCII value **left by 8 bits** (`<< 8`), then **adds** the second character's ASCII value
- This combines two 8-bit ASCII characters into a single **16-bit Unicode character**
- To reverse: extract the upper 8 bits (`>> 8`) for the first character and lower 8 bits (`& 0xFF`) for the second

The intended approach is to **write a simple Python decoder** that reverses the bit manipulation.

---

## 🛠️ 3. Steps to Solve

### 3.1 Understand the Encoding

The encoding formula:
```python
chr((ord(flag[i]) << 8) + ord(flag[i + 1]))
```

Works like this for each pair of characters:

```
flag[i]   = 'p'  → ord('p') = 112
flag[i+1] = 'i'  → ord('i') = 105

encoded = chr((112 << 8) + 105)
        = chr(28672 + 105)
        = chr(28777)
        = one Unicode character
```

So every **2 ASCII characters** → **1 Unicode character**.

### 3.2 Work Out the Reverse

To decode, for each Unicode character:

```
val = ord(unicode_char)

first_char  = chr(val >> 8)      # upper 8 bits
second_char = chr(val & 0xFF)    # lower 8 bits
```

### 3.3 Write the Decoder

```python
# Read the encoded file
enc = open('enc', 'r', encoding='utf-8').read()

# Reverse the transformation
flag = ""
for char in enc:
    val = ord(char)
    flag += chr(val >> 8)      # extract first original character
    flag += chr(val & 0xFF)    # extract second original character

print(flag)
```

### 3.4 Run the Script

```bash
python3 reng.py
```

Output:
```
picoCTF{16_bits_inst34d_of_8_b7f62ca5}
```

---

## 🧩 4. Final Flag

```
picoCTF{16_bits_inst34d_of_8_b7f62ca5}
```

---

## 📚 5. Key Learnings

- **Bit shifting** is a common encoding technique — `<< 8` moves bits left by 8 places (multiplies by 256), packing two bytes into one value
- Two 8-bit ASCII characters can be combined into a single **16-bit Unicode character** using `(A << 8) + B`
- To reverse: use `>> 8` (right shift) to get the upper byte and `& 0xFF` (bitwise AND) to isolate the lower byte
- When the encoding formula is given directly, just **reverse the math** — no need for brute force
- The flag name `16_bits_inst34d_of_8` perfectly describes the technique — 16-bit Unicode instead of standard 8-bit ASCII

---

## 🚀 6. Improvements for Next Time

- When an encoding formula is provided, immediately identify: **what goes in, what comes out, and how to reverse it**
- Recognize bit shift patterns: `<< 8` means packing, `>> 8` and `& 0xFF` means unpacking
- Always open encoded files with `encoding='utf-8'` in Python when Unicode characters are expected
- For any `chr()` based encoding, `ord()` is the reverse — and bit operations are always reversible
- Check the flag name after solving — it often explains the technique used

---

## 🔗 7. References

- [Python Bitwise Operators](https://wiki.python.org/moin/BitwiseOperators)
- [MDN – Unicode and Character Encoding](https://developer.mozilla.org/en-US/docs/Glossary/Unicode)
- [Python ord() and chr() Documentation](https://docs.python.org/3/library/functions.html#ord)
- [picoCTF Reverse Engineering Challenges](https://picoctf.org)