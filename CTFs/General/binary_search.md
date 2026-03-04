# 🧩 CTF Challenge Writeup  
**Challenge Name:** Binary Search  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** *04-03-26*   

---

## 🔍 1. Challenge Description

The challenge presents an interactive guessing game over SSH:

> *"Want to play a game? Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses."*

Connect via SSH, guess the correct number between 1 and 1000 in 10 guesses or fewer, and the server reveals the flag.

**Connection details:**
```
ssh -p 50654 ctf-player@atlas.picoctf.net
Password: 83dcefb7
```

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- 1000 possible numbers, only 10 guesses allowed
- The challenge is literally named **Binary Search**

Key observations:
- Binary search always finds a number in a sorted range of n values in at most **log₂(n)** guesses
- log₂(1000) ≈ 9.97 → 10 guesses is exactly enough to guarantee finding any number from 1-1000
- The hint says **"start around 500"** — confirming the binary search approach
- Always guess the **midpoint** of the current range, then halve the range based on the response

The intended approach is to apply **classic binary search**: start at 500, then narrow the range by half each guess.

---

## 🛠️ 3. Steps to Solve

### 3.1 Connect via SSH
```bash
ssh -p 50654 ctf-player@atlas.picoctf.net
# Type yes for fingerprint
# Password: 83dcefb7
```

### 3.2 Apply Binary Search

The algorithm:
- Start: `low = 1`, `high = 1000`
- Each guess: `mid = (low + high) / 2`
- If **Higher**: `low = mid + 1`
- If **Lower**: `high = mid - 1`
- If **Correct**: flag is printed!

| Guess | Value | Response | New Range |
|-------|-------|----------|-----------|
| 1 | 500 | Higher | 501–1000 |
| 2 | 750 | Higher | 751–1000 |
| 3 | 875 | Lower | 751–874 |
| 4 | **812** | ✅ Correct! | — |

### 3.3 Flag Printed
```
Congratulations! You guessed the correct number: 812
Here's your flag: picoCTF{g00d_gu355_ee8225d0}
```

---

## 🧩 4. Final Flag

```
picoCTF{g00d_gu355_ee8225d0}
```

---

## 📚 5. Key Learnings

- **Binary search** finds a value in a sorted range of n items in at most log₂(n) steps — for 1000 items that's 10 steps
- Always start at the **midpoint** (500 for 1–1000) — never guess randomly or sequentially
- Each guess **eliminates half** the remaining possibilities — this is what makes binary search so powerful
- The number changes every reconnection — if you run out of guesses, reconnect and start fresh at 500
- Binary search is fundamental to cybersecurity — used in log analysis, vulnerability scanning, and fuzzing

---

## 🚀 6. Improvements for Next Time

- **Never guess randomly** — always start at 500 for a 1–1000 range
- If you waste early guesses (like guessing 5 and 8), reconnect immediately — don't continue from a bad position unless the number happens to be found quickly
- Memorize the binary search template:
  ```
  low, high = 1, 1000
  while low <= high:
      mid = (low + high) // 2
      guess mid
      if higher: low = mid + 1
      if lower:  high = mid - 1
      if correct: done!
  ```
- You can also automate this with a Python script using `pwntools` for future similar challenges

---

## 🔗 7. References

- [Binary Search Algorithm – Wikipedia](https://en.wikipedia.org/wiki/Binary_search_algorithm)
- [Rust Book – Binary Search](https://doc.rust-lang.org/std/primitive.slice.html#method.binary_search)
- [pwntools Documentation](https://docs.pwntools.com/en/stable/)
- [picoCTF General Skills Challenges](https://picoctf.org)