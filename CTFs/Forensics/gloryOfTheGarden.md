# 🧩 CTF Challenge Writeup  
**Challenge Name:** Glory of the Garden  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2019  
**Author:** JEDAVIS/DANNY  
**Date:** 26-02-26  

---

## 🔍 1. Challenge Description

> *"This file contains more than it seems. Get the flag from garden.jpg."*

A single JPEG image file `garden.jpg` is provided for download.  
The hint states: **"What is a hex editor?"** — suggesting the flag is hidden in the raw bytes of the file, not visible in the image itself.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- A single JPEG image file
- Hint references a **hex editor** → flag is hidden in raw file bytes, not metadata

Key observations:
- The image looks like a normal garden photo — nothing suspicious visually
- JPEG files can have **arbitrary data appended** after the image data without corrupting the image
- `strings` extracts all human-readable ASCII sequences from any binary file
- The flag was appended as **plaintext** at the end of the JPG file
- `strings garden.jpg | grep picoCTF` finds it instantly

The intended approach is to use `strings` (or a hex editor) to inspect the raw contents of the file beyond what is displayed as an image.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the File
```bash
wget https://challenge-files.picoctf.net/c_fickle_tempest/<hash>/garden.jpg
```

### 3.2 Extract Readable Strings and Search for the Flag
```bash
strings garden.jpg | grep picoCTF
```

Output:
```
Here is a flag: picoCTF{more_than_m33ts_the_3y339140129}
```

The flag was appended as plain ASCII text at the end of the JPEG file — completely invisible when the image is opened normally, but immediately revealed by `strings`.

### 3.3 Alternative — Inspect Raw Bytes with xxd
```bash
xxd garden.jpg | tail -20
```

This would show the flag in hex + ASCII representation at the end of the file, confirming it was simply appended after the image data.

---

## 🧩 4. Final Flag

```
picoCTF{more_than_m33ts_the_3y339140129}
```

---

## 📚 5. Key Learnings

- **`strings`** is one of the most powerful and fastest forensics tools — it extracts all human-readable ASCII sequences from any binary file and should be run early in any forensics challenge
- Data can be **appended to the end of a JPEG** without breaking the image — image viewers stop reading at the JPEG end-of-image marker (`FFD9`) and ignore everything after it
- The challenge name **"more_than_m33ts_the_3y3"** (more than meets the eye) is a direct hint that the file contains hidden data beyond what is visually displayed
- The hint "What is a hex editor?" points to inspecting raw file bytes — `strings` is the quickest way, while `xxd` or `hexdump` give a full hex view
- This technique of appending data to image files is used in real-world **steganography** and **malware** to hide payloads inside innocent-looking files
- Always run both `strings` and `exiftool` on image files — they cover two different hiding locations (raw bytes vs metadata)

---

## 🚀 6. Improvements for Next Time

- **Run `strings <file> | grep picoCTF` immediately** — it's the fastest possible check for any forensics challenge
- If `strings` finds nothing, move to `exiftool` for metadata inspection
- Use `xxd <file> | tail -50` to manually inspect the end of a file for appended data
- Try `binwalk <file>` to detect and extract any embedded files or archives
- For JPEG files specifically, remember that data after `FFD9` (end-of-image marker) is ignored by image viewers but preserved in the file

---

## 🔗 7. References

- [strings Man Page](https://man7.org/linux/man-pages/man1/strings.1.html)
- [xxd Man Page](https://linux.die.net/man/1/xxd)
- [JPEG File Format – Wikipedia](https://en.wikipedia.org/wiki/JPEG)
- [Steganography – Wikipedia](https://en.wikipedia.org/wiki/Steganography)
- [picoCTF Forensics Challenges](https://picoctf.org)