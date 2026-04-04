# Cron Schedule — Hack the Flag Writeup

**Challenge:** Cron Schedule  
**Category:** Miscellaneous  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{monday_03:00}`  

---

## Description

A cron job runs at `0 3 * * 1`. When does it execute? The flag is `ctf{day_time}` where day is the day of the week and time is in HH:MM format.

---

## Hints

> Cron format: `minute hour day-of-month month day-of-week`. `0=minute`, `3=hour`, `*=any day`, `*=any month`, `1=Monday`.

---

## Solution

### Step 1: Understand cron syntax

A cron expression has **5 fields**:

```
┌───── minute       (0–59)
│ ┌─── hour         (0–23)
│ │ ┌─ day of month (1–31)
│ │ │ ┌ month       (1–12)
│ │ │ │ ┌ day of week (0–7, where 0 and 7 = Sunday, 1 = Monday)
│ │ │ │ │
0 3 * * 1
```

### Step 2: Parse `0 3 * * 1`

| Field | Value | Meaning |
|-------|-------|---------|
| minute | `0` | At minute :00 |
| hour | `3` | At 3 AM (03:00) |
| day of month | `*` | Every day of the month |
| month | `*` | Every month |
| day of week | `1` | **Monday** |

**Translation:** *"Run at 03:00 every Monday"*

### Step 3: Verify with crontab.guru

Go to 👉 https://crontab.guru/

Enter `0 3 * * 1`

Output: *"At 03:00 on Monday"* ✅

### Step 4: Build the flag

```
day  = monday
time = 03:00
flag = ctf{monday_03:00}
```

---

## Common Cron Examples (for reference)

| Expression | Meaning |
|-----------|---------|
| `0 0 * * *` | Every day at midnight |
| `*/5 * * * *` | Every 5 minutes |
| `0 9 * * 1-5` | 9 AM on weekdays (Mon–Fri) |
| `0 3 * * 1` | 3 AM every Monday ← this challenge |
| `0 0 1 * *` | Midnight on the 1st of every month |

---

## Flag

```
ctf{monday_03:00}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| crontab.guru | Visual cron expression explainer |
| `man 5 crontab` | Linux manual for cron format |

---

## Key Takeaways

- Cron format: `minute hour day-of-month month day-of-week`
- Day-of-week `1` = Monday (0 and 7 are both Sunday in Linux cron)
- `*` means "every" — it matches all valid values for that field
- crontab.guru is a must-bookmark tool for reading/writing cron expressions
