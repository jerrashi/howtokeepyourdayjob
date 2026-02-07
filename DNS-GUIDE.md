# DNS Configuration Visual Guide

This diagram shows how DNS records connect your custom domain to GitHub Pages.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     YOUR CUSTOM DOMAIN                           │
│                    (www.yourdomain.com)                          │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │ DNS Lookup
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                  DOMAIN REGISTRAR DNS                            │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  CNAME Record                                             │  │
│  │  Host: www                                                │  │
│  │  Points to: yourusername.github.io                       │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  A Records (for apex domain)                             │  │
│  │  Host: @                                                  │  │
│  │  IP: 185.199.108.153                                     │  │
│  │  IP: 185.199.109.153                                     │  │
│  │  IP: 185.199.110.153                                     │  │
│  │  IP: 185.199.111.153                                     │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │ Resolves to
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    GITHUB PAGES SERVERS                          │
│                  (185.199.108-111.153)                          │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                                                           │  │
│  │        Your Repository: howtokeepyourdayjob              │  │
│  │                                                           │  │
│  │        ┌─────────────────────────────────┐               │  │
│  │        │  CNAME file                     │               │  │
│  │        │  Contents: www.yourdomain.com   │               │  │
│  │        └─────────────────────────────────┘               │  │
│  │                                                           │  │
│  │        ┌─────────────────────────────────┐               │  │
│  │        │  index.html (your website)      │               │  │
│  │        └─────────────────────────────────┘               │  │
│  │                                                           │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ✅ HTTPS via Let's Encrypt (free SSL certificate)             │
└─────────────────────────┬────────────────────────────────────────┘
                          │
                          │ Serves webpage
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                         VISITOR                                  │
│                  (Browser shows your site)                       │
│                  🔒 https://www.yourdomain.com                  │
└─────────────────────────────────────────────────────────────────┘
```

## Step-by-Step Flow

### 1️⃣ Visitor Types URL
```
User types: www.yourdomain.com
```

### 2️⃣ DNS Resolution
```
Browser → "What's the IP for www.yourdomain.com?"
DNS → "It's a CNAME pointing to yourusername.github.io"
DNS → "Which resolves to 185.199.108.153 (and mirrors)"
```

### 3️⃣ Connection to GitHub
```
Browser → Connects to 185.199.108.153
GitHub → "Let me check which site you want..."
GitHub → Reads CNAME file in your repo
GitHub → "Ah, you want the howtokeepyourdayjob site!"
```

### 4️⃣ SSL/HTTPS
```
GitHub → Provides Let's Encrypt SSL certificate
Browser → Shows 🔒 secure connection
```

### 5️⃣ Content Delivery
```
GitHub → Serves your index.html
Browser → Displays your website
```

## DNS Record Types Explained

### CNAME Record (Canonical Name)
```
Purpose: Creates an alias from one domain to another
Example: www.yourdomain.com → yourusername.github.io
Use case: Subdomain setup (www, blog, docs, etc.)

┌──────────────────┐        ┌─────────────────────┐
│ www.yourdomain   │  ───▶  │ username.github.io  │
│     .com         │        │                     │
└──────────────────┘        └─────────────────────┘
```

### A Record (Address Record)
```
Purpose: Points domain directly to an IP address
Example: yourdomain.com → 185.199.108.153
Use case: Apex/root domain setup

┌──────────────────┐        ┌─────────────────────┐
│  yourdomain.com  │  ───▶  │  185.199.108.153   │
│   (root domain)  │        │   (GitHub server)   │
└──────────────────┘        └─────────────────────┘
```

## Common Configurations

### Configuration 1: WWW Only
```
DNS Setup:
└─ CNAME: www → yourusername.github.io

CNAME File:
└─ www.yourdomain.com

Result:
✅ www.yourdomain.com works
❌ yourdomain.com does NOT work
```

### Configuration 2: Apex Only
```
DNS Setup:
└─ 4x A Records: @ → 185.199.108-111.153

CNAME File:
└─ yourdomain.com

Result:
✅ yourdomain.com works
❌ www.yourdomain.com does NOT work
```

### Configuration 3: Both (Recommended) ⭐
```
DNS Setup:
├─ 4x A Records: @ → 185.199.108-111.153
└─ CNAME: www → yourusername.github.io

CNAME File:
└─ yourdomain.com (or www.yourdomain.com)

Result:
✅ yourdomain.com works
✅ www.yourdomain.com works
✅ GitHub auto-redirects between them
```

## Propagation Timeline

```
Time: 0 min         DNS changes saved at registrar
      ↓
      5 min         Changes start propagating
      ↓
      30 min        ~50% of DNS servers updated
      ↓
      1 hour        ~80% of DNS servers updated
      ↓
      24 hours      ~99% of DNS servers updated
      ↓
      48 hours      100% guaranteed propagation
```

## Troubleshooting Flowchart

```
Start: Custom domain not working
         ↓
    ┌────┴────┐
    │  Step 1 │
    │ Wait    │──── Has it been < 1 hour? ──→ Wait longer
    │ 1 hour  │                                (DNS propagation)
    └────┬────┘
         ↓
    ┌────┴────┐
    │  Step 2 │
    │ Check   │──── A/CNAME records wrong? ──→ Fix DNS settings
    │ DNS     │                                at registrar
    └────┬────┘
         ↓
    ┌────┴────┐
    │  Step 3 │
    │ Check   │──── CNAME file missing? ────→ Add CNAME file
    │ Repo    │                                to repository
    └────┬────┘
         ↓
    ┌────┴────┐
    │  Step 4 │
    │ GitHub  │──── Not configured? ────────→ Add domain in
    │ Settings│                                Settings → Pages
    └────┬────┘
         ↓
    ┌────┴────┐
    │  Step 5 │
    │ HTTPS   │──── Not working? ────────────→ Wait 24 hours,
    │ Check   │                                toggle Enforce HTTPS
    └────┬────┘
         ↓
    ✅ Working!
```

## Security Notes

### What HTTPS Provides
```
┌─────────────────────────────────────────┐
│         🔒 HTTPS Benefits               │
├─────────────────────────────────────────┤
│ ✅ Encrypted connection                 │
│ ✅ Verified identity (your domain)      │
│ ✅ Data integrity (no tampering)        │
│ ✅ SEO boost (Google ranking factor)    │
│ ✅ Browser trust (no warnings)          │
│ ✅ Required for modern web APIs         │
└─────────────────────────────────────────┘
```

### Certificate Chain
```
Let's Encrypt (Root CA)
    ↓
Let's Encrypt Authority X3 (Intermediate)
    ↓
yourdomain.com (Your Certificate) 🔒
    ↓
✅ Trusted by all browsers
```

## References

- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [Understanding DNS](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [How HTTPS Works](https://howhttps.works/)
