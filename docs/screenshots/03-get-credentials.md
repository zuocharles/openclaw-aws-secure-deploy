# Step 3: Get AWS Credentials

> **TODO:** Add screenshots for each step below

---

## Overview

AWS credentials let the setup script create your EC2 instance automatically. You'll create:
- **Access Key ID** (like a username)
- **Secret Access Key** (like a password)

**⚠️ Security Note:** These credentials are powerful. The script uses them only during setup and doesn't store them. See [TRUST.md](../../TRUST.md) for full details.

**Estimated time:** 3 minutes

---

## Step 3.1: Go to IAM

1. In AWS Console, click "Services" in the top menu
2. Search for "IAM" and click it

**[SCREENSHOT NEEDED]:** AWS Console showing Services dropdown with IAM highlighted

---

## Step 3.2: Navigate to Users

1. In the left sidebar, click "Users"

**[SCREENSHOT NEEDED]:** IAM dashboard with Users link highlighted

---

## Step 3.3: Create New User

1. Click the blue "Create user" button

**[SCREENSHOT NEEDED]:** Users list page with "Create user" button

---

## Step 3.4: Set User Name

1. **User name:** Enter a name (e.g., `openclaw-setup`)
2. Click "Next"

**[SCREENSHOT NEEDED]:** User details page with name entered

---

## Step 3.5: Set Permissions

1. Select "Attach policies directly"
2. Search for and select "AmazonEC2FullAccess"
3. Click "Next"

**[SCREENSHOT NEEDED]:** Permissions page with AmazonEC2FullAccess selected

---

## Step 3.6: Review and Create

1. Review the settings
2. Click "Create user"

**[SCREENSHOT NEEDED]:** Review page with "Create user" button

---

## Step 3.7: Create Access Key

1. Click on the user name you just created
2. Go to the "Security credentials" tab
3. Scroll down to "Access keys"
4. Click "Create access key"

**[SCREENSHOT NEEDED]:** User details page with "Create access key" button

---

## Step 3.8: Select Use Case

1. Select "Command Line Interface (CLI)"
2. Check the confirmation box
3. Click "Next"

**[SCREENSHOT NEEDED]:** Access key use case selection

---

## Step 3.9: Set Description (Optional)

1. Add a description (e.g., "OpenClaw setup script")
2. Click "Create access key"

**[SCREENSHOT NEEDED]:** Description field filled in

---

## Step 3.10: Copy Your Credentials

⚠️ **IMPORTANT:** This is the ONLY time you'll see the Secret Access Key!

1. Click "Show" next to Secret access key
2. Copy both values:
   - **Access key** (starts with AKIA...)
   - **Secret access key** (long random string)
3. Save them somewhere safe temporarily (you'll paste them into the script)
4. Click "Done"

**[SCREENSHOT NEEDED]:** Credentials page with both keys visible (blur the actual values!)

---

## 🎉 Credentials Created!

You now have:
- ✅ Access Key ID
- ✅ Secret Access Key
- ✅ SSH Key Pair name (from Step 2)

These are the three things the automated script needs!

**Next step:** Run the automated setup or [create Discord bot](04-discord-bot.md)

---

## Security Best Practices

1. **Delete the credentials after setup:**
   - Go back to IAM → Users → your user → Security credentials
   - Delete the access key once your server is running

2. **Rotate credentials regularly:**
   - Create new keys every 90 days
   - Delete old ones immediately

3. **Monitor usage:**
   - In IAM, you can see when keys were last used
   - If you see unexpected usage, rotate immediately

---

## If You Lose the Secret Key

If you didn't copy the secret key or lost it:
1. Go to IAM → Users → your user
2. Delete the old access key
3. Create a new one
4. **You cannot retrieve a secret key after closing the page!**
