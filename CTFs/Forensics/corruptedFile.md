# 🧩 CTF Challenge Writeup
**Challenge Name:** Corrupted file
**Category:** Forensics
**Difficulty:** Easy
**Platform:** picoMini by CMU-Africa
**Author:** YAHAYA MEDDY
**Date:** 23-02-26

---

## 🔍 1. Challenge Description

The challenge presents a file with the following description:

> *"This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?"*

A file named `file.1` is provided for download. The hint suggests using tools like `xxd` or `hexdump` to inspect and edit file bytes.

The objective is to analyze the corrupted file, repair it, and retrieve the flag.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- A file that appears broken
- Hint mentions `xxd` / `hexdump`

Key observations:
- Files often fail to open when their **magic bytes** (file signature) are corrupted
- Inspecting the file header using `xxd` can reveal incorrect starting bytes
- JPEG files must begin with the magic number:
  ```
  FF D8 FF
  ```
- The file started with:
  ```
  5C 78 FF E0
  ```
- `5C 78` corresponds to the ASCII characters `\x`, which were mistakenly inserted
- Removing those extra two bytes restores the correct JPEG header

The intended solution is: **Inspect header → identify incorrect bytes → remove corruption → reconstruct file → extract flag.**

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the File
```bash
wget <challenge-url>/file
```

The file is saved as:
```
file.1
```

### 3.2 Inspect File Header
```bash
xxd file.1 | head
```

Output:
```
00000000: 5c78 ffe0 0010 4a46 4946 0001 0100 0001
```

The first bytes are:
```
5C 78 FF E0
```

However, a valid JPEG should begin with:
```
FF D8 FF E0
```

The extra `5C 78` (`\x`) is corrupting the file.

### 3.3 Dump File to Editable Hex Format
```bash
xxd file.1 > file.hex
```

### 3.4 Edit the Header
```bash
nano file.hex
```

Original first line:
```
00000000: 5c78 ffe0 0010 4a46 ...
```

Modify it to:
```
00000000: ffd8 ffe0 0010 4a46 ...
```

Save and exit.

### 3.5 Reconstruct the File
```bash
xxd -r file.hex fixed.jpg
```

### 3.6 Verify File Type
```bash
file fixed.jpg
```

Output:
```
JPEG image data, JFIF standard 1.01
```

The file is now restored successfully.

### 3.7 View the Image

After downloading or reconstructing locally:
```bash
open fixed.jpg
```

The image displays the flag.

---

## 🧩 4. Final Flag

```
picoCTF{3sT0r1ng_th3_by73s_e1d8a8c0}
```

---

## 📚 5. Key Learnings

- Many file formats rely on **magic numbers** (file signatures) at the beginning
- JPEG files must start with `FF D8`
- Even **2 incorrect bytes** can completely break a file
- `xxd` is a powerful tool for:
  - Viewing file contents in hex
  - Editing raw bytes
  - Reconstructing modified files
- CTF forensic challenges often test understanding of:
  - File structure
  - Magic numbers
  - Byte-level editing

---

## 🚀 6. Improvements for Next Time

- Always run:
  - `file filename`
  - `xxd filename | head`
- Memorize common file signatures:
  - PNG → `89 50 4E 47`
  - JPEG → `FF D8 FF`
  - GIF → `47 49 46 38`
  - PDF → `%PDF`
- If a file doesn't open but structure looks correct → check header first
- Small byte edits often solve "corrupted file" challenges

---

## 🔗 7. References

- `man xxd`
- [JPEG File Format Specification](https://www.w3.org/Graphics/JPEG/)
- [Common File Signatures Reference](https://en.wikipedia.org/wiki/List_of_file_signatures)
- [picoCTF Forensics Challenges](https://picoctf.org)
- [CTF101 – Forensics Fundamentals](https://ctf101.org/forensics/overview/)