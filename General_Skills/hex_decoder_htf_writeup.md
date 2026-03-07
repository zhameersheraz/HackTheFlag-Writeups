# Hex Decoder - Hack the Flag Writeup

**Challenge:** Hex Decoder  
**Category:** General Skills  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{h3x_d3c0d3d}`

---

## Description

Convert this hex string to ASCII text:

```
63 74 66 7b 68 33 78 5f 64 33 63 30 64 33 64 7d
```

---

## Solution

### Step 1: Recognize the Encoding

The numbers are all between `00` and `ff` and written in pairs — that's **hexadecimal** (base-16). Every pair represents one character.

### Step 2: Decode with CyberChef (My Method)

1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search for **"Magic"** and drag it into the Recipe
3. Paste the hex string into the Input box:
   ```
   63 74 66 7b 68 33 78 5f 64 33 63 30 64 33 64 7d
   ```
4. Click **BAKE!**
5. CyberChef Magic detected it as `From_Hex('Space')` and output:

```
ctf{h3x_d3c0d3d}
```

Got the flag! 🎯

---

## Why This Works

Hex is just another way to write numbers. Each hex pair maps to one ASCII character:

| Hex | ASCII |
|-----|-------|
| 63  | c     |
| 74  | t     |
| 66  | f     |
| 7b  | {     |
| 68  | h     |
| 33  | 3     |
| 78  | x     |
| 5f  | _     |
| 64  | d     |
| 33  | 3     |
| 63  | c     |
| 30  | 0     |
| 64  | d     |
| 33  | 3     |
| 64  | d     |
| 7d  | }     |

Reading them all together: `ctf{h3x_d3c0d3d}` ✅

---

## Alternative Methods

### Method 1: From Hex (direct, no Magic)
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search **"From Hex"** and drag it to Recipe
3. Paste the hex string → click BAKE!

### Method 2: RapidTables Hex to Text
1. Go to 👉 https://www.rapidtables.com/convert/number/hex-to-ascii.html
2. Paste the hex values
3. Click Convert → `ctf{h3x_d3c0d3d}`

### Method 3: dcode.fr
1. Go to 👉 https://www.dcode.fr/ascii-code
2. Paste the hex values
3. Select HEX decode → result shows the flag

### Method 4: Python
```python
hex_string = "63 74 66 7b 68 33 78 5f 64 33 63 30 64 33 64 7d"
flag = bytes.fromhex(hex_string.replace(" ", "")).decode()
print(flag)
```
Output:
```
ctf{h3x_d3c0d3d}
```

### Method 5: Kali Linux terminal
```bash
echo "63 74 66 7b 68 33 78 5f 64 33 63 30 64 33 64 7d" | xxd -r -p
```
Output:
```
ctf{h3x_d3c0d3d}
```

---

## Flag

```
ctf{h3x_d3c0d3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef Magic | Auto-detected hex encoding and decoded instantly |
| From Hex (CyberChef) | Direct hex-to-text conversion |
| Python | One-liner using `bytes.fromhex()` |

---

## Key Takeaways

- Hex values are always pairs of `0-9` and `a-f`
- Values `20–7e` are all printable ASCII characters (letters, numbers, symbols)
- `7b` = `{` and `7d` = `}` — the curly braces of the flag format
- CyberChef **Magic** is the fastest tool — it auto-detects the encoding for you
- `5f` = `_` (underscore) — useful to remember for leet-speak flags
