# information - Hack the Flag Writeup

**Challenge:** information  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `picoCTF{the_m3tadata_1s_modified}`

---

## Description

Files can always be changed in a secret way. Can you find the flag? Flag format is `picoCTF{...}`

---

## Hints

1. Look at the details of the file

---

## Solution

### Step 1: Navigate to the File

```bash
cd /media/sf_downloads
```

### Step 2: Run exiftool on the Image

`exiftool` reads all hidden metadata stored inside a file:

```bash
exiftool 1772730322_cat.jpg
```

Output includes this suspicious field:

```
License : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

That looks like Base64!

### Step 3: Decode the Base64 String

```bash
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
```

Output:
```
picoCTF{the_m3tadata_1s_modified}
```

Got the flag! 🎯

> ⚠️ **Common mistake:** Forgetting the `-d` flag on base64! Without it, the command tries to **encode** instead of **decode** and gives wrong output. Always use `base64 -d` to decode.

---

## What is Metadata?

Every file contains **metadata** — hidden information about the file itself that isn't visible when you just open it normally:

```
cat.jpg (what you see):  a photo of a cat 🐱
cat.jpg (metadata):      creation date, camera model, GPS location,
                         copyright, author, custom fields... and flags!
```

`exiftool` reads ALL of this hidden data. In this challenge, the flag was hidden inside the **License** field, encoded in Base64 to make it less obvious.

---

## Why Base64?

Base64 looks like random letters — `cGljb0NURnt0aGVf...` — so it's not immediately obvious it's a flag. It's a common way to slightly hide data in CTF challenges. You can recognize Base64 because:
- Only uses letters, numbers, `+`, `/`, and `=` at the end
- Length is always a multiple of 4

---

## Step-by-Step Summary

| Step | Command | Result |
|------|---------|--------|
| 1 | `cd /media/sf_downloads` | Navigate to file |
| 2 | `exiftool cat.jpg` | Read all metadata |
| 3 | Find suspicious Base64 in License field | `cGljb0NURnt...` |
| 4 | `echo "cGljb0..." \| base64 -d` | `picoCTF{the_m3tadata_1s_modified}` |

---

## Flag

```
picoCTF{the_m3tadata_1s_modified}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `exiftool` | Read all hidden metadata from the image file |
| `base64 -d` | Decode the Base64 string found in the License field |

---

## Key Takeaways

- **Always run `exiftool`** on image files in forensics/general skills challenges — flags are often hidden in metadata!
- `base64 -d` decodes, `base64` without `-d` encodes — don't forget the `-d`!
- Metadata fields like Author, Copyright, License, Comment are common hiding spots for CTF flags
- Base64 is easy to spot: looks like random letters/numbers ending with `=`
