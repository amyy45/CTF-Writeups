# 🧩 CTF Challenge Writeup  
**Challenge Name:** Log Hunt  
**Category:**  Forensics 
**Difficulty:**  Easy 
**Platform:**  picoCTF 
**Date:**  *04-12-25*

---

## 🔍 1. Challenge Description
This challenge is a **short interactive terminal-based game** designed to introduce players to picoCTF rules, good practices, and basic competition guidelines.  
You interact with a character named *Eibhilin* and her AI companion *Nyx* while making choices related to CTF ethics, registration, and gameplay.

The objective is simple:  
➡️ **Play through the simulation and retrieve the flag at the end.**

---

## 🧠 2. Initial Thoughts / Approach
Since this is under the *intro/misc* category, I expected:

- A fully guided, story-like experience  
- Simple keypress interactions  
- Basic teaching on CTF etiquette:  
  - Do not share flags  
  - Do not create multiple accounts  
  - Start with “sanity check” challenges  
- The flag would be revealed automatically at the end

This challenge teaches rules rather than requiring technical exploitation.


---

## 🛠️ 3. Steps to Solve

### 1. Connect to the challenge instance
```bash
nc verbal-sleep.picoctf.net 51859
```
### 2. Play through the interactive story
The terminal displays a narrative where you must press Enter repeatedly to continue.

Important decision points:

Choosing account type
```csharp
[A] Register multiple accounts
[B] Share an account
[C] Register a single private account  ← Correct
```
Choosing how to find the flag
```scss
[A] Play the game  ← Correct
[B] Search the Ether for the flag (not allowed)
```
### 3. Completing the “sanity” challenge
The in-game bar loads to 100%:
```yaml
Playing the Game: 100%
Playing the Game completed successfully!
```
### 4. The game reveals the final flag
At the end, Nyx and Eibhilin confirm that you solved the challenge and display the flag.

---
## 🧩  4. Final Flag 
```bash
picoCTF{m1113n1um_3d1710n_f71e4e49}
```

---
## 📚 5. Key Learnings
- Ethical rules of CTF participation:  
  - Do not share accounts 
  - Do not share flags  
  - Do not use multiple accountss  
- Starting with “sanity challenges” helps orient beginners
- Some CTF tasks are designed purely to teach process, not exploitation
- Importance of reading instructions and selecting correct options

---
## 🚀 6. Improvements for Next Time
- Explore more interactive terminal challenges
- Automate netcat session logging (useful for writeups)
- Practice recognizing ethical traps intentionally placed in intro tasks

---
## 🔗 7. References
- picoCTF Official Documentation
- Unix nc (netcat) manual