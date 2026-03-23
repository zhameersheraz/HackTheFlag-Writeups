# Number Station - Hack the Flag Writeup

**Challenge:** Number Station  
**Category:** Cryptography  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{numbers_station}`

---

## Description

An intercepted number station broadcast contained: `3 20 6 { 14 21 13 2 5 18 19 _ 19 20 1 20 9 15 14 }`. Each number represents a letter position (A=1, B=2, …, Z=26). Decode the transmission.

---

## Hints

1. Map each number to the alphabet: 1=A, 2=B, 3=C, and so on up to 26=Z.

---

## Solution

### Step 1: Write Out the Alphabet Key

The encoding is simple — each number maps directly to its position in the alphabet:

```
A=1   B=2   C=3   D=4   E=5   F=6   G=7   H=8   I=9   J=10
K=11  L=12  M=13  N=14  O=15  P=16  Q=17  R=18  S=19  T=20
U=21  V=22  W=23  X=24  Y=25  Z=26
```

Special characters `{`, `}`, and `_` are passed through as-is (they're not encoded).

### Step 2: Decode Each Number

Going through the transmission one by one:

```
3  → C
20 → T
6  → F
{  → {   (kept as-is)
14 → N
21 → U
13 → M
2  → B
5  → E
18 → R
19 → S
_  → _   (kept as-is)
19 → S
20 → T
1  → A
20 → T
9  → I
15 → O
14 → N
}  → }   (kept as-is)
```

### Step 3: Put it Together

```
C T F { N U M B E R S _ S T A T I O N }
= ctf{numbers_station}
```

Got the flag! 🎯

---

## Why This Works — Number-to-Letter Substitution

This is one of the most basic ciphers — a **simple alphabetic substitution** where letters are replaced by their position number:

```
A=1, B=2, C=3 ... Z=26
```

It's essentially the same as the A1Z26 cipher. The "number station" theme references real-world **shortwave radio number stations** — mysterious broadcasts of number sequences used historically for spy communications.

### Visual of the Decode

```
Transmission:  3  20  6  {  14  21  13   2   5  18  19  _  19  20   1  20   9  15  14  }
               ↓   ↓  ↓  ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓  ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓  ↓
Decoded:       C   T  F  {   N   U   M   B   E   R   S  _   S   T   A   T   I   O   N  }
```

---

## Alternative Methods

```python
# Python solution
transmission = [3, 20, 6, '{', 14, 21, 13, 2, 5, 18, 19, '_', 19, 20, 1, 20, 9, 15, 14, '}']

flag = ''
for x in transmission:
    if isinstance(x, int):
        flag += chr(x + 64)  # 1=A (65 in ASCII), so +64
    else:
        flag += x

print(flag.lower())
# Output: ctf{numbers_station}
```

```python
# Even simpler one-liner
nums = [3, 20, 6, 14, 21, 13, 2, 5, 18, 19, 19, 20, 1, 20, 9, 15, 14]
print('ctf{' + ''.join(chr(n + 96) for n in nums[:3]) + ... + '}')
```

---

## Flag

```
ctf{numbers_station}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Alphabet mapping (A=1 to Z=26) | Converted each number to its corresponding letter |

---

## Key Takeaways

- **A1Z26** is one of the simplest ciphers — just map numbers 1–26 to letters A–Z
- Always check if `{`, `}`, and `_` are passed through unchanged (they usually are in CTF flags)
- In Python, `chr(n + 64)` converts a number (1–26) to its uppercase letter (A–Z)
- Or use `chr(n + 96)` for lowercase (a–z)
- Real "number stations" were Cold War-era shortwave broadcasts used for spy communications — the challenge name is a reference to that!
