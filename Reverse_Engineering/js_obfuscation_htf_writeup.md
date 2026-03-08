# JS Obfuscation - Hack the Flag Writeup

**Challenge:** JS Obfuscation  
**Category:** Reverse Engineering  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{js_d3obfuscat3d}`

---

## Description

Deobfuscate this JavaScript to find the flag:

```javascript
var _0x=['63','74','66','7b','6a','73','5f','64','33','6f','62','66','75','73','63','61','74','33','64','7d'];
var flag='';
for(var i=0;i<_0x.length;i++){
    flag+=String.fromCharCode(parseInt(_0x[i],16));
}
// flag is now set
```

---

## Hints

1. The array contains hex values of ASCII characters. Convert each hex to a character.

---

## Solution

### Step 1: Understand What the Code Does

The code looks scary but it's actually doing something very simple — just building a string letter by letter from hex values.

Let me translate it into plain English:

```
1. There's a list of hex numbers: ['63','74','66','7b', ...]
2. Start with an empty flag: flag = ''
3. Loop through each hex number:
   - Convert hex to decimal
   - Convert decimal to its ASCII character
   - Add that character to the flag
4. After the loop, flag = the full string
```

It's just the **Hex Decoder** challenge in disguise, but written as JavaScript code instead!

### Step 2: Decode Each Hex Value

Every item in the array is a hex value that maps to one character:

| Hex | Decimal | Character |
|-----|---------|-----------|
| 63  | 99      | c         |
| 74  | 116     | t         |
| 66  | 102     | f         |
| 7b  | 123     | {         |
| 6a  | 106     | j         |
| 73  | 115     | s         |
| 5f  | 95      | _         |
| 64  | 100     | d         |
| 33  | 51      | 3         |
| 6f  | 111     | o         |
| 62  | 98      | b         |
| 66  | 102     | f         |
| 75  | 117     | u         |
| 73  | 115     | s         |
| 63  | 99      | c         |
| 61  | 97      | a         |
| 74  | 116     | t         |
| 33  | 51      | 3         |
| 64  | 100     | d         |
| 7d  | 125     | }         |

Reading all characters together: **`ctf{js_d3obfuscat3d}`** ✅

### Step 3: Run It in the Browser Console (My Method)

The easiest way — just run the code itself!

1. Press `F12` to open Browser DevTools
2. Click the **Console** tab
3. Paste the code and press Enter:

```javascript
var _0x=['63','74','66','7b','6a','73','5f','64','33','6f','62','66','75','73','63','61','74','33','64','7d'];
var flag='';
for(var i=0;i<_0x.length;i++){
    flag+=String.fromCharCode(parseInt(_0x[i],16));
}
console.log(flag);
```

Output:
```
ctf{js_d3obfuscat3d}
```

Got the flag! 🎯

---

## Why This Works

### What is Obfuscation?

**Obfuscation** means hiding what code does by making it look confusing — using weird variable names like `_0x`, splitting strings into arrays, encoding text as numbers, etc.

But obfuscated code still HAS to work — so if you just **run it**, it decodes itself for you!

### What does `String.fromCharCode(parseInt(_0x[i], 16))` mean?

Breaking it apart:

```javascript
parseInt('63', 16)
// parseInt(value, base) converts a string to a number
// base 16 = hexadecimal
// '63' in hex = 99 in decimal

String.fromCharCode(99)
// Converts ASCII number to character
// 99 = 'c'

// So: '63' → 99 → 'c'
```

The code just loops through every hex value and converts it to a character!

### Visual of What Happens

```
['63','74','66','7b', ...]
   ↓    ↓    ↓    ↓
   c    t    f    {    ...  → ctf{js_d3obfuscat3d}
```

---

## Alternative Methods

### Method 1: CyberChef
1. Go to 👉 https://gchq.github.io/CyberChef/
2. Search **"From Hex"** and drag to Recipe
3. Paste the hex values (without quotes, space-separated):
   ```
   63 74 66 7b 6a 73 5f 64 33 6f 62 66 75 73 63 61 74 33 64 7d
   ```
4. Click BAKE! → `ctf{js_d3obfuscat3d}`

### Method 2: Python
```python
hex_values = ['63','74','66','7b','6a','73','5f','64','33','6f','62','66','75','73','63','61','74','33','64','7d']
flag = ''.join(chr(int(h, 16)) for h in hex_values)
print(flag)
```
Output:
```
ctf{js_d3obfuscat3d}
```

### Method 3: Kali Linux
```bash
echo "63 74 66 7b 6a 73 5f 64 33 6f 62 66 75 73 63 61 74 33 64 7d" | xxd -r -p
```
Output:
```
ctf{js_d3obfuscat3d}
```

---

## Flag

```
ctf{js_d3obfuscat3d}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Browser Console (F12) | Run the obfuscated JS directly — it decodes itself! |
| CyberChef From Hex | Paste hex values and decode |
| Python | One-liner to convert hex array to string |

---

## Key Takeaways

- **Obfuscation** hides what code does but doesn't change what it does — just run it!
- `parseInt('63', 16)` converts hex string to decimal number
- `String.fromCharCode(99)` converts decimal to ASCII character
- The fastest way to deobfuscate JS is always to run it in **browser console (F12)**
- Arrays of hex values in JS = almost always a hidden string — decode them!
