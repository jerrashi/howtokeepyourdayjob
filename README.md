# How to Keep Your Day Job

A guide to professional development and career success.

## 🌐 Hosting on a Custom Domain

This GitHub Pages site can be hosted on your own custom domain (e.g., `www.yourdomain.com` or `yourdomain.com`). Follow this comprehensive guide to set it up.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Step 1: Purchase a Domain Name](#step-1-purchase-a-domain-name)
3. [Step 2: Configure DNS Settings](#step-2-configure-dns-settings)
4. [Step 3: Add CNAME File to Repository](#step-3-add-cname-file-to-repository)
5. [Step 4: Configure GitHub Repository Settings](#step-4-configure-github-repository-settings)
6. [Step 5: Enable HTTPS](#step-5-enable-https)
7. [Verification and Troubleshooting](#verification-and-troubleshooting)
8. [Advanced Configuration](#advanced-configuration)

📚 **Additional Resources:**
- [Quick Start Guide](QUICKSTART.md) - Get set up in 5 steps
- [DNS Visual Guide](DNS-GUIDE.md) - Diagrams and flowcharts explaining DNS setup

---

## Prerequisites

Before you begin, make sure you have:

- ✅ A GitHub account with this repository
- ✅ A domain name (or ready to purchase one)
- ✅ Access to your domain registrar's DNS settings
- ✅ Basic understanding of DNS concepts (optional but helpful)

---

## Step 1: Purchase a Domain Name

If you don't already own a domain, you'll need to purchase one from a domain registrar.

### Popular Domain Registrars:

- **Namecheap** - User-friendly, affordable
- **Google Domains** - Simple interface, integrated with Google services
- **Cloudflare** - Great for advanced users, includes free CDN
- **GoDaddy** - Well-known, widely used
- **Hover** - Clean interface, no upselling
- **Porkbun** - Budget-friendly option

**Cost:** Usually $10-15 per year for common TLDs like `.com`, `.net`, `.org`

---

## Step 2: Configure DNS Settings

After purchasing your domain, you need to point it to GitHub's servers. You have two options:

### Option A: Using a Custom Subdomain (Recommended)

If you want to use `www.yourdomain.com`:

1. Log in to your domain registrar's dashboard
2. Navigate to DNS settings/management
3. Add a **CNAME record**:
   - **Host/Name:** `www`
   - **Value/Points to:** `yourgithubusername.github.io.`
   - **TTL:** 3600 (or default)

**Example:**
```
Type: CNAME
Host: www
Value: jerrashi.github.io.
TTL: 3600
```

### Option B: Using an Apex Domain (Root Domain)

If you want to use `yourdomain.com` (without www):

1. Log in to your domain registrar's dashboard
2. Navigate to DNS settings/management
3. Add **four A records** pointing to GitHub's IP addresses:
   - **Host/Name:** `@` (represents root domain)
   - **Values:** Add these four IP addresses as separate A records:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - **TTL:** 3600 (or default)

4. **(Optional but recommended)** Add a **CNAME record** for www subdomain:
   - **Host/Name:** `www`
   - **Value:** `yourgithubusername.github.io.`

**Example:**
```
Type: A
Host: @
Value: 185.199.108.153

Type: A
Host: @
Value: 185.199.109.153

Type: A
Host: @
Value: 185.199.110.153

Type: A
Host: @
Value: 185.199.111.153

Type: CNAME
Host: www
Value: jerrashi.github.io.
```

### Option C: Both Apex and Subdomain (Best Practice)

Configure both options above so that both `yourdomain.com` and `www.yourdomain.com` work, with GitHub automatically redirecting one to the other.

---

## Step 3: Add CNAME File to Repository

Create a file named `CNAME` (all uppercase, no file extension) in the root of your repository:

1. In your repository root, create a new file called `CNAME`
2. Add your custom domain on the first line (no protocol, no trailing slash):

**For subdomain:**
```
www.yourdomain.com
```

**For apex domain:**
```
yourdomain.com
```

**Important Notes:**
- Use only ONE domain in the CNAME file (the primary one you want)
- No `https://` or `http://` prefix
- No trailing slash `/`
- The file should contain only the domain, nothing else

3. Commit and push the CNAME file to your repository

```bash
git add CNAME
git commit -m "Add custom domain CNAME"
git push
```

---

## Step 4: Configure GitHub Repository Settings

1. Go to your GitHub repository: `https://github.com/yourusername/howtokeepyourdayjob`
2. Click **Settings** (⚙️) tab
3. Scroll down to the **Pages** section in the left sidebar
4. Under **Custom domain**, enter your domain:
   - Example: `www.yourdomain.com` or `yourdomain.com`
5. Click **Save**
6. GitHub will automatically verify your domain (this may take a few minutes)

You should see a green checkmark with "DNS check successful" once configured correctly.

---

## Step 5: Enable HTTPS

After your custom domain is configured and verified:

1. Wait for DNS propagation (can take 5 minutes to 48 hours, usually within 1 hour)
2. Return to **Settings → Pages** in your GitHub repository
3. Check the box for **Enforce HTTPS**
4. GitHub will automatically provision a free SSL certificate from Let's Encrypt

**Note:** You may need to wait 24 hours after DNS configuration before HTTPS becomes available.

---

## Verification and Troubleshooting

### Check DNS Propagation

Use these tools to verify your DNS changes have propagated:

- [https://www.whatsmydns.net/](https://www.whatsmydns.net/)
- [https://dnschecker.org/](https://dnschecker.org/)

Enter your domain and check if the A records or CNAME records are visible globally.

### Common Issues and Solutions

#### 1. **"DNS check was not successful"**

**Cause:** DNS changes haven't propagated yet, or records are incorrect.

**Solution:**
- Wait 30-60 minutes and try again
- Verify your DNS records match exactly as specified
- Clear your browser cache
- Try accessing from an incognito window

#### 2. **"Domain already taken"**

**Cause:** Another GitHub repository is already using this domain.

**Solution:**
- Make sure you removed the custom domain from any other GitHub repositories
- Check if you have multiple GitHub accounts and remove the domain from all

#### 3. **Certificate Not Found / HTTPS Not Working**

**Cause:** DNS hasn't fully propagated or GitHub hasn't provisioned the certificate yet.

**Solution:**
- Wait 24 hours after DNS configuration
- Uncheck and re-check "Enforce HTTPS" in Settings
- Make sure your domain resolves correctly (use dig or nslookup)

#### 4. **404 Error on Custom Domain**

**Cause:** CNAME file is missing or incorrect.

**Solution:**
- Verify CNAME file exists in repository root
- Check that CNAME file contains correct domain (no typos)
- Make sure CNAME file doesn't have file extension

#### 5. **"www" Not Working**

**Cause:** Missing CNAME record for www subdomain.

**Solution:**
- Add a CNAME record: `www` → `yourusername.github.io.`
- Wait for DNS propagation

### Testing Your Configuration

Use command-line tools to verify:

**Check A records (for apex domain):**
```bash
dig yourdomain.com +short
```
Should return GitHub's IP addresses (185.199.108.153, etc.)

**Check CNAME record (for www subdomain):**
```bash
dig www.yourdomain.com +short
```
Should return `yourusername.github.io.`

**Test HTTP response:**
```bash
curl -I https://www.yourdomain.com
```
Should return 200 OK or 301 redirect

---

## Advanced Configuration

### Using Cloudflare (Optional)

Cloudflare provides additional benefits:
- Free CDN (faster loading worldwide)
- DDoS protection
- Additional security features
- Flexible SSL options

**Setup with Cloudflare:**

1. Sign up for a free Cloudflare account
2. Add your domain to Cloudflare
3. Update your domain's nameservers to Cloudflare's nameservers (at your registrar)
4. In Cloudflare DNS settings, add:
   - **CNAME record:** `www` → `yourusername.github.io` (proxied/orange cloud)
   - **CNAME record:** `@` → `yourusername.github.io` (proxied/orange cloud)
5. In Cloudflare SSL/TLS settings:
   - Set SSL mode to **Full** or **Full (strict)**
6. Continue with GitHub Pages configuration as normal

### Setting Up Email Forwarding

Many domain registrars offer free email forwarding, allowing you to use addresses like `contact@yourdomain.com`:

1. Check if your registrar offers email forwarding
2. Set up forwarding rules in your registrar's control panel
3. Add MX records as required by your provider

**Note:** Email forwarding doesn't conflict with GitHub Pages setup.

### Custom 404 Page

Create a `404.html` file in your repository root for a custom error page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Page Not Found</title>
</head>
<body>
  <h1>404 - Page Not Found</h1>
  <p><a href="/">Return to home</a></p>
</body>
</html>
```

---

## Quick Reference

### DNS Records Cheat Sheet

| Type | Host | Value | Purpose |
|------|------|-------|---------|
| A | @ | 185.199.108.153 | Apex domain support |
| A | @ | 185.199.109.153 | Apex domain support |
| A | @ | 185.199.110.153 | Apex domain support |
| A | @ | 185.199.111.153 | Apex domain support |
| CNAME | www | yourusername.github.io. | WWW subdomain |

### Checklist

- [ ] Domain purchased
- [ ] DNS A records added (for apex domain)
- [ ] DNS CNAME record added (for www subdomain)
- [ ] CNAME file added to repository
- [ ] CNAME file committed and pushed
- [ ] Custom domain configured in GitHub Settings → Pages
- [ ] DNS check successful in GitHub
- [ ] Waited for DNS propagation (up to 48 hours)
- [ ] HTTPS enabled in GitHub Settings → Pages
- [ ] Tested domain in browser

---

## Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Pages Custom Domain Guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [About DNS](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [Let's Encrypt](https://letsencrypt.org/) - Free SSL certificates

---

## Support

If you run into issues:

1. Check the [Troubleshooting](#verification-and-troubleshooting) section above
2. Review [GitHub Pages documentation](https://docs.github.com/en/pages)
3. Contact your domain registrar's support for DNS-specific questions
4. Open an issue in this repository for site-specific questions

---

## License

This project is open source and available under the MIT License.
