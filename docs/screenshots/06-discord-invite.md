# Step 6: Invite Bot to Your Server

> **TODO:** Add screenshots for each step below

---

## Overview

Before your bot can send or receive messages, you need to invite it to a Discord server. You can use an existing server or create a new one just for your bot.

**Estimated time:** 3 minutes

---

## Step 6.1: Go to OAuth2 Section

1. In the Discord Developer Portal, go to your application
2. In the left sidebar, click "OAuth2"
3. Click on "URL Generator"

**[SCREENSHOT NEEDED]:** Application settings with OAuth2 → URL Generator highlighted

---

## Step 6.2: Select Scopes

Under "OAuth2 URL Generator":

1. Check **"bot"**
2. Check **"applications.commands"**

**[SCREENSHOT NEEDED]:** Scopes section with both options checked

---

## Step 6.3: Select Bot Permissions

Scroll down to "Bot Permissions" and check:

- ✅ **Send Messages**
- ✅ **Send Messages in Threads**
- ✅ **Create Public Threads**
- ✅ **Embed Links**
- ✅ **Attach Files**
- ✅ **Read Message History**
- ✅ **Use Slash Commands**
- ✅ **Add Reactions**

**[SCREENSHOT NEEDED]:** Bot permissions with recommended options checked

---

## Step 6.4: Copy the Generated URL

1. Scroll to the bottom
2. Copy the generated URL

It will look something like:
```
https://discord.com/api/oauth2/authorize?client_id=YOUR_BOT_ID&permissions=12345&scope=bot%20applications.commands
```

**[SCREENSHOT NEEDED]:** Generated URL at bottom of page

---

## Step 6.5: Open the Invite Link

1. Paste the URL into your browser
2. Select the server you want to add the bot to
3. Click "Continue"

**[SCREENSHOT NEEDED]:** Authorization page with server dropdown

---

## Step 6.6: Review Permissions

1. Review the permissions the bot is requesting
2. Click "Authorize"

**[SCREENSHOT NEEDED]:** Permission review page

---

## Step 6.7: Complete CAPTCHA

1. Complete the "I am human" CAPTCHA
2. You should see "Authorized" confirmation

**[SCREENSHOT NEEDED]:** Success page showing bot has been added

---

## Step 6.8: Verify Bot is in Server

1. Go to your Discord server
2. Look for your bot in the member list (usually offline until started)
3. You can also check for a system message showing the bot joined

**[SCREENSHOT NEEDED]:** Discord server showing bot in member list

---

## 🎉 Bot Invited!

Your bot is now in your Discord server! Once you complete the setup:
- The bot will come online
- You can send it direct messages
- It will respond only to you (via the allowlist)

---

## Quick Test After Setup

Once setup is complete, test your bot:

1. Send a direct message to your bot
2. Type: "Hello, are you there?"
3. The bot should respond!

If it doesn't respond:
- Check that the setup completed successfully
- Verify your Discord User ID was entered correctly
- Check the OpenClaw logs: `openclaw logs --follow`

---

## All Discord Steps Complete! ✅

You now have:
- ✅ Discord bot created
- ✅ Bot token copied
- ✅ Your Discord User ID copied
- ✅ Bot invited to your server

**Ready to run the setup script!** Go back to [Quick Start](../quickstart.md)
