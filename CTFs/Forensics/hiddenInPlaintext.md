# 🧩 CTF Challenge Writeup  
**Challenge Name:** Hidden in Plainsight  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoMini by CMU-Africa  
**Author:** YAHAYA MEDDY  
**Date:** 23-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a JPG image with the following description:

> *"You're given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag."*

A JPG image file `img.jpg` is provided for download.  
The hint **"Download the jpg image and read its metadata"** confirms the first step, but the full solution involves a double Base64 decode chain followed by steganographic extraction.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category  
- **Easy** difficulty  
- A seemingly ordinary JPG image
- Hint points to **metadata**

Key observations:
- Image files can hide data in both **metadata fields** and **steganographically inside the image data**
- `exiftool` reveals all metadata fields including `Comment`, which is often used to hide data
- A Base64 encoded string in the `Comment` field suggests encoded information
- The decoded value `steghide:...` reveals both the tool (`steghide`) and an encoded password
- `steghide` is a steganography tool that hides files inside JPG images — extracting requires the correct password

The intended approach is a **chain**: exiftool → Base64 decode → Base64 decode again → steghide extract → flag.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Image
```bash
wget <challenge-url>/img.jpg
```

### 3.2 Extract Metadata Using exiftool
```bash
exiftool img.jpg
```

The `Comment` field contains a suspicious Base64 string:
```
Comment: c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

### 3.3 First Base64 Decode
```bash
echo "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9" | base64 -d ; echo
```

Output:
```
steghide:cEF6endvcmQ=
```

This reveals two things — the tool to use (`steghide`) and another Base64 encoded string (the password).

### 3.4 Second Base64 Decode
```bash
echo "cEF6endvcmQ=" | base64 -d ; echo
```

Output:
```
pAzzword
```

This is the **steghide password**.

### 3.5 Extract Hidden File Using steghide
```bash
steghide extract -sf img.jpg -p pAzzword
```

Output:
```
wrote extracted data to "flag.txt".
```

### 3.6 Read the Flag
```bash
cat flag.txt
```

Output:
```
picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}
```

---

## 🧩 4. Final Flag

```
picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}
```

---

## 📚 5. Key Learnings

- **Image metadata** (especially the `Comment` field) is a common place to hide encoded data in forensics challenges
- Always try **Base64 decoding** on any suspicious string found in metadata — and decode **multiple times** if the result still looks encoded
- The format `tool:encoded_password` is a common CTF pattern — it tells you both the tool and credentials needed
- **Steghide** is a popular steganography tool that hides files inside JPG images — always try it when an image challenge involves hidden payloads
- The full decode chain here was: `Base64 → steghide tool name + Base64 password → Base64 → plaintext password → steghide extract → flag`
- Always add `; echo` after `base64 -d` to force a newline and make output visible

---

## 🚀 6. Improvements for Next Time

- For any image forensics challenge: run `exiftool` first, then `strings`, then try steganography tools
- When Base64 decoding gives another encoded string — **keep decoding** until you get plaintext
- Common steganography tools to try: `steghide`, `zsteg`, `stegsolve`, `binwalk`
- `steghide` only works with **JPG and BMP** files — for PNG use `zsteg` instead
- The `Comment` field in image metadata is one of the most commonly used hiding spots in forensics CTFs

---

## 🔗 7. References

- [ExifTool Documentation](https://exiftool.org/)
- [Steghide Documentation](http://steghide.sourceforge.net/documentation.php)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [CTF Steganography Techniques](https://ctf101.org/forensics/what-is-stegonagraphy/)
- [picoCTF Forensics Challenges](https://picoctf.org)