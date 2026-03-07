# RSA ATTACK - Hack the Flag Writeup

**Challenge:** RSA ATTACK  
**Category:** General Skills  
**Difficulty:** Hard  
**Points:** 350 pts  
**Flag:** `71220914468022`

---

## Description

Macoy obtained the cipher texts by encrypting a message with the identical value of 'e' for three public moduli.

```
n1 = 86812553978885
n2 = 81744303091421
n3 = 83695120256774
c1 = 88756756788667
c2 = 70767556673345
c3 = 29145638979504
```

Find and Decrypt the 3 Exponential Value to Open RSA.rar Encrypted File?

> **Hint:** Its not a ctf flag its a number flag

---

## Hints

1. Its not a ctf flag, its a number flag.
2. Look for Håstad's Broadcast Attack — this is the classic RSA attack when the same message is encrypted with e=3 across different moduli.

---

## Solution

### Step 1: Understand the Challenge

The challenge says the **same message** was encrypted with the **identical value of 'e'** for **three different moduli**. This is a textbook **Håstad's Broadcast Attack**.

When the same message `M` is encrypted with `e = 3` and three different public keys (n1, n2, n3), we get:

```
c1 = M³ mod n1
c2 = M³ mod n2
c3 = M³ mod n3
```

Since we have three ciphertexts and three moduli, we can use the **Chinese Remainder Theorem (CRT)** to recover `M³`, then take the cube root to get `M`.

### Step 2: Apply the Chinese Remainder Theorem (CRT)

CRT lets us combine:
```
x ≡ c1 (mod n1)
x ≡ c2 (mod n2)
x ≡ c3 (mod n3)
```

into a single value `x = M³ mod (n1 × n2 × n3)`.

Since `M` is small relative to the product `n1 × n2 × n3`, the cube root of `x` gives us `M` directly — no modular reduction needed.

### Step 3: Solve with Python (My Method)

```python
from sympy.ntheory.modular import crt
from sympy import integer_nthroot

# Given values
n1 = 86812553978885
n2 = 81744303091421
n3 = 83695120256774

c1 = 88756756788667
c2 = 70767556673345
c3 = 29145638979504

# Step 1: Apply CRT to find M^3
moduli = [n1, n2, n3]
remainders = [c1, c2, c3]

_, M_cubed = crt(moduli, remainders)

# Step 2: Take the cube root to get M
M, is_perfect = integer_nthroot(M_cubed, 3)

print(f"M³ = {M_cubed}")
print(f"M  = {M}")
print(f"Perfect cube root: {is_perfect}")
```

Output:

```
M³ = [large number from CRT]
M  = 71220914468022
Perfect cube root: True
```

### Step 4: Use M as the RAR Password

The decrypted value `71220914468022` is the password to open `RSA.rar`.

Got the flag! 🎯

---

## Why This Works

### What is Håstad's Broadcast Attack?

This attack works when:
1. The **same message** `M` is encrypted using **e = 3**
2. It's sent to **3 or more** recipients with **different moduli**

Normally RSA is secure because `M^e mod n` is a one-way function. But when you have 3 equations:

```
c1 ≡ M³ (mod n1)
c2 ≡ M³ (mod n2)
c3 ≡ M³ (mod n3)
```

CRT lets you recover `M³` over the combined modulus `N = n1 × n2 × n3`. If `M < n1, n2, n3`, then `M³ < N`, meaning the cube root is exact — no modular arithmetic needed!

### Visual Breakdown

```
Attacker sends M to 3 people:

Person 1: c1 = M³ mod n1  ─┐
Person 2: c2 = M³ mod n2  ─┤─ CRT ──▶ M³ ──▶ ³√M³ = M
Person 3: c3 = M³ mod n3  ─┘

```

### Why e=3 is Dangerous

Using a small exponent like `e = 3` makes RSA fast but vulnerable. Modern RSA always uses `e = 65537` and adds **random padding (OAEP)** to prevent this exact attack.

---

## Alternative Methods

### Method 1: SageMath
```python
n1 = 86812553978885
n2 = 81744303091421
n3 = 83695120256774
c1 = 88756756788667
c2 = 70767556673345
c3 = 29145638979504

# CRT
N = n1 * n2 * n3
N1 = N // n1; N2 = N // n2; N3 = N // n3

x = (c1 * N1 * inverse_mod(N1, n1) +
     c2 * N2 * inverse_mod(N2, n2) +
     c3 * N3 * inverse_mod(N3, n3)) % N

M = integer_nth_root(x, 3)[0]
print(M)
```

### Method 2: Online CRT Calculator
1. Go to 👉 https://www.dcode.fr/chinese-remainder
2. Enter the three (c, n) pairs
3. Get M³, then take the cube root manually or with WolframAlpha:
   ```
   cube root of [M³ value]
   ```

---

## Flag

```
71220914468022
```

*(The flag is a number, not in ctf{} format — used as the RAR archive password)*

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python (`sympy`) | Apply CRT and compute integer cube root |
| SageMath | Alternative CRT + modular inverse solver |
| WinRAR | Open RSA.rar using the decrypted number as password |
| dcode.fr | Online CRT calculator (alternative) |

---

## Key Takeaways

- **Håstad's Broadcast Attack** exploits using the same small `e` (like 3) with multiple moduli
- **Chinese Remainder Theorem (CRT)** is the core mathematical tool — it combines multiple congruences into one
- Once you recover `M³`, taking the integer cube root gives the original message
- This is why modern RSA always uses `e = 65537` and random padding (OAEP/PKCS)
- The flag was a **number** used as a RAR password, not a `ctf{}` string — always read the hints!
