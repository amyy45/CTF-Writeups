# 🧩 CTF Challenge Writeup  
**Challenge Name:** information  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2021  
**Author:** SUSIE  
**Date:** 2026-02-26  

---

## 🔍 1. Challenge Description

> *"Files can always be changed in a secret way. Can you find the flag?"*

A single image file `cat.jpg` is provided for download.  
The hint states: **"Look at the details of the file"** — directly pointing toward **file metadata** as the hiding spot.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- A single JPEG image file
- Hint explicitly says to look at "details of the file" → EXIF/XMP metadata

Key observations:
- The image is a normal-looking cat photo — nothing suspicious visually
- JPEG files store **EXIF and XMP metadata** which can contain arbitrary fields
- `exiftool` reveals all metadata fields including non-standard ones
- The flag was Base64-encoded and hidden in the **`License`** XMP metadata field
- Base64 decoding the value directly reveals the flag

The intended approach is to **run exiftool on the image**, spot the suspicious Base64 value in a metadata field, and decode it.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the File
```bash
wget https://challenge-files.picoctf.net/c_wily_courier/<hash>/cat.jpg
```

### 3.2 Inspect Metadata with exiftool
```bash
exiftool cat.jpg
```

Output (relevant portion):
```
XMP Toolkit     : Image::ExifTool 10.80
License         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights          : PicoCTF
Copyright Notice: PicoCTF
```

The `License` field contains a suspicious **Base64-encoded string** instead of a normal license URL or name — this immediately stands out as the hidden data.

### 3.3 Decode the Base64 String
```bash
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
```

Output:
```
picoCTF{the_m3tadata_1s_modified}
```

---

## 🧩 4. Final Flag

```
picoCTF{the_m3tadata_1s_modified}
```

---

## 📚 5. Key Learnings

- **EXIF/XMP metadata** is a common hiding place for CTF flags — always inspect it with `exiftool` on any image file
- The `License` field is an XMP metadata tag that normally stores licensing information — abusing it to store Base64-encoded data is a simple but effective hiding technique
- **Base64** strings are easy to spot: they use only `A-Z`, `a-z`, `0-9`, `+`, `/` characters and often end with `=` or `==` padding (though not always)
- The flag `the_m3tadata_1s_modified` confirms the intended lesson — metadata can be silently modified without changing the visible image
- This challenge is very similar to **CanYouSee** (picoCTF 2024) — both hide Base64-encoded flags in XMP metadata fields (`Attribution URL` vs `License`)
- In real-world forensics, metadata can reveal author names, GPS coordinates, software used, and timestamps — always examine it carefully

---

## 🚀 6. Improvements for Next Time

- **Always run `exiftool <file>` first** on any image in a forensics challenge — it's fast and often reveals the answer immediately
- Scan all metadata fields for values that don't match their expected format (e.g., a `License` field containing random-looking characters)
- Use `strings <file> | grep picoCTF` as a quick parallel check
- For deeper analysis after metadata, try `binwalk`, `steghide`, and `zsteg` in that order
- Recognize Base64 without `=` padding — not all Base64 strings end in `=`

---

## 🔗 7. References

- [ExifTool Documentation](https://exiftool.org/)
- [XMP Metadata Standard](https://www.adobe.com/products/xmp.html)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [JPEG EXIF Format – Wikipedia](https://en.wikipedia.org/wiki/Exif)
- [picoCTF Forensics Challenges](https://picoctf.org)