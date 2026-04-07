# Python Reverse — Hack the Flag Writeup

**Challenge:** Python Reverse  
**Category:** Reverse Engineering  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{python_re3versed}`  

---

## Description

A Python program checks the flag with this logic:

```python
def check(flag):
    encoded = ""
    for c in flag:
        encoded += chr(ord(c) + 1)
    return encoded == "dug|qzuipo`sf4wfstfe~"
```

Reverse the encoding to find the flag.

---

## Solution

### Step 1: Understand what the code does

The function loops over every character in the flag and replaces it with the **next ASCII character** (+1 to the ASCII value):

```
chr(ord(c) + 1)
```

For example:
- `c` (ASCII 99) → `d` (ASCII 100)
- `t` (ASCII 116) → `u` (ASCII 117)
- `3` (ASCII 51) → `4` (ASCII 52)

The function then checks if the result equals the **target encoded string**:
```
dug|qzuipo`sf4wfstfe~
```

### Step 2: Reverse the encoding

To reverse, we do the **opposite** — subtract 1 from each character instead of adding 1:

```python
encoded = "dug|qzuipo`sf4wfstfe~"
flag = ''.join(chr(ord(c) - 1) for c in encoded)
print(flag)
# ctf{python_re3versed}
```

```bash
python3 -c "print(''.join(chr(ord(c)-1) for c in 'dug|qzuipo\`sf4wfstfe~'))"
# ctf{python_re3versed}
```

### Step 3: Verify the answer

```python
def check(flag):
    encoded = ""
    for c in flag:
        encoded += chr(ord(c) + 1)
    return encoded == "dug|qzuipo`sf4wfstfe~"

print(check("ctf{python_re3versed}"))
# True ✅
```

---

## ❌ Why My Wrong Attempts Failed

This is important to understand — each wrong attempt failed for a specific reason.

### Attempt 1: `ctf{python_re2verse}`

**Problems: wrong digit (2 instead of 3) AND missing the final `d`**

The encoded string has `4` at the number position. Since encoding does `+1`, we must do `-1` to decode:
```
4 - 1 = 3   ← the correct digit is 3, NOT 2
```
Also, the encoded string ends in `fe~` which decodes to `ed}` meaning the word ends in **"ed"** (reversed → **re3versed**, not re3verse).

```
Encoded target:  dug|qzuipo`sf4wfstfe~
Your attempt:    dug|qzuipo`sf3wfstf~   ← has 3 AND is missing one char
```

---

### Attempt 2: `ctf{python_re2versed}`

**Problem: wrong digit (2 instead of 3)**

The encoded string has the character `4` at the digit position. Decoding means `4 - 1 = 3`, not 2.

```
Encoded target:  dug|qzuipo`sf4wfstfe~
Your attempt:    dug|qzuipo`sf3wfstfe~   ← 3 ≠ 4, so doesn't match
```

The mistake here was probably guessing `2` by intuition instead of actually decoding the character. Always decode each character mathematically — don't guess!

---

### Attempt 3: `ctf{python_reversed}`

**Problem: missing the digit entirely**

The encoded target has `sf4wf` which decodes to `re3ve`. The `4` must be there — skipping it means the encoded string won't match.

```
Encoded target:  dug|qzuipo`sf4wfstfe~
Your attempt:    dug|qzuipo`sfwfstfe~    ← missing the digit position entirely
```

---

### Attempt 4: `python_re2versed`

**Problems: missing `ctf{}` wrapper AND wrong digit**

The `check()` function encodes the **entire** string including `ctf{` and `}`. The target encoded string `dug|...~` starts with `dug|` which decodes to `ctf{`. The flag must always be wrapped in `ctf{}`.

```
Encoded target:  dug|qzuipo`sf4wfstfe~
Your attempt:    qzuipo`sf3wfstfe        ← completely different — missing prefix and suffix
```

---

## The Decode Table (full breakdown)

| Encoded char | ASCII | -1 | Decoded char |
|-------------|-------|----|-------------|
| `d` | 100 | 99 | `c` |
| `u` | 117 | 116 | `t` |
| `g` | 103 | 102 | `f` |
| `\|` | 124 | 123 | `{` |
| `q` | 113 | 112 | `p` |
| `z` | 122 | 121 | `y` |
| `u` | 117 | 116 | `t` |
| `i` | 105 | 104 | `h` |
| `p` | 112 | 111 | `o` |
| `o` | 111 | 110 | `n` |
| `` ` `` | 96 | 95 | `_` |
| `s` | 115 | 114 | `r` |
| `f` | 102 | 101 | `e` |
| `4` | 52 | 51 | **`3`** ← this was the tricky one! |
| `w` | 119 | 118 | `v` |
| `f` | 102 | 101 | `e` |
| `s` | 115 | 114 | `r` |
| `t` | 116 | 115 | `s` |
| `f` | 102 | 101 | `e` |
| `e` | 101 | 100 | `d` |
| `~` | 126 | 125 | `}` |

Result: `ctf{python_re3versed}` ✅

---

## Flag

```
ctf{python_re3versed}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python interpreter | Write and run the reverse decoder |
| ASCII table | Understand +1/-1 character shifts |

---

## Key Takeaways

- **Always decode mathematically** — don't guess flag names. Map each encoded character back using `ord(c) - 1`.
- **Include the full flag wrapper** — `ctf{` and `}` are part of the input being encoded. If the encoded string starts with `dug|`, decode it — you'll get `ctf{`.
- **The digit `4` in the encoded string decodes to `3`** — this is where `re3versed` comes from, not `re2versed`.
- When reversing any encoding function: do the **exact opposite operation** on the encoded output to recover the input.
