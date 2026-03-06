# File Signature - Hack the Flag Writeup

**Challenge:** File Signature  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{png}`

---

## Description

Every file type has a magic number (file signature) at the beginning. A PNG file starts with the hex bytes `89 50 4E 47`. What do the bytes `50 4E 47` spell in ASCII? Submit as `ctf{answer_in_lowercase}`.

---

## Hints

1. Convert hex bytes to ASCII characters: 50="P", 4E="N", 47="G".

---

## Solution

### Step 1: Read the Question

The challenge gave us 3 hex bytes and asked what they spell in ASCII:

```
50  4E  47
```

### Step 2: Convert Hex to ASCII

Using the ASCII table:

| Hex | ASCII |
|-----|-------|
| 50 | P |
| 4E | N |
| 47 | G |

So `50 4E 47` = **PNG**

### Step 3: Submit in Lowercase

The challenge said to submit as `ctf{answer_in_lowercase}`:

```
ctf{png}
```

Got the flag! 🎯

---

## Why This Works

### What is a File Signature?

Every file type has a **magic number** — a specific sequence of bytes at the very beginning of the file that identifies what type of file it is. This is how your computer knows what program to open a file with, even if the file extension is changed.

For PNG files, the full magic number is:
```
89 50 4E 47 0D 0A 1A 0A
```

The bytes `50 4E 47` spell out **PNG** in ASCII — the file format name is literally written in the file header!

### Simple Breakdown

```
Read the hex bytes: 50 4E 47
      |
      v
Convert each to ASCII using ASCII table
50=P, 4E=N, 47=G
      |
      v
They spell: PNG
      |
      v
Submit in lowercase: ctf{png}
```

---

## Alternative Method: Python

```python
hex_bytes = ['50', '4E', '47']
result = ''.join([chr(int(h, 16)) for h in hex_bytes])
print(result.lower())  # Output: png
```

---

## Flag

```
ctf{png}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| ASCII table | Convert hex values to characters |

---

## Key Takeaways

- Every file type has a unique **magic number** (file signature) at the start
- PNG files start with `89 50 4E 47` where `50 4E 47` spells "PNG"
- Knowing file signatures is useful in Forensics challenges
- You can check a file's real type using the `file` command or by reading its hex header with `xxd`o
