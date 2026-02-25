# 🧩 CTF Challenge Writeup  
**Challenge Name:** Scan Surprise  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** 25-02-26  

---

## 🔍 1. Challenge Description

> *"I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?"*

Two methods of access are provided:
- **Download:** `challenge.zip` containing the challenge files
- **SSH:** `ssh -p 60379 ctf-player@atlas.picoctf.net` with password `84b12bae`

The hint states: *"QR codes are a way of encoding data. While they're most known for storing URLs, they can store other things too."*

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- Tags: `qr_code`, `shell`, `browser_webshell_solvable`
- The flag is hidden inside a **QR code image** instead of being given as plaintext

Key observations:
- The challenge delivers the flag as a **PNG image** containing a QR code
- Standard `strings` or `grep` won't work on image files
- A **QR code decoder** tool is needed to extract the encoded data
- `zbarimg` (from the `zbar-tools` package) can decode QR codes directly from image files in the terminal
- The dbus connection warning from `zbarimg` is harmless and does not affect the result

The intended approach is to **SSH into the instance**, find the QR code image, and **decode it using `zbarimg`**.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Challenge Files (Optional)
```bash
wget https://artifacts.picoctf.net/c_atlas/14/challenge.zip
```

Output:
```
challenge.zip saved [742/742]
```

### 3.2 SSH Into the Instance
```bash
ssh -p 60379 ctf-player@atlas.picoctf.net
# Accept the fingerprint with: yes
# Password: 84b12bae
```

Upon login, the terminal renders the QR code visually in ASCII art — but it cannot be scanned from a terminal directly.

### 3.3 List Available Files
```bash
ls
```

Output:
```
flag.png
```

There is a single file: `flag.png` — a QR code image containing the flag.

### 3.4 Decode the QR Code with zbarimg
```bash
zbarimg flag.png
```

Output:
```
Connection Error (Failed to connect to socket /var/run/dbus/system_bus_socket: No such file or directory)
Connection Null
QR-Code:picoCTF{p33k_@_b00_0194a007}
scanned 1 barcode symbols from 1 images in 0 seconds
```

> ⚠️ The `Connection Error` regarding dbus is a harmless system warning and does **not** affect the result. `zbarimg` still successfully decodes the QR code.

---

## 🧩 4. Final Flag

```
picoCTF{p33k_@_b00_0194a007}
```

---

## 📚 5. Key Learnings

- Flags can be **encoded in images** (QR codes, steganography, etc.) — not just plain text files
- `zbarimg` is a powerful command-line tool to decode **QR codes and barcodes** directly from image files without needing a phone or GUI
- dbus-related `Connection Error` warnings from `zbarimg` are **harmless** — the tool still works correctly
- The terminal rendered the QR code visually in ASCII on login, but it was easier to decode programmatically with `zbarimg`
- QR codes can encode **any arbitrary string**, not just URLs — including CTF flags
- Always check what tools are available on a remote shell (`which zbarimg`, `which python3`, etc.) before resorting to file transfer

---

## 🚀 6. Improvements for Next Time

- For image-based challenges, immediately try `zbarimg <file>` if the image looks like a QR code
- If `zbarimg` is unavailable, use Python with `pyzbar`:
  ```python
  from PIL import Image
  from pyzbar.pyzbar import decode
  print(decode(Image.open('flag.png')))
  ```
- Alternatively, transfer the file locally with `scp` and scan with a phone or online decoder:
  ```bash
  scp -P 60379 ctf-player@atlas.picoctf.net:~/drop-in/flag.png .
  ```
- For files with visual ASCII art QR codes in the terminal, a programmatic decode is always more reliable than trying to scan the screen

---

## 🔗 7. References

- [zbar-tools Documentation](http://zbar.sourceforge.net/)
- [pyzbar Python Library](https://pypi.org/project/pyzbar/)
- [QR Code – Wikipedia](https://en.wikipedia.org/wiki/QR_code)
- [picoCTF Forensics Challenges](https://picoctf.org)