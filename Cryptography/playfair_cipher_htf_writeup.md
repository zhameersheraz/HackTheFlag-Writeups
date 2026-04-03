# Playfair Cipher — Hack the Flag Writeup

**Challenge:** Playfair Cipher  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 300 pts  
**Flag:** `ctf{instruments}`  

---

## Description

The Playfair cipher uses a 5×5 grid. With the keyword **MONARCHY**, decrypt: **GATLMZCLRQTX**. Submit as `ctf{decrypted_lowercase}`.

---

## Hints

> Build the 5×5 Playfair grid with MONARCHY, then decrypt each digraph pair using Playfair rules.

---

## Solution

### Step 1: Build the 5×5 Playfair grid

Write the keyword (remove duplicates, J merges with I), then fill remaining alphabet letters in order:

```
MONARCHY → M O N A R C H Y B D E F G I K L P Q S T U V W X Z
```

Grid:
```
M  O  N  A  R
C  H  Y  B  D
E  F  G  I  K
L  P  Q  S  T
U  V  W  X  Z
```

### Step 2: Playfair Decryption Rules

- **Same row** → each letter shifts **LEFT** by 1 (wraps around)
- **Same column** → each letter shifts **UP** by 1 (wraps around)
- **Rectangle** → each letter moves to the same row but the **other letter's column**

### Step 3: Decrypt each digraph — GATLMZCLRQTX

Split into pairs: **GA · TL · MZ · CL · RQ · TX**

| Digraph | Positions | Rule | Plaintext |
|---------|-----------|------|-----------|
| **GA** | G(2,2), A(0,3) | Rectangle | (2,3)=I, (0,2)=N → **IN** |
| **TL** | T(3,4), L(3,0) | Same row → shift left | T→S, L→T → **ST** |
| **MZ** | M(0,0), Z(4,4) | Rectangle | (0,4)=R, (4,0)=U → **RU** |
| **CL** | C(1,0), L(3,0) | Same col → shift up | C→M, L→E → **ME** |
| **RQ** | R(0,4), Q(3,2) | Rectangle | (0,2)=N, (3,4)=T → **NT** |
| **TX** | T(3,4), X(4,3) | Rectangle | (3,3)=S, (4,4)=Z → **SZ** |

### Step 4: Read the plaintext

```
IN + ST + RU + ME + NT + SZ = INSTRUMENTSZ
```

The trailing **Z** is padding (added to make an even number of characters). Strip it:

**Plaintext = INSTRUMENTS** ✅

### Step 5: Use dcode.fr (quickest method)

1. Go to 👉 https://www.dcode.fr/playfair-cipher
2. Set **Mode** → Decrypt
3. Key = `MONARCHY`
4. Ciphertext = `GATLMZCLRQTX`
5. Click **Decrypt**

Output: `INSTRUMENTSZ` → remove padding Z → `INSTRUMENTS` ✅

### Python solver

```python
def build_playfair_grid(keyword):
    keyword = keyword.upper().replace('J','I')
    seen = []
    for ch in keyword:
        if ch not in seen and ch.isalpha():
            seen.append(ch)
    for ch in 'ABCDEFGHIKLMNOPQRSTUVWXYZ':
        if ch not in seen:
            seen.append(ch)
    grid = [seen[i*5:(i+1)*5] for i in range(5)]
    pos = {ch: (r, c) for r, row in enumerate(grid) for c, ch in enumerate(row)}
    return grid, pos

def playfair_decrypt(cipher, keyword):
    grid, pos = build_playfair_grid(keyword)
    digraphs = [cipher[i:i+2] for i in range(0, len(cipher), 2)]
    plain = []
    for a, b in digraphs:
        ra, ca = pos[a]
        rb, cb = pos[b]
        if ra == rb:
            plain += [grid[ra][(ca-1)%5], grid[rb][(cb-1)%5]]
        elif ca == cb:
            plain += [grid[(ra-1)%5][ca], grid[(rb-1)%5][cb]]
        else:
            plain += [grid[ra][cb], grid[rb][ca]]
    return ''.join(plain)

result = playfair_decrypt("GATLMZCLRQTX", "MONARCHY")
print(result)                        # INSTRUMENTSZ
print(result.rstrip('Z').lower())    # instruments
```

```bash
python3 playfair.py
# INSTRUMENTSZ
# instruments
```

---

## Flag

```
ctf{instruments}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| dcode.fr/playfair-cipher | Automatic Playfair decrypt with keyword |
| Python | Custom grid builder + decryptor |

---

## Key Takeaways

- Playfair uses a 5×5 grid keyed by a keyword; **I and J share one cell**
- Decryption uses LEFT (same row), UP (same col), or rectangle swap rules
- Trailing X or Z at the end of plaintext is almost always **padding** — strip it
- Playfair was used by British forces in WWI and WWII
