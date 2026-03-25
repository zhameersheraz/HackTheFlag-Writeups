# Logic Gate - Hack the Flag Writeup

**Challenge:** Logic Gate  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{result_1}`

---

## Description

Given inputs A=1, B=0: What is the output of `(A XOR B) AND (A OR B)`? Submit as `ctf{result_<0_or_1>}`.

---

## Hints

1. XOR: 1 XOR 0 = 1. OR: 1 OR 0 = 1. AND: 1 AND 1 = 1.

---

## Solution

### Step 1: Solve A XOR B

```
A = 1, B = 0
A XOR B = 1 XOR 0 = 1
```

XOR (Exclusive OR) outputs **1** only when the inputs are **different**.

### Step 2: Solve A OR B

```
A = 1, B = 0
A OR B = 1 OR 0 = 1
```

OR outputs **1** when **at least one** input is 1.

### Step 3: Combine with AND

Now take the results of both operations and AND them together:

```
(A XOR B) AND (A OR B)
=     1    AND     1
=              1
```

AND outputs **1** only when **both** inputs are 1. Since both are 1, the result is **1**.

### Step 4: Build the Flag

```
ctf{result_1}
```

Got the flag! 🎯

---

## Why This Works — Logic Gates

### Truth Tables for Each Gate

**XOR (Exclusive OR) — outputs 1 only when inputs are DIFFERENT:**

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | **1** ← our case |
| 1 | 1 | 0 |

**OR — outputs 1 when AT LEAST ONE input is 1:**

| A | B | A OR B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | **1** ← our case |
| 1 | 1 | 1 |

**AND — outputs 1 only when BOTH inputs are 1:**

| A | B | A AND B |
|---|---|---------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | **1** ← our case (1 AND 1) |

### Visual of the Calculation

```
A=1, B=0

Step 1:  A XOR B  →  1 XOR 0  =  1
Step 2:  A OR  B  →  1 OR  0  =  1
Step 3:  1   AND  1           =  1

(A XOR B) AND (A OR B) = 1  →  ctf{result_1}  🎯
```

---

## Alternative Methods

```python
# Python — verify the answer
A = 1
B = 0

xor_result = A ^ B      # XOR in Python uses ^
or_result  = A | B      # OR  in Python uses |
final      = xor_result & or_result  # AND in Python uses &

print(f"XOR: {xor_result}")   # 1
print(f"OR:  {or_result}")    # 1
print(f"AND: {final}")        # 1
print(f"Flag: ctf{{result_{final}}}")
# Output: ctf{result_1}
```

---

## Logic Gate Quick Reference

| Gate | Symbol | Rule | Example (1,0) |
|------|--------|------|---------------|
| AND | `&` | Both must be 1 | 1 AND 0 = 0 |
| OR | `\|` | At least one must be 1 | 1 OR 0 = 1 |
| XOR | `^` | Inputs must be different | 1 XOR 0 = 1 |
| NOT | `~` | Flips the bit | NOT 1 = 0 |
| NAND | | AND then NOT | 1 NAND 0 = 1 |
| NOR | | OR then NOT | 1 NOR 0 = 0 |
| XNOR | | XOR then NOT | 1 XNOR 0 = 0 |

---

## Flag

```
ctf{result_1}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Logic gate rules | Evaluated XOR, OR, and AND step by step with A=1, B=0 |

---

## Key Takeaways

- **XOR** = inputs must be **different** to output 1 (think: "one or the other but not both")
- **OR** = **at least one** input must be 1
- **AND** = **both** inputs must be 1
- Always solve inner brackets first: `(A XOR B)` and `(A OR B)` before the final AND
- In Python: XOR=`^`, OR=`|`, AND=`&`, NOT=`~`
- Logic gates are the building blocks of all digital circuits and appear frequently in CTF Misc/Crypto challenges
