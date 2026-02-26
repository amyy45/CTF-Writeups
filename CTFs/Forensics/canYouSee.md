# 🧩 CTF Challenge Writeup  
**Challenge Name:** CanYouSee  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** MUBARAK MIKAIL  
**Date:** 26-02-26  

---

## 🔍 1. Challenge Description

> *"How about some hide and seek?"*

A zip file is provided for download containing a suspicious image file.  
The hint states: **"If something isn't in the expected form, maybe it deserves attention?"** — strongly suggesting the flag is hidden in the file's **metadata**, not in the visible image content.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- A single image file inside a zip archive
- Hint points toward something "not in expected form" → metadata anomaly

Key observations:
- The file is a JPEG image (`ukn_reality.jpg`) — visually it appears normal
- JPEG files carry **EXIF metadata** that can store arbitrary key-value fields
- `exiftool` can read all metadata fields including non-standard ones
- The flag was Base64-encoded and stored in the `Attribution URL` XMP metadata field
- Base64 decoding the value directly reveals the flag

The intended approach is to **inspect EXIF/XMP metadata with exiftool**, find the suspicious Base64 value, and decode it.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Unzip the File
```bash
wget https://artifacts.picoctf.net/c_titan/5/unknown.zip
unzip unknown.zip
ls
```

Output:
```
ukn_reality.jpg  unknown.zip
```

### 3.2 Inspect Metadata with exiftool
```bash
exiftool ukn_reality.jpg
```

Output (relevant portion):
```
XMP Toolkit      : Image::ExifTool 11.88
Attribution URL  : cGljb0NURntNRTc0RDQ3QV9ISUREM05fNGRhYmRkY2J9Cg==
Image Width      : 4308
Image Height     : 2875
Megapixels       : 12.4
```

The `Attribution URL` field contains a suspicious **Base64-encoded string** — this is not a normal URL, which immediately stands out.

### 3.3 Decode the Base64 String
```bash
echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fNGRhYmRkY2J9Cg==" | base64 -d
```

Output:
```
picoCTF{ME74D47A_HIDD3N_4dabddcb}
```

---

## 🧩 4. Final Flag

```
picoCTF{ME74D47A_HIDD3N_4dabddcb}
```

---

## 📚 5. Key Learnings

- **EXIF/XMP metadata** in image files can carry hidden data — always inspect it with `exiftool` in forensics challenges
- The `Attribution URL` field is an XMP metadata tag — attackers (and CTF authors) can abuse any metadata field to hide data
- **Base64** is a common encoding used to disguise text data inside metadata fields — a string ending in `==` is a strong Base64 indicator
- The flag name `ME74D47A_HIDD3N` is a leet-speak version of "METADATA HIDDEN" — confirming the intended lesson
- `exiftool` is one of the most important tools in a forensics toolkit — it should be one of the first things to run on any image file
- Metadata is often overlooked by defenders, making it a convenient hiding spot for both CTF flags and real-world data exfiltration

---

## 🚀 6. Improvements for Next Time

- For any image challenge, run `exiftool <file>` immediately as a first step
- Look for any field with a value that doesn't match its expected format (e.g., a URL field containing a Base64 string)
- Use `strings <file> | grep picoCTF` as a quick secondary check alongside exiftool
- Remember the Base64 indicators: characters `A-Z`, `a-z`, `0-9`, `+`, `/`, and trailing `=` padding
- For deeper steganography, also try `steghide`, `binwalk`, and `zsteg` after checking metadata

---

## 🔗 7. References

- [ExifTool Documentation](https://exiftool.org/)
- [XMP Metadata Standard](https://www.adobe.com/products/xmp.html)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [JPEG EXIF Format – Wikipedia](https://en.wikipedia.org/wiki/Exif)
- [picoCTF Forensics Challenges](https://picoctf.org)