# Custom Domain Setup Checklist

Use this checklist to ensure you've completed all steps correctly when setting up your custom domain for GitHub Pages.

## Pre-Setup Checklist

- [ ] I have a GitHub account
- [ ] I have access to this repository (howtokeepyourdayjob)
- [ ] I have purchased a domain name
- [ ] I have access to my domain registrar's control panel
- [ ] I know my GitHub username: _______________

## DNS Configuration Checklist

### For WWW Subdomain (www.yourdomain.com)

- [ ] Logged into domain registrar's DNS management
- [ ] Added CNAME record:
  - [ ] Host/Name: `www`
  - [ ] Value: `<yourusername>.github.io.` (note the trailing dot)
  - [ ] TTL: 3600 or default
- [ ] Saved DNS changes

### For Apex Domain (yourdomain.com)

- [ ] Logged into domain registrar's DNS management
- [ ] Added A record #1: Host `@`, Value `185.199.108.153`
- [ ] Added A record #2: Host `@`, Value `185.199.109.153`
- [ ] Added A record #3: Host `@`, Value `185.199.110.153`
- [ ] Added A record #4: Host `@`, Value `185.199.111.153`
- [ ] (Optional) Added CNAME record: Host `www`, Value `<yourusername>.github.io.`
- [ ] Saved DNS changes

## Repository Configuration Checklist

- [ ] Created `CNAME` file in repository root (no file extension)
- [ ] Added my domain to CNAME file (one line, no https://, no trailing slash)
  - Domain in file: _______________
- [ ] Committed CNAME file: `git add CNAME`
- [ ] Pushed CNAME file: `git commit -m "Add custom domain"` → `git push`
- [ ] Verified CNAME file exists on GitHub in repository root

## GitHub Settings Checklist

- [ ] Navigated to repository Settings on GitHub
- [ ] Clicked on "Pages" in left sidebar
- [ ] Entered custom domain in "Custom domain" field
- [ ] Clicked "Save" button
- [ ] Waited for DNS check (green checkmark appears)
- [ ] DNS check shows: ✅ "DNS check successful"

## HTTPS Configuration Checklist

- [ ] Waited at least 1 hour after DNS configuration
- [ ] Returned to Settings → Pages
- [ ] Enabled "Enforce HTTPS" checkbox
- [ ] Waited up to 24 hours for certificate provisioning
- [ ] Verified HTTPS works by visiting `https://yourdomain.com`

## Verification Checklist

- [ ] Visited `http://yourdomain.com` - it redirects to HTTPS
- [ ] Visited `https://yourdomain.com` - site loads correctly
- [ ] (If using www) Visited `https://www.yourdomain.com` - works correctly
- [ ] Checked browser for 🔒 lock icon indicating secure connection
- [ ] Checked [whatsmydns.net](https://www.whatsmydns.net/) for DNS propagation
- [ ] No browser warnings about insecure content
- [ ] All images and resources load correctly

## Troubleshooting Checklist

If things aren't working, check:

### DNS Issues
- [ ] Waited at least 1 hour since DNS changes
- [ ] Ran `dig yourdomain.com +short` - returns GitHub IPs
- [ ] Ran `dig www.yourdomain.com +short` - returns CNAME
- [ ] Checked DNS propagation at [dnschecker.org](https://dnschecker.org/)
- [ ] Verified no typos in DNS records
- [ ] Removed any conflicting DNS records (old A/CNAME records)

### Repository Issues
- [ ] CNAME file exists in repository root (not in a folder)
- [ ] CNAME file has correct domain name (no typos)
- [ ] CNAME file has no file extension (.txt, .md, etc.)
- [ ] CNAME file contains only the domain (no protocol, no path)
- [ ] Checked file with: `cat CNAME` to verify contents
- [ ] Latest commit includes CNAME file

### GitHub Settings Issues
- [ ] Custom domain is entered in Settings → Pages
- [ ] Domain exactly matches what's in CNAME file
- [ ] No other repository uses this domain
- [ ] GitHub shows green checkmark for DNS
- [ ] Branch is set correctly (usually `main` or `master`)

### HTTPS Issues
- [ ] Waited full 24 hours after DNS configuration
- [ ] Toggled "Enforce HTTPS" off and on again
- [ ] Cleared browser cache (Ctrl+Shift+Del / Cmd+Shift+Del)
- [ ] Tried different browser or incognito mode
- [ ] DNS propagation is complete (verified above)

## Common Error Messages

| Error Message | What It Means | Solution |
|--------------|---------------|----------|
| "DNS check was not successful" | DNS not configured or not propagated | Wait 1 hour, verify DNS records |
| "Domain already taken" | Another repo uses this domain | Remove from other repos |
| "Not Found / 404" | CNAME file missing or wrong | Check CNAME file in repo |
| Certificate error | HTTPS not ready yet | Wait 24 hours, toggle HTTPS |
| "This site can't be reached" | DNS not pointing to GitHub | Verify A/CNAME records |

## Timeline Expectations

| Time | What Should Happen |
|------|-------------------|
| 0 minutes | DNS changes saved at registrar |
| 5 minutes | DNS starts propagating |
| 30-60 minutes | DNS mostly propagated, site might work |
| 1-24 hours | HTTPS certificate provisioning |
| 24 hours | Everything should be fully working |
| 48 hours | 100% guaranteed DNS propagation |

## Quick Command Reference

Check DNS A records:
```bash
dig yourdomain.com +short
# Should return: 185.199.108.153, etc.
```

Check DNS CNAME record:
```bash
dig www.yourdomain.com +short
# Should return: yourusername.github.io.
```

View CNAME file:
```bash
cat CNAME
# Should return: yourdomain.com (or www.yourdomain.com)
```

Test HTTP response:
```bash
curl -I https://yourdomain.com
# Should return: 200 OK or 301 redirect
```

## Support Resources

If you're still stuck after going through this checklist:

- [ ] Re-read [README.md](README.md) for detailed explanations
- [ ] Check [DNS-GUIDE.md](DNS-GUIDE.md) for visual diagrams
- [ ] Review [QUICKSTART.md](QUICKSTART.md) for condensed steps
- [ ] Search [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [ ] Contact your domain registrar's support for DNS help
- [ ] Check [GitHub Community Forum](https://github.community/)
- [ ] Open an issue in this repository

## Notes Section

Use this space to write down your specific details:

**My Domain:** _______________________________

**My GitHub Username:** _______________________________

**DNS Registrar:** _______________________________

**Date DNS Configured:** _______________________________

**Date Added to GitHub:** _______________________________

**Expected HTTPS Ready Date:** _______________________________ (DNS date + 24 hours)

**Status:** 
- [ ] DNS configured
- [ ] CNAME file added
- [ ] GitHub settings updated
- [ ] HTTPS working
- [ ] ✅ COMPLETE!

---

**Last Updated:** 2026-02-07

**Version:** 1.0
