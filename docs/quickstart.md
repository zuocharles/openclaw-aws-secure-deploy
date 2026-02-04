# Quick Start Guide

**Deploy your secure OpenClaw instance in 15-30 minutes.**

Choose your path based on your comfort level:

---

## Path A: Automated Setup (15 min) ⭐ Recommended

**Best for:** Users who are comfortable running a script that creates AWS resources

**What you'll do:**
1. Create AWS account (if needed)
2. Get AWS credentials (2 minutes)
3. Run one command
4. Answer a few prompts
5. Create Discord bot (5 minutes)

### Step-by-Step

#### Step 1: Pre-Flight Checklist

Before running the script, you need:

- [ ] AWS account ([create one](docs/screenshots/01-aws-signup.md))
- [ ] SSH key pair created in AWS ([see guide](docs/screenshots/02-create-keypair.md))
- [ ] AWS Access Key ID and Secret Key ([see guide](docs/screenshots/03-get-credentials.md))
- [ ] Tailscale account (free) - [sign up here](https://login.tailscale.com/start)

#### Step 2: Run the Automated Script

```bash
# Download the script (read it first if you want!)
curl -sL https://raw.githubusercontent.com/zuocharles/openclaw-aws-secure-deploy/main/scripts/setup.sh -o setup.sh

# Optional: Review what it does
less setup.sh

# Run it
bash setup.sh
```

The script will ask for:
- Your AWS Access Key ID
- Your AWS Secret Access Key
- Your preferred AWS region (default: us-east-1)
- Your SSH key name (from Step 1)

Then it will:
1. Create an EC2 instance
2. Harden the server security
3. Install Tailscale VPN
4. Install OpenClaw
5. Configure everything securely

#### Step 3: Create Your Discord Bot

While the server is being set up:

- [ ] Create a Discord bot ([see guide](docs/screenshots/04-discord-bot.md))
- [ ] Get your Discord User ID ([see guide](docs/screenshots/05-discord-userid.md))
- [ ] Invite bot to your server ([see guide](docs/screenshots/06-discord-invite.md))

The script will pause and ask for:
- Discord Bot Token
- Your Discord User ID

#### Step 4: Connect Tailscale

When prompted:
1. Click the link shown in your terminal
2. Log in to Tailscale in your browser
3. Click "Connect" or "Authorize"
4. Return to the terminal

#### Step 5: Done!

The script will verify everything is working and show you:
- Your server's Tailscale IP
- How to connect via SSH
- How to check status

---

## Path B: Manual Setup (30 min)

**Best for:** Users who want full control or don't want to provide AWS credentials to a script

**What you'll do:**
1. Create EC2 instance manually in AWS Console
2. SSH in and run commands individually
3. Configure everything step-by-step

### Step-by-Step

#### Step 1: Create AWS EC2 Instance

Follow our screenshot guide: [Create EC2 Instance](docs/screenshots/01-aws-signup.md)

**Settings to use:**
- **AMI:** Ubuntu Server 24.04 LTS
- **Instance Type:** t3.small (or t3.medium for better performance)
- **Storage:** 30 GB gp3
- **Security Group:** Allow SSH (port 22) from anywhere (we'll restrict this later)

#### Step 2: Connect to Your Server

```bash
# Use the key you downloaded when creating the instance
ssh -i ~/Downloads/your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

#### Step 3: Run Commands Individually

Instead of running the automated script, follow our manual guide:

📖 [Full Manual Guide](docs/full-guide.md)

Or run individual scripts:

```bash
# Download the repo
git clone https://github.com/zuocharles/openclaw-aws-secure-deploy.git
cd openclaw-aws-secure-deploy

# Run each phase separately:

# Phase 2: Harden server
sudo bash -c '
  apt update && apt upgrade -y
  apt install -y ufw fail2ban unattended-upgrades curl wget git
  # ... (see full guide for complete commands)
'

# Phase 3: Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

# Phase 4: Install OpenClaw
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
curl -fsSL https://get.docker.com | sh
sudo npm install -g openclaw

# Phase 5: Configure OpenClaw
openclaw onboard
# ... (see full guide for security configuration)
```

---

## Comparison

| Feature | Path A: Automated | Path B: Manual |
|---------|-------------------|----------------|
| Time | ~15 minutes | ~30 minutes |
| AWS credentials in script | Yes (memory only) | No |
| Control | High | Maximum |
| Error handling | Automatic | Manual |
| Learning opportunity | Less | More |
| Comfort level | "I trust the script" | "I want to see every step" |

---

## Common Concerns

### "Is it safe to give the script my AWS credentials?"

**Short answer:** The script uses them only to create your EC2 instance, then discards them.

**Long answer:** See [TRUST.md](TRUST.md) for complete transparency on:
- Exactly what API calls are made
- How to verify the script yourself
- Alternative manual approach
- What data is stored where

### "What if something goes wrong?"

The automated script:
- Shows progress at each step
- Can be re-run to resume from where it failed
- Creates detailed logs at `~/.openclaw-setup.log`
- Won't charge you unexpectedly (AWS free tier covers this)

### "I got an error, what do I do?"

1. Check the log: `cat ~/.openclaw-setup.log`
2. Re-run the script: `bash setup.sh` (it resumes from where it left off)
3. Open an [issue on GitHub](https://github.com/zuocharles/openclaw-aws-secure-deploy/issues)

---

## After Setup

Once complete, you'll have:

- 🔒 **Secure server** - No public ports exposed
- 🔑 **SSH via Tailscale only** - No brute-force attacks possible
- 🤖 **Discord bot** - Responding only to you
- 🛡️ **Docker sandbox** - Commands run isolated
- 📊 **Monitoring** - Automated security audits

**Next steps:**
- [Configure additional channels](docs/full-guide.md#adding-more-channels)
- [Set up skills](docs/full-guide.md#installing-skills)
- [Learn maintenance commands](docs/full-guide.md#ongoing-maintenance)

---

## Need Help?

- 📖 Full detailed guide: [docs/full-guide.md](docs/full-guide.md)
- 🛡️ Security deep-dive: [docs/vulnerabilities.md](docs/vulnerabilities.md)
- 💬 GitHub Discussions: [github.com/zuocharles/openclaw-aws-secure-deploy/discussions](https://github.com/zuocharles/openclaw-aws-secure-deploy/discussions)
- 🐛 Bug reports: [GitHub Issues](https://github.com/zuocharles/openclaw-aws-secure-deploy/issues)
