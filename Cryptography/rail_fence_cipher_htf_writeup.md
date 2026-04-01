# Rail Fence Cipher — Hack the Flag Writeup

**Challenge:** Rail Fence Cipher  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{rail_fence_cracked}`  

---

## Description

Decrypt this Rail Fence cipher (3 rails): `cr_cret{alfnecakdfie_c}`. The plaintext is a valid flag.

---

## Hints

> A Rail Fence cipher writes text in a zigzag pattern across N rails, then reads each rail. Try 3 rails.

---

## Solution

### What is the Rail Fence Cipher?

The Rail Fence cipher writes plaintext in a **zigzag (diagonal) pattern** across N rails, then reads horizontally row by row to produce ciphertext.

For 3 rails:
```
Rail 1: X . . . X . . . X . . . X . . . X . . . X . .
Rail 2: . X . X . X . X . X . X . X . X . X . X . X .
Rail 3: . . X . . . X . . . X . . . X . . . X . . . X
```

### Step 1: Identify rail positions for 23 characters

| Rail | Positions | Character Count |
|------|-----------|----------------|
| Rail 1 | 0, 4, 8, 12, 16, 20 | **6 chars** |
| Rail 2 | 1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21 | **11 chars** |
| Rail 3 | 2, 6, 10, 14, 18, 22 | **6 chars** |

### Step 2: Split ciphertext `cr_cret{alfnecakdfie_c}` into rails

```
Rail 1 (6):  c r _ c r e
Rail 2 (11): t { a l f n e c a k d
Rail 3 (6):  f i e _ c }
```

### Step 3: Reconstruct by filling positions

| Pos | Rail | Char | | Pos | Rail | Char |
|-----|------|------|-|-----|------|------|
| 0 | R1 | `c` | | 12 | R1 | `c` |
| 1 | R2 | `t` | | 13 | R2 | `e` |
| 2 | R3 | `f` | | 14 | R3 | `_` |
| 3 | R2 | `{` | | 15 | R2 | `c` |
| 4 | R1 | `r` | | 16 | R1 | `r` |
| 5 | R2 | `a` | | 17 | R2 | `a` |
| 6 | R3 | `i` | | 18 | R3 | `c` |
| 7 | R2 | `l` | | 19 | R2 | `k` |
| 8 | R1 | `_` | | 20 | R1 | `e` |
| 9 | R2 | `f` | | 21 | R2 | `d` |
| 10 | R3 | `e` | | 22 | R3 | `}` |
| 11 | R2 | `n` | | | | |

**Plaintext: `ctf{rail_fence_cracked}`** ✅

### Step 4: CyberChef (quickest method)

1. Go to 👉 https://gchq.github.io/CyberChef/
2. Paste `cr_cret{alfnecakdfie_c}` into **Input**
3. Search **Rail Fence Cipher Decode** → drag into Recipe
4. Set **Key (Rails)** = `3`
5. Click **BAKE!**

Output: `ctf{rail_fence_cracked}` ✅

### Python decoder

```python
def rail_fence_decode(cipher, rails):
    n = len(cipher)
    pattern, rail, direction = [], 0, 1
    for _ in range(n):
        pattern.append(rail)
        if rail == rails - 1: direction = -1
        elif rail == 0: direction = 1
        rail += direction

    indices = [[] for _ in range(rails)]
    for i, r in enumerate(pattern):
        indices[r].append(i)

    chars = [[] for _ in range(rails)]
    pos = 0
    for r in range(rails):
        for _ in indices[r]:
            chars[r].append(cipher[pos])
            pos += 1

    result = [''] * n
    for r in range(rails):
        for i, idx in enumerate(indices[r]):
            result[idx] = chars[r][i]
    return ''.join(result)

print(rail_fence_decode("cr_cret{alfnecakdfie_c}", 3))
# ctf{rail_fence_cracked}
```

```bash
python3 railfence.py
# ctf{rail_fence_cracked}
```

---

## Flag

```
ctf{rail_fence_cracked}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef (Rail Fence Decode) | Instant decode with rails=3 |
| Python | Manual implementation |
| dcode.fr/rail-fence-cipher | Web decoder with visualization |

---

## Key Takeaways

- Rail Fence writes diagonally, reads horizontally row by row
- The number of rails is the key — always try small values (2, 3, 4)
- CyberChef handles this in seconds — use it as your first approach!
