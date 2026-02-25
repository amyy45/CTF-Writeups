# 🧩 CTF Challenge Writeup  
**Challenge Name:** Verify  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** 25-02-26  

---

## 🔍 1. Challenge Description

> *"People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate."*

An SSH instance is provided along with:
- **SSH command:** `ssh -p 51376 ctf-player@rhea.picoctf.net`
- **Password:** `6abf4a82`
- **Checksum:** `b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2`
- **Hint:** Run `./decrypt.sh files/<file>` once the correct file is verified

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category
- **Easy** difficulty
- A remote shell with a `files/` directory containing many candidate files
- A known **SHA-256 checksum** to identify the legitimate flag file
- A `decrypt.sh` script to decrypt the verified file

Key observations:
- Many fake flag files are planted in the `files/` directory as decoys
- Only **one file** matches the provided SHA-256 checksum
- `sha256sum` + `grep` can quickly identify the correct file out of all candidates
- Once identified, running `./decrypt.sh` on it reveals the real flag

The intended approach is to **verify file integrity using SHA-256**, then decrypt the legitimate file.

---

## 🛠️ 3. Steps to Solve

### 3.1 SSH Into the Instance
```bash
ssh -p 51376 ctf-player@rhea.picoctf.net
# Accept the fingerprint with: yes
# Password: 6abf4a82
```

### 3.2 List Available Files
```bash
ls
```
Output:
```
checksum.txt  decrypt.sh  files
```

There is a `files/` directory containing many candidate files, a `checksum.txt`, and a `decrypt.sh` decrypt script.

### 3.3 Find the File Matching the SHA-256 Checksum
```bash
sha256sum files/* | grep b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2
```

Output:
```
b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2  files/451fd69b
```

The file `files/451fd69b` is the only one whose hash matches — this is the legitimate flag file.

### 3.4 Decrypt the Verified File
```bash
./decrypt.sh files/451fd69b
```

Output:
```
picoCTF{trust_but_verify_451fd69b}
```

---

## 🧩 4. Final Flag

```
picoCTF{trust_but_verify_451fd69b}
```

---

## 📚 5. Key Learnings

- **SHA-256 checksums** are a reliable way to verify file integrity — if the hash matches, the file is authentic
- `sha256sum files/* | grep <hash>` is an efficient one-liner to find a matching file among many candidates
- CTF challenges often plant many decoy/fake files to make brute-force reading impractical — hash verification cuts through the noise instantly
- The challenge name **"Verify"** and flag text **"trust_but_verify"** reinforce the real-world lesson: always verify what you receive before trusting it
- `decrypt.sh` scripts in CTFs usually wrap standard tools like `openssl` — understanding what's inside can help in harder variants

---

## 🚀 6. Improvements for Next Time

- Always check `checksum.txt` first — it may already contain the hash to match against
- When given a hash and many files, reach for `sha256sum files/* | grep <hash>` immediately
- Inspect `decrypt.sh` with `cat decrypt.sh` to understand the decryption method used
- For harder versions of this challenge type, the correct file might be nested in subdirectories — use `find` + `sha256sum` together
- Practice recognizing SHA-256 hashes (64 hex characters) vs MD5 (32) and SHA-1 (40)

---

## 🔗 7. References

- [sha256sum Man Page](https://man7.org/linux/man-pages/man1/sha256sum.1.html)
- [OpenSSL Documentation](https://www.openssl.org/docs/)
- [picoCTF Forensics Challenges](https://picoctf.org)
- [File Integrity Verification – Wikipedia](https://en.wikipedia.org/wiki/File_verification)