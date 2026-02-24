# Zoho Mail Setup Guide - Complete Step-by-Step

## Overview
This guide will help you create 3 professional email addresses with your domain `nkps.ai` using Zoho Mail's free plan.

**Email addresses to create**:
- `info@nkps.ai`
- `careers@nkps.ai`
- `contact@nkps.ai`

**Cost**: FREE (up to 5 users)

---

## Part 1: Sign Up for Zoho Mail

### Step 1: Go to Zoho Mail Website
1. Open browser and go to: **https://www.zoho.com/mail/**
2. Click **"Sign Up Now"** or **"Get Started"**

### Step 2: Choose Plan
**Note**: The free "Forever Free Plan" is no longer prominently displayed. You'll see paid plans first.

**Options**:
1. **Mail Lite** (₹75/user/month) - Email + Calendar
2. **Mail Premium** (₹199/user/month) - Email + 50GB storage
3. **Workplace Standard** (₹99/user/month) - Email + Calendar + Chat + Meetings

**For Free Trial**:
- Click **"Start 15 days free trial"** on any plan
- After trial, you can downgrade or continue with paid plan

### Step 3: Enter Organization Details
1. **Organization Name**: Enter `Nkaizen` or `Nkaizen Consulting Services`
2. **Email**: Enter your personal email (e.g., `satheeshreddy92@gmail.com`)
   - This is for account recovery only
   - Your business emails will be separate
3. **Phone Number**: Enter your phone number
4. Check **"I agree to Terms of Service and Privacy Policy"**
5. Click **"Sign Up Now"**

### Step 4: Enter Your Domain
1. In the **"Domain Name"** field, enter: `nkps.ai`
2. Click **"Add Domain"** or **"Continue"**

---

## Part 2: Verify Domain Ownership

### Step 1: Get Verification Code
1. After signup, Zoho will show **"Verify Domain Ownership"** page
2. You'll see a **TXT record** like:
   ```
   Type: TXT
   Host: @ (or leave blank)
   Value: zoho-verification=zb12345678.zmverify.zoho.com
   ```
3. **Copy this value** - you'll need it for GoDaddy

### Step 2: Add TXT Record in GoDaddy
1. Login to **GoDaddy**
2. Go to: **DNS Management** for `nkps.ai`
3. Click **"Add New Record"**
4. Configure:
   - **Type**: Select `TXT`
   - **Name**: Enter `@`
   - **Value**: Paste the Zoho verification code
   - **TTL**: `1 Hour`
5. Click **"Save"**

### Step 3: Verify in Zoho
1. Go back to Zoho Mail setup page
2. Click **"Verify"** or **"Verify Domain"**
3. Wait 5-10 minutes if verification fails (DNS propagation)
4. Try again until you see **"Domain Verified"** ✓

---

## Part 3: Configure MX Records (Email Routing)

**Important**: The "DNS Mapping" option in Zoho's left sidebar may be grayed out during trial/free plan. You'll need to add MX records manually in GoDaddy.

### Step 1: Zoho MX Record Details

You need to add these 3 MX records in GoDaddy:

| Priority | Mail Server |
|----------|-------------|
| 10 | mx.zoho.com (or mx1.zoho.com) |
| 20 | mx2.zoho.com |
| 50 | mx3.zoho.com |

### Step 2: Delete Existing MX Records in GoDaddy
1. Go to **GoDaddy DNS Management**
2. Find any existing **MX records**
3. **Delete them all** (important!)
4. Keep only NS (nameserver), SOA, and existing A/CNAME records

### Step 3: Add Zoho MX Records in GoDaddy

**Add MX Record 1:**
1. Click **"Add New Record"**
2. Configure:
   - **Type**: `MX`
   - **Name**: `@`
   - **Value**: `mx.zoho.com` or `mx1.zoho.com`
   - **Priority**: `10`
   - **TTL**: `1/2 Hour` or `1 Hour`
3. Click **"Save"**

**Add MX Record 2:**
1. Click **"Add New Record"**
2. Configure:
   - **Type**: `MX`
   - **Name**: `@`
   - **Value**: `mx2.zoho.com`
   - **Priority**: `20`
   - **TTL**: `1/2 Hour` or `1 Hour`
3. Click **"Save"**

**Add MX Record 3:**
1. Click **"Add New Record"**
2. Configure:
   - **Type**: `MX`
   - **Name**: `@`
   - **Value**: `mx3.zoho.com`
   - **Priority**: `50`
   - **TTL**: `1/2 Hour` or `1 Hour`
3. Click **"Save"**

### Step 4: Wait for DNS Propagation
**Important**: DNS changes take time to propagate.

- **Minimum wait**: 30 minutes
- **Average wait**: 1-2 hours
- **Maximum wait**: 24-48 hours (rare)

### Step 5: Verify MX Records
**Option 1: Use MX Toolbox**
1. Go to: https://mxtoolbox.com/SuperTool.aspx
2. Enter: `nkps.ai`
3. Select: **MX Lookup**
4. Click **MX Lookup**
5. You should see Zoho's MX records

**Option 2: Command Line**
```cmd
nslookup -type=MX nkps.ai
```

You should see:
```
nkps.ai MX preference = 10, mail exchanger = mx.zoho.com
nkps.ai MX preference = 20, mail exchanger = mx2.zoho.com
nkps.ai MX preference = 50, mail exchanger = mx3.zoho.com
```

---

## Part 4: Configure SPF Record (Email Authentication)

### Step 1: Add SPF Record in GoDaddy
1. Go to **GoDaddy DNS Management**
2. Click **"Add New Record"**
3. Configure:
   - **Type**: `TXT`
   - **Name**: `@`
   - **Value**: `v=spf1 include:zoho.com ~all`
   - **TTL**: `1 Hour`
4. Click **"Save"**

**Purpose**: Prevents your emails from going to spam

---

## Part 5: Configure DKIM (Optional but Recommended)

### Step 1: Get DKIM Record from Zoho
1. In Zoho Mail admin panel
2. Go to **Email Configuration** → **DKIM**
3. Click **"Add Selector"**
4. Copy the DKIM record details

### Step 2: Add DKIM Record in GoDaddy
1. Go to **GoDaddy DNS Management**
2. Click **"Add New Record"**
3. Configure:
   - **Type**: `TXT`
   - **Name**: `zoho._domainkey` (or as provided by Zoho)
   - **Value**: (paste the long DKIM value from Zoho)
   - **TTL**: `1 Hour`
4. Click **"Save"**

### Step 3: Verify DKIM in Zoho
1. Go back to Zoho DKIM page
2. Click **"Verify"**
3. Wait if needed, then verify ✓

---

## Part 6: Create Additional Email Accounts

### Step 1: Access User Management
1. After domain verification, you'll see **"Account Creation"** page
2. Enter your first email username (e.g., `info`)
3. This creates `info@nkps.ai`
4. Click **"Create"**
5. Set password and complete account setup

### Step 2: Navigate to User Management
1. You'll be redirected to **"Zoho Mail Users"** page
2. Click **"+ Add"** button (top left)

### Step 3: Create Second Email (admin@nkps.ai or careers@nkps.ai)
1. **Email Address**: Enter `admin` or `careers`
2. **First Name**: `Admin` or `Careers`
3. **Last Name**: `Team`
4. **Password**: Create a password (or auto-generate)
5. Click **"Add"** or **"Create"**

### Step 4: Create Third Email (developers@nkps.ai or contact@nkps.ai)
1. Click **"+ Add"** again
2. **Email Address**: Enter `developers` or `contact`
3. **First Name**: `Developers` or `Contact`
4. **Last Name**: `Team`
5. **Password**: Create a password
6. Click **"Add"** or **"Create"**

### Step 5: Verify All Accounts Created
You should now have 3 email accounts:
- ✓ `info@nkps.ai` (admin account)
- ✓ `admin@nkps.ai` or `careers@nkps.ai`
- ✓ `developers@nkps.ai` or `contact@nkps.ai`

**Note**: You can create up to 5 users on the trial/free plan.

---

## Part 7: Access Your Email

### Option 1: Webmail (Browser)
1. Go to: **https://mail.zoho.com/**
2. Enter email: `info@nkps.ai`
3. Enter password
4. Click **"Sign In"**

### Option 2: Mobile App
1. Download **Zoho Mail** app from:
   - iOS: App Store
   - Android: Google Play Store
2. Open app and sign in with your email

### Option 3: Email Client (Outlook, Thunderbird, etc.)

**IMAP Settings** (for receiving):
- **Server**: `imap.zoho.com`
- **Port**: `993`
- **Security**: SSL/TLS
- **Username**: `info@nkps.ai`
- **Password**: Your password

**SMTP Settings** (for sending):
- **Server**: `smtp.zoho.com`
- **Port**: `465` (SSL) or `587` (TLS)
- **Security**: SSL/TLS
- **Username**: `info@nkps.ai`
- **Password**: Your password

---

## Part 8: Update Career Form

### Update career.html to use new email
1. Open `career.html`
2. Find the form action line:
   ```html
   <form action="https://formsubmit.co/satheeshreddy92@gmail.com" method="POST">
   ```
3. Change to:
   ```html
   <form action="https://formsubmit.co/careers@nkps.ai" method="POST">
   ```
4. Save and push to GitHub

### Verify FormSubmit with New Email
1. Submit a test application
2. Check `careers@nkps.ai` inbox
3. Click the FormSubmit confirmation link (one-time)
4. Future submissions will work automatically

---

## Part 9: Testing

### Test 1: Send Test Email
1. Login to `info@nkps.ai`
2. Compose new email
3. Send to your personal email
4. Verify you receive it

### Test 2: Receive Test Email
1. Send email from your personal email
2. Send to `info@nkps.ai`
3. Check Zoho inbox
4. Verify you receive it

### Test 3: Test All Accounts
Repeat for:
- `careers@nkps.ai`
- `contact@nkps.ai`

---

## Part 10: Email Forwarding (Optional)

### Forward careers@nkps.ai to your Gmail
1. Login to Zoho Mail as `careers@nkps.ai`
2. Go to **Settings** → **Mail** → **Forwarding**
3. Click **"Add Forwarding Address"**
4. Enter: `satheeshreddy92@gmail.com`
5. Verify the forwarding email
6. Enable forwarding

**Benefit**: Receive career applications in both Zoho and Gmail

---

## Troubleshooting

### Issue: Domain verification fails
**Solution**:
- Wait 10-30 minutes for DNS propagation
- Verify TXT record is correct in GoDaddy (no typos)
- Check for trailing dots in the verification code
- Try verification again

### Issue: "DNS Mapping" is grayed out in Zoho
**Solution**:
- This is normal for trial/free accounts
- Add MX records manually in GoDaddy (see Part 3)
- No need to access DNS Mapping in Zoho

### Issue: Not receiving emails / "Message not delivered" error
**Symptoms**: 
- Email bounces with "553 Relaying disallowed" or "remote server is misconfigured"
- Sender receives delivery failure notification

**Solution**:
- MX records not yet propagated - wait 30 min to 2 hours
- Verify MX records are correct in GoDaddy
- Check spam folder
- Use MX Toolbox to verify: https://mxtoolbox.com/
- Verify sender's email is valid

### Issue: Emails going to spam
**Solution**:
- Ensure SPF record is configured
- Configure DKIM record
- Ask recipients to mark as "Not Spam"
- Build sender reputation over time

### Issue: Can't login to Zoho Mail
**Solution**:
- Verify email address is correct (include @nkps.ai)
- Reset password if needed
- Clear browser cache
- Try incognito/private mode

### Issue: MX record verification fails
**Solution**:
- Delete ALL existing MX records in GoDaddy first
- Add Zoho MX records with correct priorities
- Wait 30 minutes to 2 hours
- Use online MX checker: https://mxtoolbox.com/

### Issue: Free plan not available
**Solution**:
- Zoho no longer prominently displays free plan
- Start with 15-day free trial
- Consider alternatives:
  - ImprovMX (free email forwarding)
  - Google Workspace ($6/user/month)
  - Keep trial and evaluate before paying

---

## DNS Records Summary

After completing setup, your GoDaddy DNS should have:

### A Records (for website):
```
A | @ | 185.199.108.153 | 1 Hour
A | @ | 185.199.109.153 | 1 Hour
A | @ | 185.199.110.153 | 1 Hour
A | @ | 185.199.111.153 | 1 Hour
```

### CNAME Record (for www):
```
CNAME | www | sachitti.github.io | 1 Hour
```

### MX Records (for email):
```
MX | @ | mx.zoho.com | Priority: 10 | 1 Hour
MX | @ | mx2.zoho.com | Priority: 20 | 1 Hour
MX | @ | mx3.zoho.com | Priority: 50 | 1 Hour
```

### TXT Records (for verification & authentication):
```
TXT | @ | zoho-verification=zb12345678.zmverify.zoho.com | 1 Hour
TXT | @ | v=spf1 include:zoho.com ~all | 1 Hour
TXT | zoho._domainkey | (DKIM value) | 1 Hour
```

---

## Quick Reference

### Zoho Mail Login
- **Webmail**: https://mail.zoho.com/
- **Admin Panel**: https://mailadmin.zoho.com/

### Your Email Accounts
1. `info@nkps.ai` - General inquiries
2. `careers@nkps.ai` - Job applications
3. `contact@nkps.ai` - Contact form

### IMAP/SMTP Settings
- **IMAP**: imap.zoho.com:993 (SSL)
- **SMTP**: smtp.zoho.com:465 (SSL) or :587 (TLS)

### Support
- **Zoho Help**: https://help.zoho.com/portal/en/home
- **Community**: https://help.zoho.com/portal/en/community

---

## Next Steps

1. ✅ Complete Zoho Mail setup
2. ✅ Create 3 email accounts
3. ✅ Configure DNS records
4. ✅ Test sending and receiving
5. ✅ Update career form to use `careers@nkps.ai`
6. ✅ Set up email forwarding (optional)
7. ✅ Configure email signature
8. ✅ Set up mobile app

---

**Setup Time**: 30-60 minutes (including DNS propagation wait time)

**Documentation Created**: February 2026

---

**End of Guide**
