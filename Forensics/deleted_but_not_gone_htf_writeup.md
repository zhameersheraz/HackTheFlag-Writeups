# Deleted But Not Gone - Hack the Flag Writeup

**Challenge:** Deleted But Not Gone  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{d3l3ted_n0t_g0ne}`

---

## Description

A file was "deleted" from a disk image, but in forensics, deleted doesn't mean gone. Download the disk image and use forensic recovery techniques to find the flag hidden in the deleted file.

---

## Hints

1. Deleted files can be recovered using tools like strings, foremost, or photorec until the disk sectors are overwritten.

---

## First — Why Can Deleted Files Be Recovered?

When you delete a file, the computer **does NOT actually erase the data**. It just removes the file's name from the directory (like removing a label from a box). The actual data is still sitting on the disk — it's just marked as "free space" that can be overwritten later.

```
Before delete:   [File: secret.txt | Data: ctf{...}]  ← labelled box with data
After delete:    [     (no label)   | Data: ctf{...}]  ← label removed, data still there!
```

This is why forensics tools can recover "deleted" files — the data is still physically on the disk!

---

## Solution

### Step 1: Navigate to the Challenge File

The disk image file (`deleted_disk.img`) should already be in your downloads folder.

```bash
cd /media/sf_downloads
ls
```

You should see `deleted_disk.img` listed.

---

### Step 2: Use strings to Find the Flag (Fastest Method!)

`strings` reads ALL readable text from any file — including disk images. Since the flag is stored as plain text somewhere in the image, this finds it instantly:

```bash
strings deleted_disk.img | grep "ctf{"
```

**Output:**
```
The flag is: ctf{d3l3ted_n0t_g0ne}
```

Got the flag! 🎯

---

## How strings Works

`strings` scans the raw binary of any file and prints out every sequence of readable characters (at least 4 characters long by default). It doesn't care about file systems or deleted status — it reads the raw bytes directly.

```
deleted_disk.img (raw binary):
  ...4f 63 74 66 7b 64 33 6c...  ← raw bytes
  ...o  c  t  f  {  d  3  l...  ← strings reads this as text!

Output: ctf{d3l3ted_n0t_g0ne}
```

`grep "ctf{"` then filters for only lines containing the flag format — so even in a huge disk image, you instantly find it.

---

## Alternative Method: photorec (Interactive)

`photorec` is already installed on Kali. It has an interactive menu:

```bash
photorec deleted_disk.img
```

Follow the on-screen menu:
1. Select the disk image
2. Choose file system type
3. Choose output folder
4. Wait for recovery → find the flag in the recovered files

---

## Step-by-Step Summary

| Step | Command | Result |
|------|---------|--------|
| 1 | `cd /media/sf_downloads` | Navigate to file |
| 2 | `strings deleted_disk.img \| grep "ctf{"` | **Flag found instantly!** |

---

## Visual Summary

```
deleted_disk.img
       |
       ▼
strings deleted_disk.img | grep "ctf{"
       |
       ▼
Scans ALL readable text in the raw binary
       |
       ▼
ctf{d3l3ted_n0t_g0ne} 🎯
```

---

## Real-World Significance

This is exactly what **digital forensics investigators** do in real cases:

- Police recover "deleted" photos/messages from suspects' phones
- Cybersecurity analysts recover malware that attackers tried to delete
- Companies recover accidentally deleted business data

The only way to truly erase data is to **overwrite it** multiple times (using tools like `shred` or `wipe`). Just pressing Delete or emptying the Recycle Bin is NOT enough!

---

## Flag

```
ctf{d3l3ted_n0t_g0ne}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` | Main method — scan raw image for readable text |
| `grep "ctf{"` | Filter strings output to find only the flag |
| `photorec` | Alternative — interactive file carver |

---

## Key Takeaways

- **`strings file | grep "ctf{"`** should ALWAYS be your first attempt in forensics challenges — it's the fastest!
- **Deleting a file does NOT erase the data** — it just removes the directory entry
- The data stays on disk until it gets overwritten by new files
- In real forensics, even "emptied" drives can yield evidence — overwriting is the only true deletion!
