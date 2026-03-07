# strings it - Hack the Flag Writeup

**Challenge:** strings it  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `picoCTF{5tRIng5_1T_A1b9ECAa}`

---

## Description

Can you find the flag in a file without running it?

---

## Hints

1. String or hexeditor

---

## Solution

### Step 1: Check What the File Is

I downloaded the challenge file and ran `exiftool` to see what type of file it was:

```bash
exiftool 1772730661_strings__1_
```

Key output:
```
File Type     : ELF shared library
File Type Ext : so
CPU Type      : AMD x86-64
Object Type   : Shared object file
```

It's a **compiled binary** (Linux `.so` shared library). We can't just open it like a text file — but we don't need to run it either. We just need to extract readable text from inside it.

### Step 2: Use the strings Command

`strings` is a tool that pulls out all **human-readable text** embedded inside a binary file. Even compiled programs often have plaintext strings baked in.

First I searched for `ctf`:

```bash
strings 1772730661_strings__1_ | grep ctf
```

Output:
```
tedjxd10rPQo8dZj9KructfPPqrKBISAAMrwGWo1vsSou
```

That had `ctf` but it was buried inside a random string — not the flag.

Then I searched for `flag`:

```bash
strings 1772730661_strings__1_ | grep -i "flag"
```

Output:
```
Mf9MFZCIv8gY7faUsaeAx8oEI8pvFLAGYZSFekIv
```

Still not clean. So I searched for the **curly brace** `{` which is always part of the flag format:

```bash
strings 1772730661_strings__1_ | grep "{"
```

Output:
```
picoCTF{5tRIng5_1T_A1b9ECAa}
```

Got the flag! 🎯

---

## Why This Works

### What is a Binary File?

When developers write code and compile it, it becomes a binary — a file full of machine instructions that the CPU can execute. It looks like gibberish when you open it.

But here's the thing: **text strings are still stored as plain text** inside the binary. Things like error messages, version numbers, hardcoded passwords, and in this case — a flag!

```
Binary file (looks like garbage):
^@^@^@^@ELF^B^A^A^@^@^@^@^@...

But hidden inside:
picoCTF{5tRIng5_1T_A1b9ECAa}  ← plain text, readable!
```

The `strings` command scans the binary and pulls out anything that looks like readable text (sequences of printable characters).

### Why grep "{"?

Searching for `ctf` or `flag` can match random garbage strings in the binary. But `{` (curly brace) is very specific — it almost never appears randomly in binary data. Since every CTF flag uses `{` in its format, searching for it is the most reliable trick!

```
grep "ctf"   → too broad, matches junk like "tedjxd...ructf..."
grep "flag"  → too broad, matches "pvFLAGYZS..."
grep "{"     → very specific → finds ctf{...} and picoCTF{...} ✅
```

---

## Alternative Methods

### Method 1: hexed.it (Online Hex Editor)
1. Go to 👉 https://hexed.it/
2. Upload the binary file
3. Use `Ctrl+F` to search for `ctf` or `{`
4. Find the flag in the hex dump

### Method 2: strings + grep on Windows (Git Bash)
```bash
strings strings_file | grep "{"
```

### Method 3: Python
```python
with open("strings_file", "rb") as f:
    data = f.read().decode(errors="ignore")
    
for word in data.split():
    if "{" in word and "}" in word:
        print(word)
```

### Method 4: Ghidra / IDA (Overkill for this challenge)
Reverse engineering tools like Ghidra can also show all embedded strings, but `strings` command is much faster for simple challenges like this.

---

## Flag

```
picoCTF{5tRIng5_1T_A1b9ECAa}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `exiftool` | Identify the file type (ELF binary) |
| `strings` | Extract readable text from inside the binary |
| `grep "{"` | Filter for flag-format strings specifically |

---

## Key Takeaways

- `strings <file>` extracts all readable text from any binary — always try this first!
- `grep "{"` is the most reliable way to find CTF flags in binary output
- Even compiled binaries can have plaintext secrets hardcoded inside them
- This is why developers should never hardcode passwords or keys directly in their code
- `exiftool` can identify unknown file types before you start analyzing them
