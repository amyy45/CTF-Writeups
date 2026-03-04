# 🧩 CTF Challenge Writeup  
**Challenge Name:** Time Machine  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** *04-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a git repository with the following description:

> *"What was I last working on? I remember writing a note to help me remember..."*

A `challenge.zip` file is provided containing a git repository.  
The hint says **"When committing a file with git, a message can (and should) be included"** — directly pointing to **git commit messages** as the hiding spot.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- Tags include **git** — the challenge is about git forensics
- Description mentions "writing a note" — git commit messages are notes

Key observations:
- The zip contains a `.git` folder — a full git repository
- Git stores the entire history of a project including **commit messages**
- `git log` shows all commits with their messages, authors, and timestamps
- The flag was embedded directly in a **commit message**

The intended approach is to **extract the zip, navigate into the repo, and run `git log`** to read commit history.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract
```bash
wget https://artifacts.picoctf.net/c_titan/68/challenge.zip
unzip challenge.zip
cd drop-in
```

### 3.2 View Git Commit History
```bash
git log
```

Output:
```
commit 705ff639b7846418603a3272ab54536e01e3dc43 (HEAD -> master)
Author: picoCTF <ops@picoctf.com>
Date:   Sat Mar 9 21:10:36 2024 +0000

    picoCTF{t1m3m@ch1n3_b476ca06}
```

The flag is right there in the commit message! Press `q` to exit the log view.

---

## 🧩 4. Final Flag

```
picoCTF{t1m3m@ch1n3_b476ca06}
```

---

## 📚 5. Key Learnings

- **Git commit messages** are stored permanently in the repository history and can contain sensitive information
- `git log` shows the full commit history including messages, authors, timestamps, and commit hashes
- A `.git` folder inside any directory means it's a git repository — always check `git log` in forensics challenges with git tag
- In real-world security, developers sometimes accidentally commit secrets (API keys, passwords, flags) in commit messages or file contents — this is a common source of data leaks
- `git log --oneline` gives a compact view of all commits — useful for scanning many commits quickly

---

## 🚀 6. Improvements for Next Time

- For any challenge tagged **git**, immediately run these commands:
  ```bash
  git log                  # full commit history with messages
  git log --oneline        # compact view
  git log --all            # include all branches
  git show <commit-hash>   # see full diff of a commit
  git diff HEAD~1          # compare with previous commit
  ```
- Check **all branches** with `git branch -a` — flags can be hidden on other branches
- Check **file contents at past commits** with `git show <hash>:filename`
- In real CTFs, git repos can have many commits — use `git log --oneline | grep pico` to search quickly

---

## 🔗 7. References

- [Git Log Documentation](https://git-scm.com/docs/git-log)
- [Git Book – Viewing Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
- [GitLeaks – Scanning for Secrets in Git](https://github.com/gitleaks/gitleaks)
- [picoCTF General Skills Challenges](https://picoctf.org)