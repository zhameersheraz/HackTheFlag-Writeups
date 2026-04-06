# Java Bytecode — Hack the Flag Writeup

**Challenge:** Java Bytecode  
**Category:** Reverse Engineering  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{ljavacode;}`  

---

## Description

A Java class file contains this constant pool string: `4C6A617661436F64653B` (hex). The flag is `ctf{decoded_string_lowercase}`. Decode the hex to find the Java descriptor.

---

## Hints

> Convert each pair of hex digits to ASCII. `4C="L"`, `6A="j"`, `61="a"`, `76="v"`, etc.

---

## Solution

The hex string `4C6A617661436F64653B` is the raw bytes of a Java class constant — we just need to decode each byte pair to its ASCII character.

### Step 1: Split into byte pairs

```
4C  6A  61  76  61  43  6F  64  65  3B
```

### Step 2: Convert each hex byte to ASCII

| Hex | Decimal | Character |
|-----|---------|-----------|
| `4C` | 76 | **L** |
| `6A` | 106 | **j** |
| `61` | 97 | **a** |
| `76` | 118 | **v** |
| `61` | 97 | **a** |
| `43` | 67 | **C** |
| `6F` | 111 | **o** |
| `64` | 100 | **d** |
| `65` | 101 | **e** |
| `3B` | 59 | **;** |

### Step 3: Read the decoded string

```
L j a v a C o d e ;
→ LjavaCode;
```

### Step 4: Lowercase (as the flag format requires)

```
LjavaCode; → ljavacode;
```

### Step 5: Use CyberChef (quickest method)

1. Go to 👉 https://gchq.github.io/CyberChef/
2. Paste `4C6A617661436F64653B` into **Input**
3. Search **From Hex** → drag into Recipe
4. Click **BAKE!**

Output: `LjavaCode;` → lowercase it → `ljavacode;` ✅

### Step 6: Python one-liner

```bash
python3 -c "print(bytes.fromhex('4C6A617661436F64653B').decode())"
# LjavaCode;
```

```bash
python3 -c "print(bytes.fromhex('4C6A617661436F64653B').decode().lower())"
# ljavacode;
```

---

## What is a Java Type Descriptor?

In Java `.class` files, types are stored in a compact **descriptor format**:

| Descriptor | Java Type |
|-----------|-----------|
| `I` | int |
| `Z` | boolean |
| `Ljava/lang/String;` | String (object) |
| `LjavaCode;` | A class named `javaCode` ← this challenge |

The `L` prefix means "class reference", and the `;` ends the descriptor. This is how the JVM (Java Virtual Machine) stores type information internally in bytecode.

The `javap -c` tool disassembles Java `.class` files and shows these descriptors in the constant pool.

---

## Flag

```
ctf{ljavacode;}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| CyberChef (From Hex) | Instant hex-to-ASCII decode |
| `python3` | One-liner decode on command line |
| `javap -c` | Disassemble real Java `.class` files |

---

## Key Takeaways

- Java bytecode stores strings as **raw hex bytes** in the constant pool
- Convert hex → each 2 hex digits = 1 ASCII character
- Java type descriptors: `L<classname>;` means a class reference, `;` always terminates it
- The flag format says lowercase — always check whether to lowercase the decoded string!
