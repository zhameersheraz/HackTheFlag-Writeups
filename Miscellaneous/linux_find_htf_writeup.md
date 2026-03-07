# Linux Find - Hack the Flag Writeup

**Challenge:** Linux Find  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{find /home -name *.txt}`

---

## Description

In Linux, the command to search for files by name is `find`. What command would find all `.txt` files in `/home`? Submit the complete command as `ctf{command}`.

---

## Hints

1. The find command syntax is: `find [path] [expression]`. Use `-name` for filename matching.

---

## Solution

### Step 1: Build the Command

The hint tells us the syntax:

```
find [path] [expression]
```

Filling in the blanks:
- **path** = `/home` (where to search)
- **expression** = `-name *.txt` (what to look for)

Put it together:

```bash
find /home -name *.txt
```

### Step 2: Submit as the Flag

Wrap it in the flag format:

```
ctf{find /home -name *.txt}
```

Got the flag! 🎯

---

## ⚠️ Why `"*.txt"` Was Wrong

My first attempt was:

```
ctf{find /home -name "*.txt"}   ← WRONG ❌
```

This felt right because in real Linux usage, you **should** put quotes around `*.txt`. Here's why:

Without quotes, your shell expands `*.txt` into actual matching filenames **before** passing them to `find`. With quotes, `find` itself handles the wildcard correctly.

**BUT** — this is a CTF challenge asking for the raw command, not shell-safe syntax. The challenge expected the command **without quotes**:

```
ctf{find /home -name *.txt}     ← CORRECT ✅
```

Think of it like being asked to write the command, not run it in a real terminal. The answer they wanted was the plain command without shell quoting.

### Quick Visual

```
Real terminal (best practice):   find /home -name "*.txt"  ← use quotes
CTF answer (exact command):      find /home -name *.txt    ← no quotes
```

---

## Breaking Down the Command

```
find  /home  -name  *.txt
  ↑     ↑      ↑      ↑
  │     │      │      └── pattern: any file ending in .txt
  │     │      └───────── flag: search by filename
  │     └──────────────── path: search inside /home directory
  └────────────────────── the command itself
```

---

## Useful `find` Commands for Future CTFs

| Command | What it does |
|---------|-------------|
| `find / -name flag.txt` | Find flag.txt anywhere on the system |
| `find /home -name *.txt` | Find all .txt files in /home |
| `find . -name "*.jpg"` | Find all JPGs in current directory |
| `find / -name flag*` | Find any file starting with "flag" |
| `find / -name "*.txt" 2>/dev/null` | Hide permission errors |

---

## Flag

```
ctf{find /home -name *.txt}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Linux knowledge | Know the `find` command syntax |
| Trial and error | First tried with quotes, then without |

---

## Key Takeaways

- `find [path] -name [pattern]` is the basic syntax for finding files in Linux
- CTF challenges asking for "the command" usually want it **without** shell quoting
- In a real terminal, always quote wildcards: `find /home -name "*.txt"`
- `find` is one of the most useful commands in CTF forensics and OSINT challenges
- When your first answer is wrong in a CTF, try removing quotes or special characters — formatting often trips people up!
