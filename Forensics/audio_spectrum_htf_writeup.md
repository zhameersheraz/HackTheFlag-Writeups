# Audio Spectrum - Hack the Flag Writeup

**Challenge:** Audio Spectrum  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 200 pts  
**Flag:** `ctf{sp3ctro_h1dden}`

---

## Description

Audio files can contain hidden data beyond what you hear. Download the WAV file and examine the raw file data using tools like a hex editor or the `strings` command. The flag is embedded within the file — look beyond the audio samples themselves.

---

## Hints

1. Maybe try using `xxd`?

---

## Solution

### Step 1: Download the WAV File

I downloaded `audio_spectrum.wav` from the challenge page.

### Step 2: Run strings (My Method — Fastest)

The hint says to try `xxd`, but `strings` is even faster — it instantly pulls out all readable text:

```bash
strings audio_spectrum.wav | grep "ctf{"
```

Output:
```
ctf{sp3ctro_h1dden}
```

Got the flag! 🎯

---

## What Was Actually Hidden in the File?

I dug deeper to see exactly where the flag was hiding using Python:

```python
with open("audio_spectrum.wav", "rb") as f:
    data = f.read()

idx = data.find(b'ctf{')
print(data[idx-20:idx+40])
```

Output:
```
b' HIDDEN MESSAGE ===\nctf{sp3ctro_h1dden}\n=== END MESSAGE ===\n'
```

The flag was appended at the **very end of the file** (byte offset 32,089 out of 32,130 total bytes), tucked after all the audio data inside a section labeled:

```
=== HIDDEN MESSAGE ===
ctf{sp3ctro_h1dden}
=== END MESSAGE ===
```

Completely invisible when you play the audio — but readable with any text/hex tool!

---

## Why This Works

### How a WAV File Is Structured

A WAV file has clearly defined sections:

```
WAV File Structure:
┌──────────────────────────┐
│  RIFF Header             │  ← "RIFF....WAVE" - identifies the file
├──────────────────────────┤
│  fmt  chunk              │  ← audio format: sample rate, channels, bit depth
├──────────────────────────┤
│  data chunk              │  ← the actual audio samples (what you HEAR)
├──────────────────────────┤
│  [extra data appended]   │  ← ← ← FLAG WAS HIDDEN HERE! 😱
└──────────────────────────┘
```

Audio players only care about the `fmt` and `data` chunks. Any extra bytes at the end are **silently ignored** when playing the audio — but they're still physically in the file!

The challenge creator appended a hidden text message after all the audio data. You'd never hear it, but forensics tools find it instantly.

---

## Step-by-Step Using xxd (The Hint Method)

```bash
xxd audio_spectrum.wav | tail -20
```

`tail -20` shows the last 20 lines of the hex dump — right where the hidden message is:

```
00007d60: 2020 2020 2020 2020 2020 2020 2020 0a3d  ..............=
00007d70: 3d3d 2048 4944 4445 4e20 4d45 5353 4147  === HIDDEN MESSAG
00007d80: 4520 3d3d 3d0a 6374 667b 7370 3363 7472  E ===.ctf{sp3ctr
00007d90: 6f5f 6831 6464 656e 7d0a 3d3d 3d20 454e  o_h1dden}.=== EN
00007da0: 44 204d 4553 5341 4745 203d 3d3d 0a    D MESSAGE ===.
```

Reading the ASCII on the right: `ctf{sp3ctro_h1dden}` ✅

---

## Alternative Methods

### Method 1: strings (Fastest — what I used)
```bash
strings audio_spectrum.wav | grep "ctf{"
```

### Method 2: xxd + tail (The Hint Method)
```bash
xxd audio_spectrum.wav | tail -20
```

### Method 3: hexed.it (Online Hex Editor)
1. Go to 👉 https://hexed.it/
2. Upload the WAV file
3. Press `Ctrl+End` to jump to the end of the file
4. See the hidden text in the hex view

### Method 4: Python
```python
with open("audio_spectrum.wav", "rb") as f:
    data = f.read()
    
# Find hidden text
idx = data.find(b'ctf{')
if idx != -1:
    print(data[idx:idx+50].decode(errors="ignore"))
```

### Method 5: Audacity (Audio approach — not needed here)
For challenges where the flag is actually **in the audio signal** (e.g., SSTV, Morse code, spectrogram), open in Audacity and look at the spectrogram view. But this challenge hid it as raw text, not in the audio itself.

---

## Real-World Steganography in Audio

There are many ways to hide data in audio files:

| Method | What it is |
|--------|-----------|
| **Appended data** | Extra bytes added after the audio ends (this challenge) |
| **tEXt metadata** | Hidden in file metadata headers |
| **LSB steganography** | Secret data encoded in the least significant bits of audio samples — inaudible to humans |
| **Spectrogram image** | Data encoded so it appears as text/image in the frequency spectrum visualization |
| **Morse/SSTV** | Actual audio signals that encode data |

`strings` and `xxd` catch the simple appended-data method. For LSB or spectrogram hiding, you'd need tools like Audacity, Sonic Visualiser, or Stegoveritas.

---

## Flag

```
ctf{sp3ctro_h1dden}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `strings` + `grep "ctf{"` | Fastest — extracted the flag in one command |
| `xxd` + `tail` | Showed the hex dump at the end of the file where the flag was |
| Python | Confirmed exact byte offset (32,089 of 32,130 bytes) |

---

## Key Takeaways

- Audio files (WAV, MP3, etc.) can contain hidden data **after** the audio ends — players ignore it, but forensics tools find it
- **Always run `strings | grep "ctf{"` first** on any binary file in forensics challenges
- `xxd file | tail -20` is great for checking what's hiding at the end of a file
- The flag was at byte **32,089 out of 32,130** — just the last 41 bytes of a 32KB file!
- Real steganography can hide data in audio LSBs or spectrograms — `strings` won't help there, but Audacity/Sonic Visualiser will
