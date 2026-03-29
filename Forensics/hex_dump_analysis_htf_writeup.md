# Hex Dump Analysis — Hack the Flag Writeup

**Challenge:** Hex Dump Analysis  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{zip}`  

---

## Description

Examine this hex dump from a file header: `50 4B 03 04`. This magic number identifies a specific file type. What type of file is this? Submit as `ctf{filetype_in_lowercase}`.

---

## Hints

> The bytes `50 4B 03 04` are the magic number for a well-known archive format. `50 4B` = "PK" — the initials of the creator.

---

## Solution

Every file format has a **magic number** — a fixed sequence of bytes at the start that identifies what it is.

### Step 1: Decode the hex bytes to ASCII

```bash
python3 -c "print(bytes.fromhex('504B0304'))"
```

Output: `b'PK\x03\x04'`

The first two printable characters are **PK**.

### Step 2: Look up the signature

`PK` stands for **Phil Katz**, the creator of the ZIP format. The magic bytes `50 4B 03 04` are universally recognized as the ZIP file signature.

| Hex Bytes | ASCII | File Type |
|-----------|-------|-----------|
| `50 4B 03 04` | `PK` | **ZIP archive** ← this challenge |
| `FF D8 FF` | — | JPEG image |
| `89 50 4E 47` | `‰PNG` | PNG image |
| `25 50 44 46` | `%PDF` | PDF document |
| `7F 45 4C 46` | `ELF` | Linux executable |

### Step 3: Verify with xxd (if you had the actual file)

```bash
xxd mystery_file | head -1
# 00000000: 504b 0304 ...   PK..
```

### Step 4: Use the file command

```bash
file mystery_file
# mystery_file: Zip archive data
```

---

## What are Magic Bytes?

Magic bytes are the **fingerprint of a file format**. Operating systems and forensic tools use these bytes — not the file extension — to identify what a file truly is. Renaming `archive.zip` to `photo.jpg` does nothing to the magic bytes.

---

## Flag

```
ctf{zip}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `xxd` | Hex dump — shows raw bytes with ASCII |
| `file` | Identifies file type from magic bytes |
| `python3` | Quick hex-to-ASCII conversion |

---

## Key Takeaways

- Files are identified by **magic bytes**, not extensions
- `50 4B` = "PK" = ZIP (named after Phil Katz, inventor of the format)
- The `file` command is your best friend for quick file type identification
- Reference: https://en.wikipedia.org/wiki/List_of_file_signatures
