# Subnet Math - HackTheFlag Writeup

**Challenge:** Subnet Math  
**Category:** Miscellaneous  
**Difficulty:** Medium  
**Points:** 150  
**Flag:** `ctf{hosts_254}`

---

## Description

A network has the CIDR notation `192.168.1.0/24`. How many usable host addresses are available? Submit as `ctf{hosts_}`.

> **Hint:** A /24 network has 2^8 = 256 addresses. Subtract 2 (network address and broadcast). 256 - 2 = 254 usable hosts.

---

## Tool Used

- Brain 🧠 (math challenge — no tools needed)
- [https://www.ipaddressguide.com/cidr](https://www.ipaddressguide.com/cidr) — subnet calculator (optional)

---

## Solution

### Step 1: Understand What /24 Means

In CIDR notation, the number after the `/` tells you how many bits are used for the **network part**.

An IP address has **32 bits** total:

```
192  . 168  .  1   .  0  /24
└────────────────────┘ └──┘
    Network (24 bits)   Host (8 bits)
```

So `/24` means:
- **24 bits** = network part (fixed)
- **32 - 24 = 8 bits** = host part (changeable)

---

### Step 2: Calculate Total Addresses

The host part has **8 bits**, so total addresses = 2^8:

```
2^8 = 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 = 256
```

---

### Step 3: Subtract 2 Reserved Addresses

Out of 256 addresses, **2 are reserved** and cannot be used for hosts:

| Address | Example | Purpose |
|---------|---------|---------|
| First address | `192.168.1.0` | Network address (identifies the network) |
| Last address | `192.168.1.255` | Broadcast address (sends to all devices) |

So usable hosts = 256 - 2 = **254**

---

### Step 4: Submit the Flag

```
ctf{hosts_254}
```

✅ **Correct! +150 points** 🎯

---

## Quick Visual Summary

```
192.168.1.0/24
      ↓
Host bits = 32 - 24 = 8
      ↓
Total addresses = 2^8 = 256
      ↓
Subtract 2 (network + broadcast)
      ↓
256 - 2 = 254 usable hosts
```

---

## Key Takeaways

- **/24** = 8 host bits = 256 total addresses = **254 usable hosts**
- Always subtract **2** from total — one for network address, one for broadcast
- The formula is always: **2^(32 - prefix) - 2**
- Common subnets to memorize:

| CIDR | Total | Usable Hosts |
|------|-------|-------------|
| /24 | 256 | **254** |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /30 | 4 | 2 |
