# Document Metadata - Hack the Flag Writeup

**Challenge:** Document Metadata  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 150 pts  
**Flag:** `ctf{d0cum3nt_m3t4d4t4_l34k}`

---

## Description

Your team intercepted leaked corporate documents. Analyze the metadata carefully — document authors sometimes hide notes in custom properties. Expand all sections and examine every field!

---

## Hints

1. Document metadata includes custom properties. Look for unusual fields like "Secret_Note" in the expandable sections.

---

## Solution

### Step 1: Open the Leaked Corporate Documents

I clicked the **leaked corporate documents** link from the challenge page. It opened a **Document Intelligence Analysis** page showing metadata extracted from two intercepted files:

- **Leaked Document #1** — `company_memo_draft.docx` (a Word document)
- **Leaked Image #2** — `office_photo.jpg` (a photo with EXIF data)

### Step 2: Examine the Standard Properties

The first document had these standard properties:

```
Title         : Q1 Strategic Priorities - CONFIDENTIAL
Author        : John Smith
Company       : AcmeCorp Internal
Last Saved By : j.smith
Created       : 2024-02-14 09:15:00
Modified      : 2024-02-14 14:30:00
```

Nothing suspicious here — but I noticed a collapsed section at the bottom:

```
▶ Click to expand custom document properties...
```

### Step 3: Expand the Custom Properties (My Method)

I clicked **"Click to expand custom document properties..."** and it revealed hidden fields that don't show up in the normal document view:

```
Department      : Executive
Classification  : CONFIDENTIAL
Project Code    : PHOENIX
Internal ID     : DOC-2024-0847
Approval Status : PENDING
Secret Note     : ctf{d0cum3nt_m3t4d4t4_l34k}   ← FLAG IS HERE!
```

The flag was sitting inside a custom property called **Secret Note**! 🎯

Got the flag! 🎯

---

## Why This Works

### What are Document Custom Properties?

Every Office document (Word, Excel, PowerPoint) has two types of metadata:

| Type | Examples | Visible to users? |
|------|----------|-------------------|
| **Standard Properties** | Title, Author, Created date | Sometimes |
| **Custom Properties** | Any field the author adds | Almost never |

Custom properties are completely invisible when you just open the document normally. You have to specifically go looking for them — which is exactly what real OSINT investigators do when analyzing leaked documents!

### What Real Metadata Leaks Look Like

The image in this challenge also showed realistic EXIF data leaks from `office_photo.jpg`:

```
Camera       : iPhone 14 Pro
Date Taken   : 2024-02-20 12:45:33
GPS Latitude : 37.7749
GPS Longitude: -122.4194
Location     : San Francisco, CA
Comment      : Team lunch photo - DO NOT SHARE EXTERNALLY
```

This is a real-world risk — photos taken on phones automatically embed GPS coordinates and device info, which can expose sensitive location data when shared externally.

---

## Alternative Methods

### Method 1: ExifTool on Kali Linux (for real .docx files)
```bash
exiftool company_memo_draft.docx
```
This extracts all metadata including custom properties from Word documents.

### Method 2: Check Properties in Windows
1. Right-click the `.docx` file
2. Click **Properties → Details**
3. Scroll down — some custom properties appear here

### Method 3: Open in LibreOffice / Microsoft Word
1. Open the document
2. Go to **File → Properties → Custom Properties**
3. All custom fields are listed here

### Method 4: Unzip the .docx (Advanced)
Word documents are actually ZIP files. You can unzip them and read the raw XML:
```bash
unzip company_memo_draft.docx -d output_folder
cat output_folder/docProps/custom.xml
```
Output will contain all custom properties in XML format, including the `Secret_Note` field.

### Method 5: Online Metadata Viewer
1. Go to 👉 https://www.metadata2go.com
2. Upload the `.docx` file
3. All metadata including custom properties are displayed

---

## Flag

```
ctf{d0cum3nt_m3t4d4t4_l34k}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Browser (expand section) | Clicked to reveal hidden custom properties |
| ExifTool | Extract all metadata from documents on Kali |
| metadata2go.com | Online document metadata viewer |
| unzip + cat | Read raw XML inside .docx files |

---

## Key Takeaways

- Document **custom properties** are invisible during normal use but store hidden data
- Always click **expand all** on any metadata viewer — the flag is often in a collapsed section
- `.docx` files are ZIP archives — you can unzip them to read raw XML metadata
- Real-world document leaks often expose author names, company names, GPS data, and internal notes through metadata
- `exiftool` works on documents, not just images — always run it on any file in OSINT challenges
