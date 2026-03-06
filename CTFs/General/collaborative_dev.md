# 🧩 CTF Challenge Writeup  
**Challenge Name:** Collaborative Development  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2024  
**Author:** JEFFERY JOHN  
**Date:** *06-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a git repository with multiple branches:

> *"My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?"*

A `challenge.zip` file is provided containing a git repository with a `main` branch and 3 feature branches: `feature/part-1`, `feature/part-2`, `feature/part-3`.  
The hint says **"How can file 'diffs' be brought to the main branch? Don't forget to `git config`!"** — pointing to git merging.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category
- **Easy** difficulty
- Tags include **git** — git branching challenge
- Description mentions team features — multiple branches with parts of the flag

Key observations:
- The repo has 3 feature branches each containing part of a flag printing Python script
- The flag is split across `feature/part-1`, `feature/part-2`, `feature/part-3`
- Merging all branches into main and running `flag.py` should print the full flag
- If merging fails due to conflicts, `git show <branch>:flag.py` reads each branch's file directly

The intended approach is to **merge all feature branches into main** and run the Python script, or read each branch's file directly if merging fails.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract
```bash
wget https://artifacts.picoctf.net/c_titan/69/challenge.zip
unzip challenge.zip  # type A if prompted
cd drop-in
```

### 3.2 Set Up Git Config
```bash
git config user.email "you@example.com"
git config user.name "you"
```

### 3.3 Check All Branches
```bash
git branch -a
```

Output shows:
```
main
feature/part-1
feature/part-2
feature/part-3
```

### 3.4 View Each Branch's Commit
```bash
git log --oneline feature/part-1
git log --oneline feature/part-2
git log --oneline feature/part-3
```

### 3.5 Read Each Branch's File Directly
Merging caused conflicts, so read each branch's `flag.py` directly:

```bash
git show feature/part-1:flag.py
git show feature/part-2:flag.py
git show feature/part-3:flag.py
```

Output:
```python
# Part 1
print("picoCTF{t3@mw0rk_", end='')

# Part 2
print("m@k3s_th3_dr3@m_", end='')

# Part 3
print("w0rk_e4b79efb}")
```

### 3.6 Assemble the Flag
Concatenating all 3 parts in order gives the full flag.

---

## 🧩 4. Final Flag

```
picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_e4b79efb}
```

---

## 📚 5. Key Learnings

- Git supports **multiple branches** — teams work on features in separate branches and merge them into main
- `git branch -a` lists all branches including remote ones
- `git show <branch>:filename` reads a file from a specific branch **without switching to it** — very useful in CTFs
- `git merge` can fail with **conflicts** when multiple branches modify the same file — in that case, read files directly
- Always run `git config user.email` and `git config user.name` before git operations in a new environment
- In real-world security, sensitive data can be hidden across multiple branches — always check all branches

---

## 🚀 6. Improvements for Next Time

- When merge fails, don't keep retrying — use `git show <branch>:file` to read directly
- Always check all branches first with `git branch -a`
- For merge conflicts use `git merge --abort` to reset before trying again
- Git branching commands to remember:
  ```bash
  git branch -a                    # list all branches
  git checkout <branch>            # switch to a branch
  git show <branch>:filename       # read file from branch without switching
  git merge <branch>               # merge branch into current
  git log --oneline --all --graph  # visual commit tree across all branches
  ```
- `git log --oneline --all --graph` is great for visualizing branching structure in CTFs

---

## 🔗 7. References

- [Git Branching – The Git Book](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Git Merge Documentation](https://git-scm.com/docs/git-merge)
- [Git Show Documentation](https://git-scm.com/docs/git-show)
- [picoCTF General Skills Challenges](https://picoctf.org)