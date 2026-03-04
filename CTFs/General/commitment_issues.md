# 🧩 CTF Challenge Writeup  
**Challenge Name:** Commitment Issues  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** *04-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a git repository with the following description:

> *"I accidentally wrote the flag down. Good thing I deleted it!"*

A `challenge.zip` file is provided containing a git repository.  
The description is ironic — **git never truly deletes anything**. Even "deleted" content lives forever in commit history.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- Tags include **git** — git forensics challenge
- Description says the flag was **deleted** — but git stores full history

Key observations:
- Git stores every version of every file ever committed — deletions only remove the file from the current state, not from history
- `git log` shows all commits including ones that deleted files
- `git show <commit-hash>` shows the full diff of what was added or removed in that commit
- The flag was added in one commit and removed in the next — `git show` on the earlier commit reveals it

The intended approach is to **find the commit that added the flag** and use `git show` to read its contents.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract
```bash
wget https://artifacts.picoctf.net/c_titan/136/challenge.zip
unzip challenge.zip  # type A if prompted to replace files
cd drop-in
```

### 3.2 View Commit History
```bash
git log --oneline
```

Output:
```
8dc5180 (HEAD -> master) remove sensitive info
87b85d7 create flag
```

Two commits — the flag was added in `87b85d7` and removed in `8dc5180`.

### 3.3 View the Flag Commit
```bash
git show 87b85d7
```

Output:
```diff
commit 87b85d7dfb839b077678611280fa023d76e017b8
Author: picoCTF <ops@picoctf.com>
Date:   Tue Mar 12 00:06:17 2024 +0000

    create flag

diff --git a/message.txt b/message.txt
new file mode 100644
index 0000000..bae247d
--- /dev/null
+++ b/message.txt
@@ -0,0 +1 @@
+picoCTF{s@n1t1z3_ea83ff2a}
```

The `+` line shows what was **added** in that commit — the flag! Press `q` to exit.

---

## 🧩 4. Final Flag

```
picoCTF{s@n1t1z3_ea83ff2a}
```

---

## 📚 5. Key Learnings

- **Git never truly deletes data** — deleted files and content remain accessible in commit history forever (unless history is rewritten with `git rebase` or `git filter-branch`)
- `git log --oneline` gives a compact view of all commits — great for spotting suspicious commit messages like "remove sensitive info"
- `git show <hash>` shows the full diff of a commit — lines starting with `+` were added, lines with `-` were removed
- This is a **real-world security issue** — developers often accidentally commit API keys, passwords, or secrets, then delete them thinking they're gone. The history still contains them!
- Tools like **GitLeaks** and **truffleHog** scan git history specifically for accidentally committed secrets

---

## 🚀 6. Improvements for Next Time

- For git forensics challenges always run these commands:
  ```bash
  git log --oneline          # see all commits
  git log --all --oneline    # include all branches
  git show <hash>            # see what a commit added/removed
  git diff HEAD~1            # compare current with previous commit
  git stash list             # check for stashed changes
  ```
- Look for suspicious commit messages like "remove sensitive info", "delete credentials", "oops", "fix"
- In real bug bounty work, always check public git repos for accidentally committed secrets in history
- `git log -p` shows all commits with their full diffs at once — useful for searching through many commits

---

## 🔗 7. References

- [Git Show Documentation](https://git-scm.com/docs/git-show)
- [Git Book – Viewing Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
- [GitLeaks – Secret Scanner](https://github.com/gitleaks/gitleaks)
- [TruffleHog – Git Secret Finder](https://github.com/trufflesecurity/trufflehog)
- [picoCTF General Skills Challenges](https://picoctf.org)