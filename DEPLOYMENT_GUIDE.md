# NK Website Deployment & Configuration Guide

## Overview
This document provides a complete step-by-step guide for deploying the NK Website (Nkaizen Consulting Services) to GitHub Pages with a custom GoDaddy domain and integrating a career application form with email notifications.

---

## Table of Contents
1. [GitHub Pages Deployment](#1-github-pages-deployment)
2. [Custom Domain Configuration (GoDaddy)](#2-custom-domain-configuration-godaddy)
3. [DNS Configuration](#3-dns-configuration)
4. [Career Form Integration](#4-career-form-integration)
5. [Testing & Verification](#5-testing--verification)
6. [Troubleshooting](#6-troubleshooting)

---

## 1. GitHub Pages Deployment

### 1.1 Repository Setup
**Objective**: Host the static website on GitHub Pages

**Steps**:
1. Create a GitHub repository (e.g., `NK-Website-29-10-21`)
2. Upload all website files to the repository root:
   - `index.html`
   - `career.html`
   - `aboutus.html`
   - `contactus.html`
   - All other HTML files
   - `assets/` folder (CSS, JS, images)

### 1.2 Enable GitHub Pages
1. Go to repository **Settings**
2. Navigate to **Pages** section (left sidebar)
3. Under **Build and deployment**:
   - Source: Select **"Deploy from a branch"**
   - Branch: Select **"main"** (or "master")
   - Folder: Select **"/ (root)"**
4. Click **Save**
5. Wait 1-2 minutes for deployment

**Result**: Your site will be live at `https://yourusername.github.io/NK-Website-29-10-21/`

---

## 2. Custom Domain Configuration (GoDaddy)

### 2.1 Add Custom Domain in GitHub
**Objective**: Connect your GoDaddy domain to GitHub Pages

**Steps**:
1. In GitHub **Settings → Pages**
2. Find **Custom domain** section
3. Enter your domain: `www.nkps.ai`
4. Click **Save**
5. GitHub will create a `CNAME` file in your repository

**Note**: Do NOT enable "Enforce HTTPS" yet - wait for DNS propagation first.

---

## 3. DNS Configuration

### 3.1 Configure GoDaddy DNS Records
**Objective**: Point your domain to GitHub Pages servers

**Access DNS Management**:
1. Login to GoDaddy account
2. Go to: https://dnsmanagement.godaddy.com/
3. Select your domain: `nkps.ai`

### 3.2 Add CNAME Record (for www subdomain)
**Purpose**: Makes `www.nkps.ai` work

| Field | Value |
|-------|-------|
| Type | CNAME |
| Name | www |
| Value | sachitti.github.io |
| TTL | 1 Hour |

**Steps**:
1. Click **Add New Record**
2. Select **CNAME** from Type dropdown
3. Enter `www` in Name field
4. Enter `sachitti.github.io` in Value field (without trailing dot)
5. Set TTL to **1 Hour**
6. Click **Save**

### 3.3 Add A Records (for root domain)
**Purpose**: Makes `nkps.ai` (without www) work

Add these 4 A records:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | @ | 185.199.108.153 | 1 Hour |
| A | @ | 185.199.109.153 | 1 Hour |
| A | @ | 185.199.110.153 | 1 Hour |
| A | @ | 185.199.111.153 | 1 Hour |

**Steps** (repeat 4 times):
1. Click **Add New Record**
2. Select **A** from Type dropdown
3. Enter `@` in Name field
4. Enter one of the IP addresses in Value field
5. Set TTL to **1 Hour**
6. Click **Save**

### 3.4 Remove Conflicting Records
**Important**: Delete any existing records that conflict:
- Delete any A record with "WebsiteBuilder Site" value
- Delete any old CNAME records pointing to other services
- Keep NS (nameserver) and SOA records

### 3.5 DNS Propagation
**Wait Time**: 10 minutes to 48 hours (usually 10-30 minutes)

**Verify DNS**:
```cmd
nslookup www.nkps.ai
nslookup nkps.ai
```

**Expected Output**:
- Should show GitHub's IP addresses (185.199.108.153, etc.)
- No other IPs should appear

**Clear DNS Cache** (on your computer):
```cmd
ipconfig /flushdns
```

### 3.6 Enable HTTPS
**After DNS propagates** (10-30 minutes):
1. Go to GitHub **Settings → Pages**
2. Check the box: **"Enforce HTTPS"**
3. Wait 5-10 minutes for SSL certificate provisioning

---

## 4. Career Form Integration

### 4.1 Form Service Selection
**Service Used**: FormSubmit.co
- **Why**: Free, unlimited submissions, supports file uploads, no account needed
- **Alternative considered**: Formspree (doesn't support file uploads on free plan)

### 4.2 Form Configuration
**File**: `career.html`

**Form Action**:
```html
<form action="https://formsubmit.co/satheeshreddy92@gmail.com" method="POST" enctype="multipart/form-data">
```

### 4.3 Form Fields Configured

| Field Name | Input Type | Required | Purpose |
|------------|-----------|----------|---------|
| Name | text | Yes | Applicant's full name |
| Email | email | Yes | Contact email |
| Phone | tel | No | Phone number |
| Location | text | No | Current location |
| Technology | select | Yes | Technology expertise |
| Resume | file | Yes | Resume upload (.pdf, .doc, .docx) |
| Message | textarea | No | Additional message |

### 4.4 Hidden Configuration Fields

```html
<!-- Email subject line -->
<input type="hidden" name="_subject" value="New Career Application - NK Website">

<!-- Disable CAPTCHA -->
<input type="hidden" name="_captcha" value="false">

<!-- Email format (table layout) -->
<input type="hidden" name="_template" value="table">

<!-- Custom redirect to stay on career page -->
<input type="hidden" name="_next" value="https://www.nkps.ai/career.html?success=true">

<!-- Honeypot spam protection -->
<input type="text" name="_honey" style="display:none">
```

**Important**: The `_next` parameter ensures users stay on your website after submission instead of being redirected to FormSubmit's thank you page.

### 4.5 Technology Options
The dropdown includes:
- SAP
- CRM
- Sales Force
- MS Azure admin
- Microsoft Azure Devops
- SQL Azure
- Full-stack Development
- Mean-stack
- Mern-stack
- Python
- Python Django

### 4.6 Success Message Implementation
**Objective**: Show success message on the same page without exposing email in URL

**Success Alert HTML**:
```html
<div id="successAlert" class="alert alert-success" style="display:none; margin-bottom: 15px;">
  <strong>Success!</strong> Your application has been submitted successfully. We'll get back to you soon!
</div>
```

**JavaScript for Success Handling**:
```javascript
<script>
  // Show success message if redirected back with success parameter
  if (window.location.search.includes('success=true')) {
    document.getElementById('successAlert').style.display = 'block';
    // Scroll to the success message
    document.getElementById('successAlert').scrollIntoView({ behavior: 'smooth', block: 'center' });
    // Remove success parameter from URL without page reload
    window.history.replaceState({}, document.title, window.location.pathname);
  }
</script>
```

**How It Works**:
1. Form submits to FormSubmit.co
2. FormSubmit redirects back to `career.html?success=true`
3. JavaScript detects the `success=true` parameter
4. Success message is displayed
5. URL is cleaned (parameter removed)
6. User stays on the career page

### 4.7 First-Time Setup (One-Time Only)
**Important**: FormSubmit requires email verification on first use

**Steps**:
1. Submit a test application through the form
2. Check inbox: `satheeshreddy92@gmail.com`
3. Look for email from FormSubmit
4. Click the **confirmation link** in the email
5. After confirmation, all future submissions will work automatically

---

## 5. Testing & Verification

### 5.1 Domain Testing
**Test URLs**:
- `http://nkps.ai` → Should redirect to `https://www.nkps.ai`
- `http://www.nkps.ai` → Should redirect to `https://www.nkps.ai`
- `https://nkps.ai` → Should work
- `https://www.nkps.ai` → Should work

### 5.2 Form Testing
**Test the Career Form**:
1. Go to: `https://www.nkps.ai/career.html`
2. Fill in all required fields:
   - Name
   - Email
   - Technology (select one)
   - Upload a test resume (PDF or DOC)
3. Click **Submit**
4. Check email: `satheeshreddy92@gmail.com`
5. Verify you received:
   - All form data in table format
   - Resume file as attachment

### 5.3 DNS Verification Commands
```cmd
# Check www subdomain
nslookup www.nkps.ai

# Check root domain
nslookup nkps.ai

# Check nameservers
nslookup -type=NS nkps.ai

# Check directly from GoDaddy nameserver
nslookup nkps.ai ns17.domaincontrol.com
```

---

## 6. Troubleshooting

### 6.1 DNS Issues

**Problem**: Domain not resolving
**Solution**:
- Wait longer (up to 24 hours)
- Clear DNS cache: `ipconfig /flushdns`
- Check DNS propagation: https://dnschecker.org

**Problem**: Wrong IPs appearing
**Solution**:
- Check for conflicting A records in GoDaddy
- Delete "WebsiteBuilder Site" records
- Verify only GitHub IPs are configured

### 6.2 GitHub Pages Issues

**Problem**: "404 - There isn't a GitHub Pages site here"
**Solution**:
- Verify `CNAME` file exists in repository root
- Check custom domain is saved in GitHub Pages settings
- Ensure `index.html` is in repository root

**Problem**: HTTPS not working
**Solution**:
- Wait for DNS to fully propagate
- Uncheck and re-check "Enforce HTTPS"
- Wait 10-15 minutes for SSL certificate

### 6.3 Form Submission Issues

**Problem**: Form redirects to FormSubmit page showing email in URL
**Solution**:
- Add `_next` hidden field with your career page URL
- Implement success message with JavaScript
- Use `window.history.replaceState()` to clean URL
```html
<input type="hidden" name="_next" value="https://www.nkps.ai/career.html?success=true">
```

**Problem**: Form not submitting
**Solution**:
- Check if FormSubmit email is confirmed
- Verify form action URL is correct
- Check browser console for errors

**Problem**: Not receiving emails
**Solution**:
- Check spam/junk folder
- Verify email address in form action
- Confirm FormSubmit activation email was clicked

**Problem**: Resume not attached
**Solution**:
- Verify `enctype="multipart/form-data"` is in form tag
- Check file size (FormSubmit limit: 10MB)
- Ensure file input has `name="Resume"` attribute

**Problem**: Success message not showing
**Solution**:
- Verify `_next` parameter includes `?success=true`
- Check if JavaScript is loaded (jQuery required)
- Ensure `successAlert` div ID matches in HTML and JavaScript

**Problem**: Email visible in browser URL after submission
**Solution**:
- This was the default FormSubmit behavior
- Fixed by adding custom `_next` redirect parameter
- JavaScript cleans the URL after showing success message

---

## 7. Summary

### What Was Accomplished

✅ **Website Deployment**:
- Static HTML website hosted on GitHub Pages
- Free hosting with unlimited bandwidth
- Automatic HTTPS with SSL certificate

✅ **Custom Domain**:
- Connected GoDaddy domain `nkps.ai` to GitHub Pages
- Both `nkps.ai` and `www.nkps.ai` work
- HTTPS enabled for secure connections

✅ **Career Form**:
- Integrated FormSubmit.co for form handling
- Supports file uploads (resume)
- Email notifications to `satheeshreddy92@gmail.com`
- Spam protection with honeypot
- Custom redirect to stay on career page
- Success message displayed without exposing email
- Clean URL (no parameters visible after submission)
- No backend server required

### Key Technologies Used
- **Hosting**: GitHub Pages (Free)
- **Domain**: GoDaddy DNS
- **Form Service**: FormSubmit.co (Free)
- **SSL**: Let's Encrypt (via GitHub Pages)

### Common Issues Resolved
1. **DNS Conflicts**: Removed "WebsiteBuilder Site" record from GoDaddy
2. **Email Exposure**: Added custom redirect to prevent email showing in URL
3. **User Experience**: Implemented on-page success message instead of external redirect
4. **File Uploads**: Configured FormSubmit.co to support resume attachments

### Maintenance
- **DNS Records**: No changes needed unless moving hosting
- **Form Email**: Update in `career.html` if email changes
- **Content Updates**: Push changes to GitHub repository

---

## 8. Future Enhancements

### Potential Improvements
1. **Custom Thank You Page**: Create a dedicated success page after form submission
2. **Form Validation**: Add client-side validation with JavaScript
3. **Analytics**: Add Google Analytics for visitor tracking
4. **Contact Form**: Add similar form integration to contact page
5. **Email Templates**: Customize FormSubmit email templates
6. **Multiple Recipients**: Add CC/BCC for form submissions
7. **Auto-responder**: Send confirmation email to applicants

### Alternative Form Services (if needed)
- **Getform.io**: 100 submissions/month, file uploads
- **Formspree**: 50 submissions/month, no file uploads on free plan
- **Web3Forms**: Unlimited, but basic features
- **Netlify Forms**: 100 submissions/month (if hosting on Netlify)

---

## Contact & Support

**Website**: https://www.nkps.ai  
**Repository**: https://github.com/sachitti/NK-Website-29-10-21  
**Form Submissions**: satheeshreddy92@gmail.com

**Documentation Created**: February 2026  
**Last Updated**: February 2026

---

## Appendix: Quick Reference

### GitHub Pages URL
```
https://sachitti.github.io/NK-Website-29-10-21/
```

### Custom Domain
```
https://www.nkps.ai
```

### FormSubmit Endpoint
```
https://formsubmit.co/satheeshreddy92@gmail.com
```

### GitHub DNS IPs
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### GoDaddy Nameservers
```
ns17.domaincontrol.com
ns18.domaincontrol.com
```

---

**End of Documentation**
