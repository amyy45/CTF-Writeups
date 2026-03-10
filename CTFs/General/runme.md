# 🧩 CTF Challenge Writeup

**Challenge Name:** runme.py  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** Beginner picoMini 2022  
**Author:** SUJEET KUMAR  
**Date:** *10-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a Python script with the following description:

> *"Run the runme.py script to get the flag. Download the script with your browser or with wget in the webshell."*

A Python script `runme.py` is provided for download. There are 3 hints available.

The objective is simply to download and execute the Python script to retrieve the flag.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- A Python script to download and run
- Challenge description is direct — just run the script

Key observations:
- The challenge name itself is the instruction: **run me**
- No decoding, no steganography, no analysis needed
- Simply downloading and executing the script with `python3` reveals the flag
- This is a beginner-level challenge testing basic command line and Python execution skills

The intended approach is: **Download script → run with python3 → get flag.**

---

## 🛠️ 3. Steps to Solve

### 3.1 Download the Script
```bash
wget https://artifacts.picoctf.net/c/34/runme.py
```

Output:
```
runme.py saved [270/270]
```

### 3.2 Run the Script
```bash
python3 runme.py
```

Output:
```
picoCTF{run_s4n1ty_run}
```

The flag is printed directly to the terminal. ✅

---

## 🧩 4. Final Flag

```
picoCTF{run_s4n1ty_run}
```

---

## 📚 5. Key Learnings

- Not every CTF challenge requires complex tools — sometimes just **running the file** is enough
- Always read the challenge description carefully — the solution is often stated directly
- `python3 script.py` is the standard way to execute a Python script in Linux
- `wget <url>` is a quick way to download files directly to the webshell
- This type of challenge tests basic familiarity with the **Linux command line** and **Python execution**

---

## 🚀 6. Improvements for Next Time

- Before over-thinking a challenge, **read the description carefully** — it may tell you exactly what to do
- Always check the file type before running:
  ```bash
  file runme.py
  cat runme.py
  ```
- Inspect the script contents first to understand what it does:
  ```bash
  cat runme.py
  ```
- If `python3` is not available, try `python`:
  ```bash
  python runme.py
  ```
- For any script-based challenge: **download → inspect → run**

---

## 🔗 7. References

- [Python3 Documentation](https://docs.python.org/3/)
- [Linux wget man page](https://linux.die.net/man/1/wget)
- [picoCTF General Skills Challenges](https://picoctf.org)
- [CTF101 – General Skills](https://ctf101.org/)