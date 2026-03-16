# C Decompile — Hack the Flag Writeup

**Challenge:** C Decompile  
**Category:** Reverse Engineering  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{d3c0mp1l3d}`

---

## Description

A compiled C program checks the flag like this:
```c
int check(char *input) {
    char key[] = {0x63,0x74,0x66,0x7b,0x64,0x33,0x63,0x30,0x6d,0x70,0x31,0x6c,0x33,0x64,0x7d};
    return memcmp(input, key, 15) == 0;
}
```

What is the flag?

**Hint:** Each hex value is an ASCII character. Convert them: 0x63="c", 0x74="t", 0x66="f", etc.

---

## Solution

### Step 1: Extract the Hex Values

Looking at the `key[]` array in the C code, we can see 15 hex values hardcoded inside it:
```
0x63, 0x74, 0x66, 0x7b, 0x64, 0x33, 0x63, 0x30,
0x6d, 0x70, 0x31, 0x6c, 0x33, 0x64, 0x7d
```

The program uses `memcmp` to compare the user's input against this key — meaning **this hex array IS the flag**.

### Step 2: Convert Hex to ASCII using CyberChef

1. Go to **[CyberChef](https://gchq.github.io/CyberChef/)**
2. Input the hex values (remove the `0x` prefix and commas):
```
63 74 66 7b 64 33 63 30 6d 70 31 6c 33 64 7d
```
3. Add recipe: **From Hex**
4. Click **Bake!**

**Output:**
```
ctf{d3c0mp1l3d}
```

✅ **Flag found!**

---

## Why This Works

The C code stores the flag as an array of hex values and compares it directly against the user input using `memcmp`. Since the hex values are hardcoded in the source code, we just need to convert them to readable ASCII characters to get the flag.

Each hex value maps directly to one ASCII character:

| Hex | ASCII |
|-----|-------|
| 0x63 | c |
| 0x74 | t |
| 0x66 | f |
| 0x7b | { |
| 0x64 | d |
| 0x33 | 3 |
| 0x63 | c |
| 0x30 | 0 |
| 0x6d | m |
| 0x70 | p |
| 0x31 | 1 |
| 0x6c | l |
| 0x33 | 3 |
| 0x64 | d |
| 0x7d | } |

Result: `ctf{d3c0mp1l3d}`

### Simple Breakdown
```
Read key[] array from C code
        ↓
Extract hex values:
63 74 66 7b 64 33 63 30 6d 70 31 6c 33 64 7d
        ↓
CyberChef → From Hex
        ↓
ctf{d3c0mp1l3d}
```

---

## CyberChef Recipe Summary
```
Input:   63 74 66 7b 64 33 63 30 6d 70 31 6c 33 64 7d
Recipe:  From Hex
Output:  ctf{d3c0mp1l3d}
```

---

## Flag
```
ctf{d3c0mp1l3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef From Hex | Convert hex values to ASCII characters |

---

## Key Takeaways

1. **Hardcoded keys in C are never safe** — anything stored in `key[]` or compared with `memcmp` is directly readable from the source or binary
2. **`0x` prefix means hex** — always strip `0x` and commas before pasting into CyberChef From Hex
3. **`memcmp` is a red flag in CTFs** — it means the program is doing a direct byte-by-byte comparison against a hardcoded value — that value is always the flag
4. **Hex to ASCII is one of the most common CTF conversions** — CyberChef From Hex solves it instantly
