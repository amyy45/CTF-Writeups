# 🧩 CTF Challenge Writeup  
**Challenge Name:** Blame Game  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** *06-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a git repository with the following description:

> *"Someone's commits seems to be preventing the program from working. Who is it?"*

A `challenge.zip` file is provided containing a git repository with many commits.  
The challenge name **"Blame Game"** directly hints at `git blame` — the git command used to find who last modified each line of a file.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- Tags include **git** — git forensics challenge
- Challenge name = `git blame` command
- Description asks **"Who is it?"** — looking for an author name

Key observations:
- `git blame` shows the author, commit hash, and timestamp for every line in a file
- The repo has 50+ commits all named "important business work" — suspicious and unhelpful
- The flag was hidden as the **commit author's name** in the git blame output
- `git blame` is typically used to find who introduced a bug — here it reveals the flag

The intended approach is to run **`git blame`** on the Python file and read the author name column.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract
```bash
wget https://artifacts.picoctf.net/c_titan/159/challenge.zip
unzip challenge.zip  # type A if prompted
cd drop-in
```

### 3.2 Check Commit History
```bash
git log --oneline
```

Output shows 50+ commits all named "important business work" — no flag in messages.

### 3.3 Run git blame
```bash
git blame message.py
```

Output:
```
23e9d4ce (picoCTF{@sk_th3_1nt3rn_81e716ff} 2024-03-12 00:07:15 +0000 1) print("Hello, World!"
```

The **author name field** in git blame contains the flag!

---

## 🧩 4. Final Flag

```
picoCTF{@sk_th3_1nt3rn_81e716ff}
```

---

## 📚 5. Key Learnings

- **`git blame <file>`** shows who last modified each line — format is: `<hash> (<author> <date> <line_number>) <content>`
- The **author name** in git is just a string set by `git config user.name` — it can be set to anything, including a flag!
- In real-world security, `git blame` is used to trace who introduced vulnerable code or accidentally committed secrets
- All 50+ commit messages being identical ("important business work") was a red herring to slow you down — always check `git blame` when the challenge name hints at it
- The flag was not in the commit message or file content — it was in the **metadata** (author field) of the commit

---

## 🚀 6. Improvements for Next Time

- Challenge name = git command — **Blame Game** = `git blame`, **Time Machine** = `git log`, **Commitment Issues** = `git show`
- When all commit messages look the same, skip `git log` and go straight to `git blame`
- `git blame` output format: `hash (Author Date Time Timezone LineNum) content`
- Useful git blame variations:
  ```bash
  git blame message.py          # blame entire file
  git blame -L 1,10 message.py  # blame only lines 1-10
  git log --all --format="%an"  # list all author names across all commits
  ```
- Always check **all metadata fields** in git: commit message, author name, author email, date

---

## 🔗 7. References

- [Git Blame Documentation](https://git-scm.com/docs/git-blame)
- [Git Book – Git Blame](https://git-scm.com/book/en/v2/Git-Tools-Debugging-with-Git)
- [picoCTF General Skills Challenges](https://picoctf.org)