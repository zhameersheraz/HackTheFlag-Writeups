# Substitution Cipher — Hack the Flag Writeup

**Challenge:** Substitution Cipher  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 250 pts  
**Flag:** `ctf{substitution_cracked}`

---

## Description

Each letter has been shifted forward by one position in the alphabet (A becomes B, B becomes C, etc). Crack this:

`dug{tvctujuvujpo_dsbdlfe}`

The plaintext starts with "ctf{".

**Hint:** You know the first 4 characters: d=c, u=t, g=f, {={. Use those to start building the substitution table, then guess common words.

---

## Solution

### Step 1: Understand the Cipher

The challenge says each letter was **shifted forward by 1** (Caesar cipher with ROT+1). To decrypt, we simply **shift each letter back by 1**.
```
Encrypted: d u g { t v c t u j u v u j p o _ d s b d l f e }
Shift -1:  c t f { s u b s t i t u t i o n _ c r a c k e d }
```

### Step 2: Use CyberChef to Decrypt

1. Go to [CyberChef](https://gchq.github.io/CyberChef/)
2. Input: `dug{tvctujuvujpo_dsbdlfe}`
3. Add recipe: **ROT13** → change Amount from `13` to `25` (shifting back 1 = shifting forward 25)
4. Click **Bake!**

**Output:**
```
ctf{substitution_cracked}
```

✅ **Flag found!**

### Alternative: Python One-Liner
```python
cipher = "dug{tvctujuvujpo_dsbdlfe}"
flag = ""
for c in cipher:
    if c.isalpha():
        # shift back by 1
        if c == 'a':
            flag += 'z'
        else:
            flag += chr(ord(c) - 1)
    else:
        flag += c
print(flag)
```

**Output:**
```
ctf{substitution_cracked}
```

---

## Why This Works

### Caesar Cipher Basics

A Caesar cipher shifts every letter by a fixed number of positions in the alphabet. ROT+1 means:
```
A → B
B → C
C → D
...
Z → A
```

To reverse it, we just go the other direction (shift back by 1):
```
B → A
C → B
D → C
...
A → Z
```

### Step-by-Step Letter Decryption

| Cipher | Shift -1 | Plain |
|--------|----------|-------|
| d | -1 | c |
| u | -1 | t |
| g | -1 | f |
| { | — | { |
| t | -1 | s |
| v | -1 | u |
| c | -1 | b |
| t | -1 | s |
| u | -1 | t |
| j | -1 | i |
| v | -1 | u |
| u | -1 | t |
| j | -1 | i |
| p | -1 | o |
| o | -1 | n |
| _ | — | _ |
| d | -1 | c |
| s | -1 | r |
| b | -1 | a |
| d | -1 | c |
| l | -1 | k |
| f | -1 | e |
| e | -1 | d |
| } | — | } |

Result: `ctf{substitution_cracked}`

---

## CyberChef Recipe Summary
```
Input:   dug{tvctujuvujpo_dsbdlfe}
Recipe:  ROT13 (Amount: 25)
Output:  ctf{substitution_cracked}
```

> 💡 **Why Amount 25?** Shifting back by 1 is the same as shifting forward by 25 in a 26-letter alphabet. ROT13 only does 13 by default — change the amount to 25 to get ROT25 (= ROT-1).

---

## Simple Breakdown
```
Read ciphertext: dug{tvctujuvujpo_dsbdlfe}
        ↓
Hint confirms: d=c, u=t, g=f → shift back by 1
        ↓
CyberChef ROT13 with Amount: 25
        ↓
Output: ctf{substitution_cracked}
```

---

## Flag
```
ctf{substitution_cracked}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef ROT13 (Amount: 25) | Shift each letter back by 1 to decrypt |
| Python | Alternative method to reverse the cipher |

---

## Key Takeaways

- **Caesar cipher** shifts letters by a fixed number — always try shifting back by the same amount to decrypt
- **Shift back by 1 = ROT25** in CyberChef (26 - 1 = 25)
- The hint gave us the first 4 characters (d=c, u=t, g=f) which immediately revealed the shift amount
- `{`, `}`, and `_` are not letters so they are never shifted — always passed through unchanged
- When you know even one plaintext-ciphertext pair, you can instantly determine the shift value for the entire Caesar cipher
