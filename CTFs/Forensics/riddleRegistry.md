# 🧩 CTF Challenge Writeup  
**Challenge Name:** Riddle Registry  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoMini by CMU-Africa  
**Author:** PRINCE NIYONSHUTI N.  
**Date:** 23-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a PDF file with the following description:

> *"Hi, intrepid investigator! You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered."*

A PDF file called `confidential.pdf` is provided.  
The challenge explicitly states: **"uncover the flag within the metadata"** — meaning the flag is not in the visible content of the PDF but hidden in its metadata fields.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category  
- **Easy** difficulty  
- A PDF file with "garbled nonsense" as visible content
- Challenge explicitly mentions **metadata**

Key observations:
- PDF files contain metadata fields like `Author`, `Title`, `Subject`, `Keywords`, `Producer`, etc.
- These fields are not visible when viewing the PDF normally
- `exiftool` is the standard tool for extracting file metadata
- The flag could be hidden directly or encoded (e.g., Base64) in a metadata field
- The `Author` field often contains hidden data in CTF challenges

The intended approach is to **extract PDF metadata using exiftool** and decode any encoded values found.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the PDF
Download `confidential.pdf` from the challenge link.

### 3.2 Extract Metadata Using exiftool
```bash
exiftool confidential.pdf
```

Output reveals:
```
ExifTool Version Number  : 12.40
File Name                : confidential.pdf
File Size                : 178 KiB
PDF Version              : 1.7
Page Count               : 1
Producer                 : PyPDF2
Author                   : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOGY5MWQ2OH0=
```

The **Author** field contains a suspicious Base64-encoded string.

### 3.3 Decode the Base64 Value
```bash
exiftool confidential.pdf | grep Author | awk -F': ' '{print $2}' | base64 -d ; echo
```

Or manually:
```bash
echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jOGY5MWQ2OH0=" | base64 -d ; echo
```

Output:
```
picoCTF{puzzl3d_m3tadata_f0und!_c8f91d68}
```

The flag was Base64 encoded and hidden in the `Author` metadata field! ✅

---

## 🧩 4. Final Flag

```
picoCTF{puzzl3d_m3tadata_f0und!_c8f91d68}
```

---

## 📚 5. Key Learnings

- **PDF metadata fields** like `Author`, `Title`, `Subject`, and `Keywords` can hide data invisible to normal viewers
- `exiftool` is the go-to tool for extracting metadata from any file type (PDF, images, documents, etc.)
- Flags in metadata are often **Base64 encoded** — always try decoding suspicious strings
- The `Producer` field showed `PyPDF2` — confirming the file was created programmatically, which is a clue that metadata was intentionally set
- Always add `; echo` after `base64 -d` to force a newline — otherwise the output blends into the shell prompt and looks empty
- The visible content of a file is not the only place to hide information — metadata, steganography, and appended data are common forensics hiding spots

---

## 🚀 6. Improvements for Next Time

- For any forensics challenge involving a file, **always run `exiftool` first** — it's fast and reveals metadata instantly
- When you see a long string of random-looking characters in metadata, **immediately try Base64 decoding**
- Use `grep picoCTF` after `strings` to quickly search file content:
  ```bash
  strings confidential.pdf | grep picoCTF
  ```
- Always add `; echo` or `echo ""` after `base64 -d` to ensure the output is visible on its own line
- Check ALL metadata fields — not just `Author` — including `Title`, `Subject`, `Keywords`, and custom fields

---

## 🔗 7. References

- [ExifTool Documentation](https://exiftool.org/)
- [Base64 Encoding – MDN Web Docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
- [PDF Metadata Structure](https://opensource.adobe.com/dc-acrobat-sdk-docs/standards/pdfstandards/pdf/PDF32000_2008.pdf)
- [OWASP – Metadata Leakage](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage)
- [picoCTF Forensics Challenges](https://picoctf.org)