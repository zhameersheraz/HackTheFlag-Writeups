# RSA Basics — Hack the Flag Writeup

**Challenge:** RSA Basics  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 300 pts  
**Flag:** `ctf{m_value_65}`

---

## Description

Given RSA parameters: p=61, q=53, e=17, ciphertext c=2790. Find the plaintext m and wrap it as `ctf{m_value_}`. For example if m=42, flag is `ctf{m_value_42}`.

**Hint:** Calculate n=p*q, phi=(p-1)*(q-1), find d (modular inverse of e mod phi), then m = c^d mod n.

---

## Solution

### Step 1: Calculate n

`n` is the RSA modulus — multiply the two primes together:
```
n = p * q
n = 61 * 53
n = 3233
```

### Step 2: Calculate phi (φ)

`phi` is Euler's totient — tells us how many numbers are coprime to n:
```
phi = (p - 1) * (q - 1)
phi = (61 - 1) * (53 - 1)
phi = 60 * 52
phi = 3120
```

### Step 3: Find d (Private Key)

`d` is the modular inverse of `e` mod `phi` — this is the private key used to decrypt:
```
d = modular_inverse(e, phi)
d = modular_inverse(17, 3120)
d = 2753
```

### Step 4: Decrypt the Ciphertext

Now use the private key `d` to decrypt the ciphertext `c`:
```
m = c^d mod n
m = 2790^2753 mod 3233
m = 65
```

### Step 5: Write the Python Script

Run this in a Python interpreter to compute everything automatically:
```python
p = 61
q = 53
e = 17
c = 2790

n = p * q
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m = pow(c, d, n)

print(f"n   = {n}")
print(f"phi = {phi}")
print(f"d   = {d}")
print(f"m   = {m}")
print(f"Flag: ctf{{m_value_{m}}}")
```

**Output:**
```
n   = 3233
phi = 3120
d   = 2753
m   = 65
Flag: ctf{m_value_65}
```

✅ **Flag found!**

---

## Why This Works — RSA Explained Simply

RSA encryption works like a padlock:
- **Public key** (e, n) — anyone can use this to lock (encrypt) a message
- **Private key** (d, n) — only you can use this to unlock (decrypt) it

### The Math Behind It
```
Encrypt:  c = m^e mod n   ← uses public key e
Decrypt:  m = c^d mod n   ← uses private key d
```

The trick is that `d` and `e` are mathematical opposites — they cancel each other out.
Finding `d` requires knowing `phi`, and finding `phi` requires knowing `p` and `q`.
In real RSA, `p` and `q` are kept secret — but here they were given to us!

### Simple Breakdown
```
Given: p=61, q=53, e=17, c=2790
        ↓
n = p * q = 3233
        ↓
phi = (p-1) * (q-1) = 3120
        ↓
d = modular_inverse(17, 3120) = 2753
        ↓
m = pow(2790, 2753, 3233) = 65
        ↓
Flag: ctf{m_value_65}
```

---

## Step-by-Step Calculation Table

| Step | Formula | Values | Result |
|------|---------|--------|--------|
| Modulus | n = p × q | 61 × 53 | 3233 |
| Totient | phi = (p-1) × (q-1) | 60 × 52 | 3120 |
| Private key | d = e⁻¹ mod phi | 17⁻¹ mod 3120 | 2753 |
| Plaintext | m = c^d mod n | 2790^2753 mod 3233 | **65** |

---

## Flag
```
ctf{m_value_65}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python `pow(e, -1, phi)` | Calculate modular inverse to find private key d |
| Python `pow(c, d, n)` | Decrypt ciphertext using private key |

---

## Key Takeaways

1. **RSA decryption requires 4 steps** — calculate n, calculate phi, find d, then decrypt with m = c^d mod n
2. **`pow(e, -1, phi)` in Python** — built-in way to find the modular inverse, no external libraries needed
3. **`pow(c, d, n)` in Python** — efficiently computes huge modular exponentiations in one line
4. **If p and q are known, RSA is broken** — the entire security of RSA relies on keeping p and q secret; once you have them, decryption is trivial
5. **phi = (p-1)(q-1)** — this is always the formula for RSA with two primes, memorize it
