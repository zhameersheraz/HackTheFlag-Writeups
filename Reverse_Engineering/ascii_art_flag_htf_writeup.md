# ASCII Art Flag - Hack the Flag Writeup

**Challenge:** ASCII Art Flag  
**Category:** Reverse Engineering  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ctf{4sc11_4rt}`

---

## Description

The flag is encoded as decimal ASCII values separated by spaces:
`99 116 102 123 52 115 99 49 49 95 52 114 116 125`

---

## Hints

1. Convert each decimal number to its ASCII character. For example: 99="c", 116="t", 102="f".

---

## Solution

### Step 1: Understand the Encoding

The numbers are **decimal ASCII values**. Each number represents one character using the ASCII table.

### Step 2: Decode Using CyberChef (My Method)

1. Go to https://gchq.github.io/CyberChef/
2. Search for **"Magic"** and drag it into the Recipe
3. Paste the numbers in the Input box:
   ```
   99 116 102 123 52 115 99 49 49 95 52 114 116 125
   ```
4. Click **BAKE!**
5. Found the result under `From_Decimal('Space',false)`

**Output:**
```
ctf{4sc11_4rt}
```

Got the flag! 🎯

---

## Why This Works

### What is ASCII?

**ASCII** is a standard that assigns a number to every character. For example:

| Decimal | Character |
|---------|-----------|
| 99 | c |
| 116 | t |
| 102 | f |
| 123 | { |
| 52 | 4 |
| 115 | s |
| 99 | c |
| 49 | 1 |
| 49 | 1 |
| 95 | _ |
| 52 | 4 |
| 114 | r |
| 116 | t |
| 125 | } |

Reading them together gives: `ctf{4sc11_4rt}`

### Simple Breakdown

```
Get decimal numbers: 99 116 102 123 ...
      |
      v
Convert each number to its ASCII character
      |
      v
Read all characters together
      |
      v
Flag: ctf{4sc11_4rt}
```

---

## Alternative Method 1: dcode.fr

1. Go to https://www.dcode.fr/ascii-code
2. Paste the numbers in the input box
3. Click **Decrypt/Convert ASCII**
4. Result shows `ctf{4sc11_4rt}` under DEC

## Alternative Method 2: Python

```python
numbers = [99, 116, 102, 123, 52, 115, 99, 49, 49, 95, 52, 114, 116, 125]
flag = ''.join([chr(n) for n in numbers])
print(flag)
```

**Output:**
```
ctf{4sc11_4rt}
```

---

## Flag

```
ctf{4sc11_4rt}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef (Magic) | Automatically detect and decode decimal ASCII |
| dcode.fr | Alternative ASCII decoder |

---

## Key Takeaways

- Decimal ASCII encoding converts each character to its number value
- CyberChef Magic can automatically detect decimal ASCII encoding
- Numbers between 32 and 126 are printable ASCII characters
- `123` = `{` and `125` = `}` are the curly braces used in the flag format
