# Assembly Puzzle — Hack the Flag Writeup

**Challenge:** Assembly Puzzle  
**Category:** Reverse Engineering  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{value_15}`  

---

## Description

What value is in the EAX register after these x86 instructions?

```
mov eax, 5
add eax, 3
imul eax, 2
sub eax, 1
```

Submit as `ctf{value_}`.

---

## Hints

> Trace through line by line: start with 5, add 3 (=8), multiply by 2 (=16), subtract 1 (=15).

---

## Solution

x86 assembly instructions operate on **registers** — small storage locations inside the CPU. The register here is `EAX`, a general-purpose 32-bit register commonly used to store return values and intermediate results.

### Step 1: Understand each instruction

| Instruction | Meaning |
|-------------|---------|
| `mov eax, 5` | Set EAX = 5 |
| `add eax, 3` | EAX = EAX + 3 |
| `imul eax, 2` | EAX = EAX × 2 |
| `sub eax, 1` | EAX = EAX − 1 |

### Step 2: Trace line by line

```
mov eax, 5    → EAX = 5
add eax, 3    → EAX = 5 + 3  = 8
imul eax, 2   → EAX = 8 × 2  = 16
sub eax, 1    → EAX = 16 - 1 = 15
```

**Final value in EAX = 15** ✅

### Step 3: Verify with Python

```python
eax = 5    # mov eax, 5
eax += 3   # add eax, 3
eax *= 2   # imul eax, 2
eax -= 1   # sub eax, 1
print(eax) # 15
```

```bash
python3 -c "eax=5; eax+=3; eax*=2; eax-=1; print(eax)"
# 15
```

### Step 4: Build the flag

```
value = 15
flag  = ctf{value_15}
```

---

## x86 Register Cheat Sheet

| Register | Size | Common Use |
|----------|------|-----------|
| `EAX` | 32-bit | Return values, arithmetic |
| `EBX` | 32-bit | General purpose |
| `ECX` | 32-bit | Loop counter |
| `EDX` | 32-bit | Data / I/O |

## Common x86 Arithmetic Instructions

| Instruction | Operation |
|-------------|-----------|
| `mov dst, src` | dst = src (assignment) |
| `add dst, val` | dst = dst + val |
| `sub dst, val` | dst = dst − val |
| `imul dst, val` | dst = dst × val (signed) |
| `idiv divisor` | EAX = EAX ÷ divisor |
| `inc dst` | dst = dst + 1 |
| `dec dst` | dst = dst − 1 |

---

## Flag

```
ctf{value_15}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python | Simulate register arithmetic |
| Mental math / pen & paper | Trace instructions manually |

---

## Key Takeaways

- x86 assembly operates on **registers** — think of them as variables inside the CPU
- Trace instructions **one line at a time**, keeping track of the register value
- `mov` = assignment, `add/sub/imul` = math operations
- Python is a great way to simulate simple assembly arithmetic quickly
