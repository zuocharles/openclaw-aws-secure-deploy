# Trust & Security Transparency

We understand that running a script that asks for your AWS credentials requires trust. This document explains exactly what our setup script does, what data it accesses, and how you can verify this for yourself.

---

## What This Script Does NOT Do

**We never:**
- ❌ Send your credentials to any external server
- ❌ Log your API keys or tokens anywhere except your own server
- ❌ Make network requests to domains other than AWS and official package repositories
- ❌ Install software from untrusted sources

---

## What This Script DOES Do

### AWS API Calls Made

The script makes exactly **3 types of AWS API calls** (only if you choose the automated path):

```bash
# 1. Create EC2 instance
aws ec2 run-instances \
  --image-id ami-xxxxx \
  --instance-type t3.small \
  --key-name your-key-name \
  --security-group-ids sg-xxxxx

# 2. Wait for instance to be ready
aws ec2 describe-instances \
  --instance-ids i-xxxxx

# 3. Get instance IP address
aws ec2 describe-instances \
  --query 'Reservations[0].Instances[0].PublicIpAddress'
```

**Your AWS credentials:**
- Are used only for these API calls during setup
- Are NOT stored in any file (they stay in memory only during script execution)
- Are NOT sent to any third-party service
- You can revoke them immediately after setup in AWS Console

---

## How to Verify This Yourself

### Option 1: Read the Script First (Recommended)

```bash
# Download the script WITHOUT executing it
curl -sL https://raw.githubusercontent.com/zuocharles/openclaw-aws-secure-deploy/main/scripts/setup.sh -o setup.sh

# Read it in any text editor
nano setup.sh
# or
vim setup.sh
# or open in your GUI text editor

# Look for any suspicious network calls
grep -n "curl\|wget\|http" setup.sh

# Only run it when you're satisfied
bash setup.sh
```

### Option 2: Use the Manual Path

If you don't want to provide AWS credentials to any script:

1. **Create the EC2 instance manually** in AWS Console (we provide step-by-step screenshots)
2. **SSH into the instance** yourself
3. **Run only the server-side hardening script** (no AWS credentials needed)

This way, you maintain full control and the script never touches your AWS account.

### Option 3: Audit Network Traffic

Run the script while monitoring network connections:

```bash
# Terminal 1: Monitor network connections
sudo tcpdump -i any -n host ec2.amazonaws.com or host amazonaws.com

# Terminal 2: Run the script
bash setup.sh
```

You'll see only AWS API calls — no connections to third-party tracking or data collection services.

---

## Where Your Data Goes

| Data | Stored Where | Encrypted? | Notes |
|------|-------------|------------|-------|
| AWS Access Key | Nowhere (memory only) | N/A | Used once, never persisted |
| Discord Bot Token | `~/.openclaw/openclaw.json` | ❌ File permissions only | `chmod 600` restricts to your user |
| OpenAI/Anthropic API Key | `~/.openclaw/credentials/` | ❌ File permissions only | `chmod 600` restricts to your user |
| Tailscale Auth Key | Nowhere (browser flow) | N/A | Authenticated via browser |
| SSH Private Key | Your local machine | ❌ | Never leaves your computer |

**Important:** Credentials are stored in plaintext on disk with restrictive file permissions. This is a limitation of OpenClaw itself — if someone gains access to your server as your user, they can read these files.

---

## What Gets Installed

The script installs only open-source software from official repositories:

1. **Node.js 22** - From NodeSource (official)
2. **OpenClaw** - From npm registry (official)
3. **Docker** - From Docker's official install script
4. **Tailscale** - From Tailscale's official install script
5. **System packages** - From Ubuntu's official repositories (ufw, fail2ban, etc.)

All installation sources are cryptographically signed and verified.

---

## If You're Still Unsure

We recommend the **Manual Path**:

1. Create EC2 instance yourself (we provide screenshots for every click)
2. SSH in manually
3. Review and run each command individually from our guide

This takes longer (~30 minutes vs ~15 minutes) but gives you complete control and visibility.

---

## Report Security Issues

If you find any security issues with our scripts or documentation:

1. **DO NOT** open a public GitHub issue
2. Email: [your-security-email@example.com]
3. We'll respond within 48 hours

---

## Code Signing (Coming Soon)

We plan to add:
- SHA256 checksums for all scripts
- GPG signatures for releases
- Reproducible builds

Track progress: [GitHub Issue #XX](https://github.com/zuocharles/openclaw-aws-secure-deploy/issues)

---

*Last updated: February 2026*
