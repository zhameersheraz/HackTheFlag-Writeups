# Packet Layers - Hack the Flag Writeup

**Challenge:** Packet Layers  
**Category:** Miscellaneous  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{layer_4}`

---

## Description

In the OSI model, which layer (by number) handles end-to-end communication and uses TCP/UDP? Submit as `ctf{layer_}`.

---

## Hints

1. The OSI model has 7 layers. TCP and UDP operate at the Transport layer.

---

## Solution

### Step 1: Know the OSI Model

The **OSI model** is a framework that describes how data travels across a network. It has **7 layers**, each with a specific job:

| Layer # | Layer Name   | What it does | Examples |
|---------|-------------|--------------|---------|
| 7 | Application  | What the user sees | HTTP, FTP, DNS |
| 6 | Presentation | Formats/encrypts data | SSL, JPEG, UTF-8 |
| 5 | Session      | Manages connections | NetBIOS, RPC |
| **4** | **Transport**    | **End-to-end communication** | **TCP, UDP** ← |
| 3 | Network      | Routing between networks | IP, ICMP |
| 2 | Data Link    | Communication within a network | Ethernet, MAC |
| 1 | Physical     | Physical cables/signals | Cables, Wi-Fi signals |

### Step 2: Identify the Layer

The challenge asks: *which layer handles **end-to-end communication** and uses **TCP/UDP**?*

The hint confirms: **TCP and UDP operate at the Transport layer.**

The Transport layer is **Layer 4**.

### Step 3: Build the Flag

The flag format is `ctf{layer_}`:

```
ctf{layer_4}
```

Got the flag! 🎯

---

## Why Layer 4?

Think of the OSI model like **sending a package** 📦:

| Layer | Real-world analogy |
|-------|--------------------|
| 7 - Application | You write a letter |
| 6 - Presentation | You translate it to another language |
| 5 - Session | You open a phone call to coordinate |
| **4 - Transport** | **You choose delivery method: fast (UDP) or guaranteed (TCP)** |
| 3 - Network | Post office finds the best route to the destination |
| 2 - Data Link | The delivery truck drives to the right street |
| 1 - Physical | The actual road the truck drives on |

**Transport Layer (Layer 4)** is specifically responsible for:
- Getting data from one device to another reliably (**end-to-end**)
- **TCP** = slow but guaranteed delivery (like registered mail with tracking)
- **UDP** = fast but no guarantee (like dropping a postcard in a mailbox)

---

## Memory Trick

To remember all 7 layers in order, use this phrase:

**"All People Seem To Need Data Processing"**

```
A - Application  (7)
P - Presentation (6)
S - Session      (5)
T - Transport    (4) ← TCP/UDP lives here!
N - Network      (3)
D - Data Link    (2)
P - Physical     (1)
```

---

## Flag

```
ctf{layer_4}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| OSI Model knowledge | Know that TCP/UDP = Layer 4 (Transport) |
| Wikipedia OSI Model | https://en.wikipedia.org/wiki/OSI_model |

---

## Key Takeaways

- The OSI model has **7 layers** — memorize them for networking CTFs
- **TCP and UDP** always operate at **Layer 4 (Transport)**
- TCP = reliable, ordered delivery | UDP = fast, no guarantee
- The Transport layer handles **end-to-end** communication (device to device)
- Memory trick: **"All People Seem To Need Data Processing"**
