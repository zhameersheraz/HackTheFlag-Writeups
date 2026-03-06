# Commitment Issues - picoCTF Writeup

**Challenge:** Commitment Issues  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** (not specified)  
**Flag:** `picoCTF{s@n1t1z3_cf09a485}`

---

## Description

Ever had to keep your commits... secret? Download the challenge files here: challenge.zip

---

## Solution

### Step 1: Download and Extract the File

```bash
wget https://artifacts.picoctf.net/c_titan/137/challenge.zip
unzip challenge.zip
cd drop-in
```

### Step 2: Read message.txt

```bash
cat message.txt
```

**Output:**
```
TOP SECRET
```

The file just says "TOP SECRET" — the flag was removed!

### Step 3: Check Git Commit History

```bash
git reflog
```

**Output:**
```
ef0b7cc (HEAD -> master) HEAD@{0}: commit: remove sensitive info
ea859bf HEAD@{1}: commit (initial): create flag
```

I can see there are 2 commits:
- The latest commit (`ef0b7cc`) removed the sensitive info
- The first commit (`ea859bf`) originally created the flag

### Step 4: Go Back to the First Commit

I used `git checkout` to travel back to the first commit before the flag was deleted:

```bash
git checkout ea859bf
```

### Step 5: Read message.txt Again

```bash
cat message.txt
```

**Output:**
```
picoCTF{s@n1t1z3_cf09a485}
```

Got the flag! 🎯

---

## Why This Works

### Git Never Truly Deletes Anything

Even when someone removes a file or overwrites content in a commit, **Git keeps the full history** of all previous commits. You can always go back to any previous state using `git checkout` with the commit hash.

In this challenge, the developer thought they were hiding the flag by making a new commit that replaced the content. But the original commit with the flag was still stored in Git history!

### Simple Breakdown

```
Extract zip file
      |
      v
cat message.txt shows "TOP SECRET" (flag was deleted)
      |
      v
git reflog shows 2 commits
      |
      v
First commit is named "create flag"
      |
      v
git checkout ea859bf (go back to first commit)
      |
      v
cat message.txt now shows the flag!
```

---

## Commands Used

```bash
# Download and extract
wget https://artifacts.picoctf.net/c_titan/137/challenge.zip
unzip challenge.zip
cd drop-in

# Read the file
cat message.txt

# Check commit history
git reflog

# Go back to the first commit
git checkout ea859bf

# Read the file again
cat message.txt
```

---

## Flag

```
picoCTF{s@n1t1z3_cf09a485}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `wget` | Download the zip file |
| `unzip` | Extract the zip file |
| `cat` | Read message.txt |
| `git reflog` | View all commits and their hashes |
| `git checkout` | Travel back to a previous commit |

---

## Key Takeaways

- Git stores the **full history** of all commits — deleting content in a new commit does not erase old commits
- `git reflog` shows every commit ever made, including ones that removed files
- `git checkout <hash>` lets you travel back to any previous commit
- Never store sensitive info like flags, passwords or API keys in Git — even deleted commits can be recovered
- This is why "sanitizing" commit history properly requires tools like `git filter-branch` or `git filter-repo`o\

