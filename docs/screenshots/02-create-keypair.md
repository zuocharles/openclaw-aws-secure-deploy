# Step 2: Create SSH Key Pair

> **TODO:** Add screenshots for each step below

---

## Overview

An SSH key pair is like a password for connecting to your server. It has two parts:
- **Public key** - Stays on the AWS server
- **Private key** - Stays on your computer (NEVER share this!)

**Estimated time:** 2 minutes

---

## Step 2.1: Navigate to EC2

1. In AWS Console, click "Services" in the top menu
2. Search for "EC2" and click it

**[SCREENSHOT NEEDED]:** AWS Console showing Services dropdown with EC2 highlighted

---

## Step 2.2: Go to Key Pairs

1. In the left sidebar, scroll down to "Network & Security"
2. Click "Key Pairs"

**[SCREENSHOT NEEDED]:** EC2 dashboard with Key Pairs link highlighted

---

## Step 2.3: Create New Key Pair

1. Click the orange "Create key pair" button

**[SCREENSHOT NEEDED]:** Key Pairs page with "Create key pair" button highlighted

---

## Step 2.4: Configure Key Pair

1. **Name:** Enter a name (e.g., `openclaw-key`)
2. **Key pair type:** Select "RSA"
3. **Private key file format:** 
   - If you're on Mac/Linux: Select ".pem"
   - If you're on Windows: Select ".ppk"

**[SCREENSHOT NEEDED]:** Create key pair form filled out

---

## Step 2.5: Download the Key

1. Click "Create key pair"
2. Your browser will download a `.pem` (or `.ppk`) file
3. **IMPORTANT:** Move this file to a safe location and remember where you put it!

**[SCREENSHOT NEEDED]:** Download dialog showing the .pem file

---

## Step 2.6: Set Proper Permissions (Mac/Linux)

If you're on Mac or Linux, you need to protect the key file:

```bash
# Navigate to where you downloaded the key
cd ~/Downloads

# Set permissions so only you can read it
chmod 400 openclaw-key.pem
```

**[SCREENSHOT NEEDED]:** Terminal showing the chmod command

On Windows, the permissions are handled automatically.

---

## 🎉 Key Pair Created!

Your SSH key is ready. You'll use it when:
- The automated script asks for your key name
- You manually SSH into your server

**Next step:** [Get AWS Credentials](03-get-credentials.md)

---

## Important Security Notes

⚠️ **NEVER share your .pem file with anyone**  
⚠️ **NEVER commit it to GitHub**  
⚠️ **Store it like a password - anyone with this file can access your server**

If you lose this file, you'll need to create a new key pair and replace it on your server.
