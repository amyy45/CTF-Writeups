# 🚩 CTF Writeups

A personal collection of CTF (Capture The Flag) challenge writeups documenting my journey through various cybersecurity competitions and platforms. Each writeup covers the thought process, tools used, and step-by-step solution.

---

## 👩‍💻 Author

**Sneha** — cybersecurity enthusiast documenting solutions across Web Exploitation, Forensics, Reverse Engineering, Cryptography, and more.

---

## 📁 Repository Structure

```
CTF-Writeups/
├── CTFs/
│   ├── Binary Exploitation/
│   ├── Cryptography/
│   ├── Forensics/
│   ├── General/
│   ├── Reverse Engineering/
│   ├── Web Exploitation/
├── HackTheBox/
├── TryHackMe/
└── Misc/
```

---

## 📚 Writeups Index

### 🌐 Web Exploitation

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [GET aHEAD](CTFs/Web%20Exploitation/get_aHead.md) | picoCTF 2021 | Easy | HTTP HEAD method |
| [dont-use-client-side](CTFs/Web%20Exploitation/dont_use_client_side.md) | picoCTF 2019 | Easy | Client-side JS analysis |
| [logon](CTFs/Web%20Exploitation/logon.md) | picoCTF 2019 | Easy | Cookie manipulation |
| [Crack the Gate 2](CTFs/Web%20Exploitation/crack_the_gate2.md) | picoMini CMU-Africa | Medium | XFF header bypass + brute force |
| [Bookmarklet](CTFs/Web%20Exploitation/Bookmarklet.md) | picoCTF | Easy | JavaScript bookmarklet |
| [Cookies](CTFs/Web%20Exploitation/Cookies.md) | picoCTF | Easy | Cookie manipulation |
| [Includes](CTFs/Web%20Exploitation/Includes.md) | picoCTF | Easy | File inclusion |
| [Inspect HTML](CTFs/Web%20Exploitation/InspectHTML.md) | picoCTF | Easy | HTML inspection |
| [Intro to Burp](CTFs/Web%20Exploitation/IntoToBurp.md) | picoCTF | Easy | Burp Suite |
| [Local Authority](CTFs/Web%20Exploitation/LocalAuthority.md) | picoCTF | Easy | Client-side auth |
| [SSTI 1](CTFs/Web%20Exploitation/SSTI1.md) | picoCTF | Easy | Server-side template injection |
| [Scavenger Hunt](CTFs/Web%20Exploitation/ScavengerHunt.md) | picoCTF 2021 | Easy | Hidden web resources |
| [Unminify](CTFs/Web%20Exploitation/Unminify.md) | picoCTF | Easy | JS deobfuscation |
| [Cookie Monster](CTFs/Web%20Exploitation/cookie_monster.md) | picoCTF | Easy | Cookie manipulation |
| [Crack the Gate 1](CTFs/Web%20Exploitation/crack_the_gate1.md) | picoMini CMU-Africa | Easy | Brute force |
| [Web Decode](CTFs/Web%20Exploitation/webDecode.md) | picoCTF | Easy | Encoding |

---

### 🔬 Forensics

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [Riddle Registry](CTFs/Forensics/riddleRegistry.md) | picoMini CMU-Africa | Easy | PDF metadata + Base64 |
| [Hidden in Plainsight](CTFs/Forensics/hiddenInPlaintext.md) | picoMini CMU-Africa | Easy | Image metadata + steghide |
| [RED](CTFs/Forensics/red.md) | picoCTF 2025 | Easy | RGBA channel LSB + Base64 |
| [Ph4nt0m 1ntrud3r](CTFs/Forensics/panthom_intruder.md) | picoCTF 2025 | Easy | PCAP analysis + timestamp ordering |
| [Disko 1](CTFs/Forensics/Disko1.md) | picoCTF | Easy | Disk forensics |
| [Corrupted File](CTFs/Forensics/corruptedFile.md) | picoCTF | Easy | File repair |

---

### 🔄 Reverse Engineering

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [Flag Hunters](CTFs/Reverse%20Engineering/flagHunter.md) | picoCTF 2025 | Easy | Delimiter injection |
| [Transformation](CTFs/Reverse%20Engineering/transformation.md) | picoCTF 2021 | Easy | Bit manipulation / Unicode encoding |

---

### 🔐 Cryptography

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [13](CTFs/Cryptography/13.md) | picoCTF | Easy | ROT13 |
| [RSA](CTFs/Cryptography/RSA.md) | picoCTF | Easy | RSA decryption |
| [Crack the Power](CTFs/Cryptography/crackThePower.md) | picoCTF | Easy | Weak exponent |
| [Hash Crack](CTFs/Cryptography/hashcrack.md) | picoCTF | Easy | Hash cracking |
| [interencdec](CTFs/Cryptography/interencdec.md) | picoCTF | Easy | Layered encoding |
| [The Numbers](CTFs/Cryptography/theNumbers.md) | picoCTF | Easy | Number-to-letter substitution |

---

### ⚙️ General

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [Log Hunt](CTFs/General/Log-hunt.md) | picoCTF | Easy | Log file analysis |

---

### 🎮 Misc

| Challenge | Platform | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [Fantasy CTF](Misc/fantasy-ctf.md) | Misc | Easy | Misc |

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| `curl` | HTTP request manipulation |
| `exiftool` | File metadata extraction |
| `steghide` | Image steganography extraction |
| `zsteg` | PNG steganography analysis |
| `tshark` / `dpkt` | PCAP network analysis |
| `binwalk` | Binary file analysis |
| `strings` | Extract readable strings from files |
| `base64` | Encoding/decoding |
| `Python (PIL/Pillow)` | Image pixel analysis |
| Browser DevTools | JS/cookie inspection |
| `Burp Suite` | Web traffic interception |

---

## 🧠 Key Concepts Covered

- HTTP methods (GET, POST, HEAD)
- Cookie and session manipulation
- Client-side authentication bypass
- SQL injection & authentication bypass
- IP spoofing via `X-Forwarded-For` header
- Password brute forcing
- PDF/image metadata analysis
- Steganography (steghide, LSB, RGBA channels)
- PCAP forensics and timestamp analysis
- Bit manipulation and Unicode encoding
- Base64 / ROT13 / RSA decryption
- Delimiter injection in interpreted programs

---

## 📝 Writeup Format

Each writeup follows a consistent format:

1. **Challenge Description** — what the challenge says
2. **Initial Thoughts / Approach** — how I approached it
3. **Steps to Solve** — detailed step-by-step walkthrough
4. **Final Flag** — the captured flag
5. **Key Learnings** — what the challenge taught me
6. **Improvements for Next Time** — how to solve faster next time
7. **References** — useful links and documentation

---

## 🚀 Platforms

- [picoCTF](https://picoctf.org) — beginner-friendly CTF by Carnegie Mellon University
- [HackTheBox](https://hackthebox.com) — intermediate/advanced machines and challenges
- [TryHackMe](https://tryhackme.com) — guided learning paths and rooms

---

## ⚠️ Disclaimer

All writeups are for **educational purposes only**. Challenges are from legal CTF platforms where solving and sharing solutions is permitted and encouraged.

---

⭐ *If you find these writeups helpful, consider starring the repo!*