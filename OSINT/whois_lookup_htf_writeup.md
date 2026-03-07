# Whois Lookup - Hack the Flag Writeup

**Challenge:** Whois Lookup  
**Category:** OSINT  
**Difficulty:** Easy  
**Points:** 100 pts  
**Flag:** `ctf{iana}`

---

## Description

Every domain has registration information accessible via WHOIS. Look up `example.com` and find who the registrant organization is. The answer is a well-known internet standards body. Submit as `ctf{org_name_lowercase_no_spaces}`.

---

## Hints

1. Use a WHOIS lookup tool and check the registrant organization field. IANA manages example.com.

---

## Solution

### Step 1: Understand What WHOIS Is

**WHOIS** is a public database that stores registration information for every domain on the internet. When someone registers a domain, their organization name, contact info, and registration dates are recorded and made publicly available.

### Step 2: Look Up example.com (My Method)

I used the `whois` command on Kali Linux:

```bash
whois example.com
```

Key output:

```
Domain Name: EXAMPLE.COM
Registrant Organization: Internet Assigned Numbers Authority
Registrar: RESERVED-Internet Assigned Numbers Authority
Creation Date: 1995-08-14
```

The **Registrant Organization** is: **Internet Assigned Numbers Authority**

### Step 3: Format the Flag

The challenge says to submit as `ctf{org_name_lowercase_no_spaces}`:

```
Internet Assigned Numbers Authority → IANA (the abbreviation)
→ ctf{iana}
```

Got the flag! 🎯

---

## Why This Works

### What is IANA?

**IANA** (Internet Assigned Numbers Authority) is the organization responsible for coordinating the internet's core infrastructure — including domain names, IP addresses, and protocol parameters. They manage special-use domains like `example.com`, `test.com`, and `localhost` to prevent them from being registered by anyone else.

### What Does WHOIS Show?

| WHOIS Field | What it means |
|-------------|--------------|
| `Domain Name` | The registered domain |
| `Registrant Organization` | Who owns the domain |
| `Registrar` | The company that registered it |
| `Creation Date` | When the domain was first registered |
| `Expiry Date` | When the registration expires |
| `Name Servers` | DNS servers handling the domain |

---

## Alternative Methods

### Method 1: whois.domaintools.com (Online, Easiest)
1. Go to 👉 https://whois.domaintools.com/
2. Type `example.com` in the search bar
3. Hit Enter
4. Look for the **Registrant Organization** field
5. It shows: **Internet Assigned Numbers Authority**

### Method 2: ICANN Lookup
1. Go to 👉 https://lookup.icann.org
2. Type `example.com`
3. Click **Lookup**
4. Find the **Registrant** section → **Internet Assigned Numbers Authority**

### Method 3: who.is
1. Go to 👉 https://who.is
2. Search for `example.com`
3. Registrant Organization is shown at the top

### Method 4: MXToolbox WHOIS
1. Go to 👉 https://mxtoolbox.com/whois.aspx
2. Enter `example.com`
3. Click **WHOIS Lookup**
4. Scroll to **Registrant Organization**

### Method 5: nslookup (Kali / Windows / Mac)
```bash
nslookup example.com
```
Output:
```
Server:   8.8.8.8
Name:     example.com
Address:  93.184.216.34
```
> Note: `nslookup` gives DNS info but not WHOIS registration details — use `whois` for the full registrant info.

---

## Flag

```
ctf{iana}
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `whois` (Kali Linux) | Look up domain registration details in terminal |
| whois.domaintools.com | Online WHOIS lookup |
| ICANN Lookup | Official domain registration database |
| who.is | Clean online WHOIS viewer |

---

## Key Takeaways

- **WHOIS** is a public database storing registration info for every domain
- Always check the **Registrant Organization** field in WHOIS lookups
- `example.com` is owned by **IANA** — reserved for documentation and testing purposes
- `whois <domain>` is the fastest terminal command for domain lookups on Linux
- WHOIS is a critical tool in OSINT investigations — it can reveal who owns a suspicious domain
