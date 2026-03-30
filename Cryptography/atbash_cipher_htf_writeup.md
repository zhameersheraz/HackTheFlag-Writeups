# Atbash Cipher — Hack the Flag Writeup

**Challenge:** Atbash Cipher  
**Category:** Cryptography  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{atbash_mirror}`  

---

## Description

Decode this Atbash cipher: `xgu{zgyzhs_nriili}`. In Atbash, A=Z, B=Y, C=X, and so on.

---

## Hints

> Atbash reverses the alphabet: A becomes Z, B becomes Y, C becomes X, etc. Apply this to each letter.

---

## Solution

### What is Atbash?

Atbash maps each letter to its **mirror position** in the alphabet:

```
A ↔ Z    B ↔ Y    C ↔ X    D ↔ W    ...    M ↔ N
```

Numbers, symbols, and spaces stay unchanged.

### Step 1: Use CyberChef (recommended)

1. Go to 👉 https://gchq.github.io/CyberChef/
2. Paste `xgu{zgyzhs_nriili}` into the **Input** box
3. Search **Atbash Cipher** → drag into Recipe
4. Click **BAKE!**

Output: `ctf{atbash_mirror}` ✅

### Step 2 (Alternative): Python

```python
def atbash(text):
    result = ''
    for ch in text:
        if ch.isalpha():
            if ch.isupper():
                result += chr(ord('Z') - (ord(ch) - ord('A')))
            else:
                result += chr(ord('z') - (ord(ch) - ord('a')))
        else:
            result += ch
    return result

print(atbash("xgu{zgyzhs_nriili}"))
# Output: ctf{atbash_mirror}
```

```bash
python3 atbash.py
# ctf{atbash_mirror}
```

---

## Atbash Decode Table

| Cipher | `x` | `g` | `u` | `{` | `z` | `g` | `y` | `z` | `h` | `s` | `_` | `n` | `r` | `i` | `i` | `l` | `i` | `}` |
|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **Plain** | `c` | `t` | `f` | `{` | `a` | `t` | `b` | `a` | `s` | `h` | `_` | `m` | `i` | `r` | `r` | `o` | `r` | `}` |

---

## Flag

```
ctf{atbash_mirror}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef (Atbash Cipher) | One-click decode |
| Python | Manual implementation |
| dcode.fr/atbash-cipher | Web-based decoder |

---

## Key Takeaways

- Atbash is a **substitution cipher** that mirrors the alphabet
- It is **self-inverse**: encrypting twice returns the original text
- Recognizable pattern: the encoded flag starts with `xgu` which maps cleanly to `ctf`
