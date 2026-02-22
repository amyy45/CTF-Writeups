# 🧩 CTF Challenge Writeup  
**Challenge Name:** Flag Hunters  
**Category:** Reverse Engineering  
**Difficulty:** Easy  
**Platform:** picoCTF 2025  
**Author:** SYREAL  
**Date:** 22-02-26  

---

## 🔍 1. Challenge Description

The challenge presents a program with the following description:

> *"Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print? There might be something in it for you."*

The program's source code is provided for download, and a netcat connection is given to interact with the running instance.  
The challenge name **Flag Hunters** and the hint **"Unsanitized user input is always good, right?"** strongly suggest that user input can be injected to alter the program's execution flow.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **Reverse Engineering** category  
- **Easy** difficulty  
- Source code is provided — no black-box guessing needed
- The program is interactive via netcat
- Hint: **"Unsanitized user input is always good, right?"**

Key observations:
- The program is a Python "lyric reader" that executes commands like `REFRAIN`, `RETURN`, and `END` embedded in the song string
- The flag is stored in `secret_intro` at the very beginning of the song string — **line 3** (0-indexed)
- The program starts reading from `[VERSE1]`, skipping `secret_intro` entirely
- A `CROWD` command takes user input and stores it back into `song_lines[lip]`
- The line is split by `;` before processing — meaning injecting `;COMMAND` in user input can execute arbitrary commands
- `MAX_LINES = 100` limits total execution, so the injection must happen efficiently

The intended approach is to **inject a `;RETURN 3` command** via the `Crowd:` prompt to jump to the flag line.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Read the Source Code
Download `lyric-reader.py` and read it carefully:

```python
elif re.match(r"CROWD.*", line):
    crowd = input('Crowd: ')
    song_lines[lip] = 'Crowd: ' + crowd  # user input stored back into song
    lip += 1
```

And the line processing loop:
```python
for line in song_lines[lip].split(';'):  # split by semicolon first!
```

This means if user input contains `;RETURN 3`, after storing `Crowd: ;RETURN 3`, when the `for` loop splits by `;`, it produces `['Crowd: ', 'RETURN 3']` — and `RETURN 3` gets executed as a real command!

### 3.2 Find the Flag Line Number
The song string starts with `secret_intro`:

```
line 0: Pico warriors rising, puzzles laid bare,
line 1: Solving each challenge with precision and flair.
line 2: With unity and skill, flags we deliver,
line 3: The ether's ours to conquer, picoCTF{...}
```

So the flag is at **line index 3**.

### 3.3 Connect via Netcat
```bash
nc verbal-sleep.picoctf.net 61434
```

### 3.4 Wait for the First `Crowd:` Prompt
The song plays through the first verse and hits the REFRAIN, which contains:
```
CROWD (Singalong here!);
RETURN
```

When `Crowd:` appears, type exactly:
```
;RETURN 3
```

### 3.5 Read the Flag
The program processes `;RETURN 3` from the split line, jumps to line 3, and prints:
```
The ether's ours to conquer, picoCTF{70637h3r_f0r3v3r_b248b032}
```

The flag keeps printing in a loop until `MAX_LINES` is exhausted — confirming the injection worked! ✅

---

## 🧩 4. Final Flag

```
picoCTF{70637h3r_f0r3v3r_b248b032}
```

---

## 📚 5. Key Learnings

- **Always read the source code carefully** — the vulnerability was visible directly in the code
- **Semicolons as command separators** are dangerous when user input is placed back into a line that gets split by `;`
- The `for line in song_lines[lip].split(';')` pattern means injecting `;COMMAND` in user input executes that command in the same iteration
- **Unsanitized user input** that gets stored and re-processed can alter program control flow
- `MAX_LINES` limits made it important to inject on the **first** `Crowd:` prompt to avoid running out of iterations
- The flag was hidden at the very start of the song string — the program deliberately skipped it by starting at `[VERSE1]`

---

## 🚀 6. Improvements for Next Time

- Read the source code **top to bottom** before connecting — identify where the flag is and what commands are available
- Look for any place where **user input is stored back into a data structure** that gets re-processed
- Check for **delimiter injection** (`;`, `|`, `\n`) whenever input is embedded into a command string
- Count the exact **line number** of the flag before connecting so you know exactly what to inject
- When `MAX_LINES` exists, always try to solve in the **fewest possible interactions**

---

## 🔗 7. References

- [OWASP – Input Validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [CWE-78 – Improper Neutralization of Special Elements](https://cwe.mitre.org/data/definitions/78.html)
- [Python re.match() Documentation](https://docs.python.org/3/library/re.html#re.match)
- [picoCTF Reverse Engineering Challenges](https://picoctf.org)