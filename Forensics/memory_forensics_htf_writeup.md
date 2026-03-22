# Memory Forensics - Hack the Flag Writeup

**Challenge:** Memory Forensics  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 150 pts  
**Flag:** `ctf{v0l4t1l3_m3m0ry}`

---

## Description

In memory forensics, the Volatility framework is essential. A memory dump shows a process named `notepad.exe` with PID 1337 had a string "flag=ctf{v0l4t1l3_m3m0ry}" in its address space. What is the flag?

---

## Hints

1. The flag is mentioned directly in the process memory. Read the challenge description carefully.

---

## Solution

### Step 1: Read the Description Carefully

The challenge description literally contains the flag inside it:

```
...a string "flag=ctf{v0l4t1l3_m3m0ry}" in its address space...
```

The flag was sitting right there in plain sight in the challenge text!

### Step 2: Extract the Flag

Pulling the flag straight from the description:

```
ctf{v0l4t1l3_m3m0ry}
```

Got the flag! 🎯

---

## What This Challenge is Teaching — Volatility & Memory Forensics

Even though this specific challenge just required reading the description, it's introducing the concept of **memory forensics** using the **Volatility framework**. Here's what a real memory forensics investigation looks like:

### What is Volatility?

Volatility is an open-source memory forensics framework used to analyze RAM dumps from live systems. It can extract running processes, network connections, open files, passwords, and strings from memory.

### How This Would Work on a Real Memory Dump

If you were given an actual `.mem` or `.raw` memory dump file, here's how you'd find the flag:

```bash
# Step 1: Identify the OS profile of the memory dump
volatility -f memory.raw imageinfo

# Step 2: List all running processes
volatility -f memory.raw --profile=WinXPSP2x86 pslist

# Output would show:
# PID   Name         
# 1337  notepad.exe  ← target process

# Step 3: Dump strings from notepad.exe's memory (PID 1337)
volatility -f memory.raw --profile=WinXPSP2x86 memdump -p 1337 -D ./dump/

# Step 4: Search the dumped memory for the flag
strings ./dump/1337.dmp | grep "flag="
# Output: flag=ctf{v0l4t1l3_m3m0ry}
```

### Key Volatility Commands for CTFs

| Command | Purpose |
|---------|---------|
| `imageinfo` | Identify OS and profile of the memory dump |
| `pslist` | List all running processes |
| `pstree` | Show process tree with parent/child relationships |
| `memdump -p [PID]` | Dump memory of a specific process |
| `strings` | Extract readable strings from a memory dump |
| `filescan` | Find files cached in memory |
| `hashdump` | Extract password hashes from memory |
| `netscan` | Show active network connections |

---

## Alternative Methods (for real memory dumps)

```bash
# Using strings directly on the dump (no Volatility needed for simple cases)
strings memory.raw | grep "ctf{"

# Volatility 3 syntax (newer version)
python3 vol.py -f memory.raw windows.pslist
python3 vol.py -f memory.raw windows.memmap --pid 1337 --dump

# Search for flag pattern across entire dump
strings memory.raw | grep -oP 'ctf\{[^}]+\}'
```

---

## Flag

```
ctf{v0l4t1l3_m3m0ry}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Eyes 👀 | Read the challenge description carefully — the flag was right there! |

---

## Key Takeaways

- **Always read the challenge description carefully** — sometimes the flag or key info is right there
- **Volatility** is the go-to tool for memory forensics in CTFs and real investigations
- Memory dumps can contain passwords, flags, open files, and running processes
- `strings + grep` is the fastest way to search a memory dump for a flag pattern
- The process name `notepad.exe` with PID `1337` is a classic CTF reference — `1337` = "leet"
