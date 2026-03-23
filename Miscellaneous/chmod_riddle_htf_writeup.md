# chmod Riddle - HackTheFlag Writeup

**Challenge:** chmod Riddle  
**Category:** Miscellaneous  
**Difficulty:** Medium  
**Points:** 150  
**Flag:** `ctf{octal_750}`

---

## Description

A Linux file has permissions `rwxr-x---`. What is this in octal notation? Submit as `ctf{octal_}`.

> **Hint:** Each group of three (rwx) converts to a digit: r=4, w=2, x=1. Add them up per group.

---

## Tool Used

- Brain 🧠 (math challenge — no tools needed)

---

## Solution

### Step 1: Understand the Permission Format

In Linux, file permissions are shown as 9 characters split into **3 groups of 3**:

```
rwxr-x---
│││├──┤└──┘
│││  │   └── Other  (everyone else)
│││  └─────── Group  (group members)
│└┴─────────── Owner  (the file owner)
```

So `rwxr-x---` breaks down like this:

```
rwx  r-x  ---
 ↓    ↓    ↓
Owner Group Other
```

---

### Step 2: Learn the Values

Each letter has a number value:

| Letter | Meaning | Value |
|--------|---------|-------|
| `r` | read | **4** |
| `w` | write | **2** |
| `x` | execute | **1** |
| `-` | no permission | **0** |

---

### Step 3: Calculate Each Group

**Owner → `rwx`**
```
r + w + x = 4 + 2 + 1 = 7
```

**Group → `r-x`**
```
r + - + x = 4 + 0 + 1 = 5
```

**Other → `---`**
```
- + - + - = 0 + 0 + 0 = 0
```

---

### Step 4: Combine the Numbers

Put the 3 digits together:
```
Owner  Group  Other
  7      5      0
       = 750
```

---

### Step 5: Submit the Flag

Wrap it in the flag format:
```
ctf{octal_750}
```

✅ **Correct! +150 points** 🎯

---

## Quick Visual Summary

```
r w x  r - x  - - -
4+2+1  4+0+1  0+0+0
  7      5      0
         ↓
       750
```

---

## Key Takeaways

- Linux permissions have **3 groups**: Owner, Group, Other
- Each group has 3 bits: **r=4, w=2, x=1**
- Add them up per group to get the octal digit
- `rwxr-x---` = **750** — owner can do everything, group can read/execute, others can do nothing
- This is the same number you use in `chmod 750 filename` in the terminal!
