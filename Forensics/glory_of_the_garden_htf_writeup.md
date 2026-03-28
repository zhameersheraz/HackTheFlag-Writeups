# Glory of the Garden — Hack the Flag Writeup

**Challenge:** Glory of the Garden  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 70 pts  
**Flag:** `picoCTF{more_than_m33ts_the_3y37fde8891}`  

---

## Description

This file contains more than it seems. Get the flag from the image.

*(Hint: hex)*

---

## Solution

The hint says "hex" — the flag is hidden in the raw binary data of the image. The `strings` command extracts all printable ASCII sequences from any binary.

### Step 1: Download the file

```bash
wget <challenge-file-url> -O garden.png
```

### Step 2: Run strings

```bash
strings garden.png
```

### Output (relevant portion):

```
...
ZqKvu
7g4js'
...
Here is a flag: picoCTF{more_than_m33ts_the_3y37fde8891}
```

### Step 3: Filter with grep (faster)

```bash
strings garden.png | grep -i "ctf\|flag\|picoCTF"
```

### Step 4 (Alternative): Hex dump inspection

```bash
xxd garden.png | tail -20
```

`xxd` shows a side-by-side hex + ASCII view. The flag appears on the right-hand ASCII column near the end of the file.

---

## Why This Works

Valid PNG files end at the `IEND` chunk. Anything appended **after** `IEND` is ignored by image viewers but still exists in the raw file. CTF challenges often hide flags this way — the image looks normal, but the bytes contain extra data.

---

## Flag

```
picoCTF{more_than_m33ts_the_3y37fde8891}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` | Extracts human-readable text from binary files |
| `xxd` | Hex dump — shows raw bytes + ASCII side by side |
| `grep` | Filters output for flag patterns |

---

## Key Takeaways

- Images can have data **appended after the valid file format ends**
- `strings | grep ctf` is a fast one-liner to find flags in any binary
- `xxd` lets you see exactly what bytes are in a file
