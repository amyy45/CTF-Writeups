# 🧩 CTF Challenge Writeup  
**Challenge Name:** DISKO 1  
**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoGym Exclusive  
**Author:** DARKRAICG492  
**Date:** 23-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a disk image file with the following description:

> *"Can you find the flag in this disk image? Download the disk image here."*

A compressed disk image file `disko-1.dd.gz` is provided for download.  
The hint **"Maybe Strings could help? If only there was a way to do that?"** directly points to the `strings` command as the primary tool needed.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Forensics** category  
- **Easy** difficulty  
- A compressed raw disk image (`.dd.gz`)
- Hint explicitly references the `strings` utility

Key observations:
- Raw disk images (`.dd`) are binary files that can contain readable text scattered throughout
- The `strings` command extracts all human-readable ASCII sequences from any binary file
- Flags in picoCTF follow the format `picoCTF{...}`, making them easy to grep for
- The file is gzip-compressed, so it must be decompressed before analysis

The intended approach is straightforward: **decompress → strings → grep for flag**.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Disk Image
```bash
wget https://artifacts.picoctf.net/c/537/disko-1.dd.gz
```

### 3.2 Decompress the File
```bash
gunzip disko-1.dd.gz
```

This produces the raw disk image `disko-1.dd`.

### 3.3 Extract Readable Strings and Search for the Flag
```bash
strings disko-1.dd | grep "picoCTF"
```

Output:
```
picoCTF{1t5_ju5t_4_5tr1n9_be6031da}
```

The flag was embedded as a plaintext string directly inside the disk image data.

---

## 🧩 4. Final Flag
```
picoCTF{1t5_ju5t_4_5tr1n9_be6031da}
```

---

## 📚 5. Key Learnings

- **Raw disk images** are binary files that often contain plaintext data embedded within them — the `strings` command is always a great first step
- Always **decompress** archived files (`.gz`, `.zip`, etc.) before attempting analysis
- The `strings` command extracts all printable character sequences from any binary file — an essential tool for forensics challenges
- Piping `strings` output into `grep` with the known flag format (`picoCTF`) instantly narrows down results even in large binary files
- The flag name itself is a hint — *"1t5_ju5t_4_5tr1n9"* translates to "it's just a string", confirming the intended method

---

## 🚀 6. Improvements for Next Time

- For any disk image forensics challenge: start with `strings` and `grep`, then escalate to `binwalk`, `file`, or `autopsy` if needed
- Check the file type with `file <filename>` before doing anything — it helps choose the right tools
- Other useful tools for disk image forensics: `binwalk` (embedded files), `foremost` (file carving), `autopsy` / `sleuthkit` (full filesystem analysis)
- Raw disk images (`.dd`) can also be mounted directly: `mount -o loop disko-1.dd /mnt/point` to browse the filesystem

---

## 🔗 7. References

- [GNU Binutils – strings documentation](https://www.gnu.org/software/binutils/)
- [The Sleuth Kit – Disk Image Forensics](https://www.sleuthkit.org/)
- [CTF Forensics Techniques](https://ctf101.org/forensics/overview/)
- [picoCTF Forensics Challenges](https://picoctf.org)