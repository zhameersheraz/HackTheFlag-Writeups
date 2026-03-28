# Strings Attached — Hack the Flag Writeup

**Challenge:** Strings Attached  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{str1ngs_r3veals_all}`  

---

## Description

Download this mysterious binary file. Somewhere inside it, a readable flag is hiding among the binary data.

---

## Hints

> The `strings` command extracts readable text from binary files.

---

## Solution

### Step 1: Try exiftool first (gives no useful info)

```bash
exiftool strings_attached.bin
```

```
Error : Unknown file type
```

exiftool doesn't recognize it. That's fine — `strings` works on anything.

### Step 2: Run strings

```bash
strings strings_attached.bin
```

### Output:

```
ctf{str1ngs_r3veals_all}
```

The flag appears directly in the output.

### Step 3: Filter with grep (for larger files)

```bash
strings strings_attached.bin | grep "ctf{"
```

---

## How strings Works

The `strings` command scans any file byte-by-byte and prints every sequence of **4 or more consecutive printable ASCII characters**. Hidden text — even inside compiled binaries or random binary blobs — will be surfaced.

**Useful flags:**

| Flag | Effect |
|------|--------|
| `strings -n 8 file` | Only show strings 8+ chars long (reduces noise) |
| `strings -a file` | Scan the entire file |
| `strings file \| grep -i flag` | Filter for flag-like strings |

---

## Flag

```
ctf{str1ngs_r3veals_all}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` | Extracts printable text sequences from any binary |
| `grep` | Filters output for specific patterns |

---

## Key Takeaways

- `strings` + `grep` is one of the fastest first steps in any forensics challenge
- Binary files are not opaque — readable data is almost always present
- Always try `strings` before reaching for a hex editor
