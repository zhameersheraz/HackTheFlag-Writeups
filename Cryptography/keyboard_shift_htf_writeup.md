# Keyboard Shift — Hack the Flag Writeup

**Challenge:** Keyboard Shift  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{k3ybo4rd_sh1ft}`  

---

## Description

Someone typed the flag on a keyboard shifted one key to the right: `vyg{l4unp5tf_dj2gy}`. Each character is the key to the RIGHT of the intended key on a QWERTY keyboard. Shift left to decode!

---

## Hints

> Look at your QWERTY keyboard. For each character, find the key to its LEFT. For example, "v" shifted left is "c".

---

## Solution

### Understanding the QWERTY Layout

```
Row 1 (digits):  1 2 3 4 5 6 7 8 9 0
Row 2 (top):     q w e r t y u i o p
Row 3 (home):    a s d f g h j k l
Row 4 (bottom):  z x c v b n m
```

Since each character was typed **one key to the right**, decode by shifting every character **one key to the left**.

### Step 1: Python decoder (most reliable)

```python
rows = ['1234567890', 'qwertyuiop', 'asdfghjkl', 'zxcvbnm']

def shift_left(ch):
    for row in rows:
        if ch.lower() in row:
            idx = row.index(ch.lower())
            if idx > 0:
                decoded = row[idx - 1]
                return decoded.upper() if ch.isupper() else decoded
            return ch  # at left edge, no shift
    return ch  # symbols/numbers not in rows stay as-is

cipher = "vyg{l4unp5tf_dj2gy}"
flag = ''.join(shift_left(c) for c in cipher)
print(flag)
# Output: ctf{k3ybo4rd_sh1ft}
```

```bash
python3 keyboard_decode.py
# ctf{k3ybo4rd_sh1ft}
```

### Step 2: Manual mapping (key characters)

| Cipher | Shift Left | Plain |
|--------|-----------|-------|
| `v` | c**v** → `c` | `c` |
| `y` | t**y** → `t` | `t` |
| `g` | f**g** → `f` | `f` |
| `{` | stays | `{` |
| `l` | k**l** → `k` | `k` |
| `4` | 3**4** → `3` | `3` |
| `u` | y**u** → `y` | `y` |
| `n` | b**n** → `b` | `b` |
| `p` | o**p** → `o` | `o` |
| `5` | 4**5** → `4` | `4` |
| `t` | r**t** → `r` | `r` |
| `f` | d**f** → `d` | `d` |
| `_` | stays | `_` |
| `d` | s**d** → `s` | `s` |
| `j` | h**j** → `h` | `h` |
| `2` | 1**2** → `1` | `1` |
| `g` | f**g** → `f` | `f` |
| `y` | t**y** → `t` | `t` |

Reading all: `ctf{k3ybo4rd_sh1ft}` ✅

---

## Flag

```
ctf{k3ybo4rd_sh1ft}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python | Automated QWERTY shift decoder |
| Physical/visual QWERTY reference | Manual letter mapping |

---

## Key Takeaways

- Keyboard shift encodes by replacing each key with the one to its RIGHT
- Decode by finding the key to the LEFT of each character
- `{`, `}`, `_` are non-alphabetic separators — they stay as-is
- Notice `l4` — always check if a digit might be a lowercase `l` in an ambiguous font!
