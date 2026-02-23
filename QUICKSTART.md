# Quick Start: Custom Domain Setup

A condensed guide to get your GitHub Page running on a custom domain in under 30 minutes.

## What You Need

1. A domain name (purchased from Namecheap, Google Domains, Cloudflare, etc.)
2. Access to your domain's DNS settings
3. This GitHub repository

## 5-Step Setup

### Step 1: Configure DNS (5 minutes)

Log into your domain registrar and add these DNS records:

**For www subdomain (e.g., www.yourdomain.com):**
```
Type: CNAME
Host: www
Value: yourgithubusername.github.io.
```

**For apex domain (e.g., yourdomain.com):**
```
Add these four A records:
Type: A, Host: @, Value: 185.199.108.153
Type: A, Host: @, Value: 185.199.109.153
Type: A, Host: @, Value: 185.199.110.153
Type: A, Host: @, Value: 185.199.111.153
```

### Step 2: Create CNAME File (1 minute)

In your repository root, create a file named `CNAME` (no extension) with your domain:

```
www.yourdomain.com
```

Or for apex domain:
```
yourdomain.com
```

### Step 3: Commit and Push (1 minute)

```bash
git add CNAME
git commit -m "Add custom domain"
git push
```

### Step 4: Configure GitHub Settings (2 minutes)

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under "Custom domain", enter: `www.yourdomain.com`
4. Click **Save**
5. Wait for the green checkmark: "DNS check successful"

### Step 5: Enable HTTPS (1 minute + wait time)

1. Still in **Settings** → **Pages**
2. Check the box: **Enforce HTTPS**
3. Wait for certificate provisioning (can take up to 24 hours)

## Wait Times

- DNS propagation: 5 minutes to 48 hours (usually under 1 hour)
- HTTPS certificate: Up to 24 hours
- Your site should be live at your custom domain shortly!

## Verify It Works

Visit your domain in a browser:
- `https://www.yourdomain.com` or `https://yourdomain.com`

Use online tools to check DNS:
- [whatsmydns.net](https://www.whatsmydns.net/)
- [dnschecker.org](https://dnschecker.org/)

## Troubleshooting

**Issue:** "DNS check was not successful"  
**Fix:** Wait 30-60 minutes for DNS propagation, verify your DNS records are correct

**Issue:** 404 Error  
**Fix:** Check that CNAME file exists and has the correct domain (no typos!)

**Issue:** HTTPS not working  
**Fix:** Wait up to 24 hours after DNS configuration, then toggle "Enforce HTTPS" off and on

---

For detailed explanations, troubleshooting, and advanced configuration, see the full [README.md](README.md).
