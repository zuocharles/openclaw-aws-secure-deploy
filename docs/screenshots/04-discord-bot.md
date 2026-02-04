# Step 4: Create Discord Bot

> **TODO:** Add screenshots for each step below

---

## Overview

Your OpenClaw instance needs a Discord bot to send and receive messages. This guide walks you through creating one.

**Estimated time:** 5 minutes  
**Cost:** Free

---

## Step 4.1: Go to Discord Developer Portal

1. Visit: https://discord.com/developers/applications
2. Log in with your Discord account

**[SCREENSHOT NEEDED]:** Discord Developer Portal homepage

---

## Step 4.2: Create New Application

1. Click the blue "New Application" button (top right)

**[SCREENSHOT NEEDED]:** Applications page with "New Application" button

---

## Step 4.3: Name Your Application

1. **Name:** Enter a name for your bot (e.g., "My OpenClaw Bot")
2. Check the Terms of Service box
3. Click "Create"

**[SCREENSHOT NEEDED]:** Create application dialog with name entered

---

## Step 4.4: Go to Bot Section

1. In the left sidebar, click "Bot"

**[SCREENSHOT NEEDED]:** Application settings with "Bot" link highlighted

---

## Step 4.5: Add Bot to Application

1. Click "Add Bot" button
2. Click "Yes, do it!" to confirm

**[SCREENSHOT NEEDED]:** Confirmation dialog for adding bot

---

## Step 4.6: Get Your Bot Token

⚠️ **IMPORTANT:** This token is like a password for your bot. Never share it!

1. Under "TOKEN", click "Reset Token"
2. Click "Yes, do it!" to confirm
3. Click "Copy" to copy the token
4. Save it somewhere safe - you'll paste it into the setup script

**[SCREENSHOT NEEDED]:** Bot page showing the token (with actual value blurred)

---

## Step 4.7: Enable Message Content Intent

**This is required for OpenClaw to read messages!**

1. Scroll down to "Privileged Gateway Intents"
2. Toggle ON "Message Content Intent"
3. Click "Save Changes"

**[SCREENSHOT NEEDED]:** Message Content Intent toggle enabled

---

## 🎉 Bot Created!

You now have:
- ✅ Bot created
- ✅ Bot token copied
- ✅ Message Content Intent enabled

**Next steps:**
- [Get your Discord User ID](05-discord-userid.md)
- [Invite bot to your server](06-discord-invite.md)

---

## Keep Your Token Safe!

The bot token gives full control of your bot. If someone gets it, they can:
- Send messages as your bot
- Read all messages your bot can see
- Potentially access your OpenClaw server

**Never:**
- Share the token in screenshots
- Commit it to GitHub
- Send it over unencrypted chat
- Store it in plain text notes

**The setup script will:**
- Ask for the token once
- Store it securely on your server only
- Never transmit it elsewhere
