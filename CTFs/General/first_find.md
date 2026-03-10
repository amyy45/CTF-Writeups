# 🧩 CTF Challenge Writeup

**Challenge Name:** First Find  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoGym Exclusive   
**Author:** LT 'SYREAL' JONES  
**Date:** *10-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a zip archive with the following description:

> *"Unzip this archive and find the file named 'uber-secret.txt'"*

A zip file `files.zip` is provided for download. There are no hints given.

The objective is to extract the zip archive, locate the hidden file `uber-secret.txt`, and retrieve the flag.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- A zip archive containing many files
- Target file is named `uber-secret.txt`
- No hints provided

Key observations:
- The zip file is **3.81MB** — large enough to contain many nested files
- Directly unzipping failed due to **disk space being full**
- `unzip -l` can list zip contents **without extracting** — useful for finding the target file path
- `unzip -p` pipes file contents directly to **stdout** — bypasses disk entirely
- The file was deeply nested inside hidden directories (`.secret/deeper_secrets/deepest_secrets/`)
- Freeing disk space by deleting unneeded files allowed targeted extraction

The intended solution is: **Download → list zip contents → locate target → extract only that file → read flag.**

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Zip File
```bash
wget https://artifacts.picoctf.net/c/500/files.zip
```

Output:
```
files.zip saved [3995553/3995553]
```

### 3.2 Attempt Full Extraction (Failed — Disk Full)
```bash
unzip files.zip
```

Output:
```
files/13771.txt.utf-8:  write error (disk full?).  Continue? (y/n/^C)
```

Disk space was exhausted. Press `Ctrl+C` to cancel.

### 3.3 List Zip Contents to Find Target File
```bash
unzip -l files.zip | grep uber-secret
```

Output:
```
31  2022-05-13 20:17   files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

Target file found deep inside hidden nested directories.

### 3.4 Free Up Disk Space
```bash
rm -rf ~/forensics/*
```

Deleted unneeded files from another directory to recover disk space.

### 3.5 Extract Only the Target File
```bash
unzip files.zip "*/uber-secret.txt"
```

Output:
```
extracting: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

### 3.6 Read the Flag
```bash
cat files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
```

Output:
```
picoCTF{f1nd_15_f457_ab443fd1}
```

---

## 🧩 4. Final Flag

```
picoCTF{f1nd_15_f457_ab443fd1}
```

---

## 📚 5. Key Learnings

- **`unzip -l`** lists all files inside a zip without extracting — essential for large archives
- **`unzip -p`** pipes a file directly to stdout with zero disk usage — useful when disk is full
- **`unzip files.zip "*/filename"`** extracts only a specific file using a wildcard path
- Hidden directories (starting with `.`) are commonly used in CTFs to obscure file locations
- Always check available disk space before extracting large archives:
  ```bash
  df -h
  ```
- Deeply nested paths like `.secret/deeper_secrets/deepest_secrets/` are a common CTF trick to make manual searching harder

---

## 🚀 6. Improvements for Next Time

- Before extracting, always **list the zip contents first**:
  ```bash
  unzip -l files.zip | less
  ```
- If disk is full, use **`unzip -p`** to read files without writing to disk:
  ```bash
  unzip -p files.zip "full/path/to/file.txt"
  ```
- Use `find` after extraction to locate hidden files:
  ```bash
  find . -name "uber-secret.txt"
  ```
- Always check for **hidden directories** (`.` prefix) when searching for files
- Use wildcards with `unzip` to extract specific files without knowing the full path:
  ```bash
  unzip files.zip "*/uber-secret.txt"
  ```

---

## 🔗 7. References

- [Linux unzip man page](https://linux.die.net/man/1/unzip)
- [Linux find command](https://linux.die.net/man/1/find)
- [picoCTF General Skills Challenges](https://picoctf.org)
- [CTF101 – Forensics Fundamentals](https://ctf101.org/forensics/overview/)