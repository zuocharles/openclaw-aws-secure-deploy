# OpenClaw AWS Secure Deploy

[![Security Hardened](https://img.shields.io/badge/security-hardened-green.svg)](docs/vulnerabilities.md)
[![AWS Ready](https://img.shields.io/badge/AWS-EC2%20Ready-orange.svg)](terraform/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**The easiest way to deploy a secure OpenClaw instance on AWS.**

Deploy your personal AI assistant with enterprise-grade security in under 30 minutes — even if you're not technical. This project addresses all 10 known OpenClaw vulnerabilities identified by security researchers.

---

## ⚡ Quick Start (Choose Your Path)

We offer two ways to deploy, depending on your comfort level:

### Path A: Automated (15 min) ⭐ Easiest

One command creates everything for you:

```bash
# Download and run the automated setup
curl -sL https://raw.githubusercontent.com/zuocharles/openclaw-aws-secure-deploy/main/scripts/setup.sh | bash
```

**What you'll need:**
- AWS account + credentials ([guide](docs/screenshots/))
- SSH key pair ([guide](docs/screenshots/02-create-keypair.md))
- Discord bot token ([guide](docs/screenshots/04-discord-bot.md))
- Discord User ID ([guide](docs/screenshots/05-discord-userid.md))

**[→ Full Automated Guide](docs/quickstart.md#path-a-automated-setup-15-min)**

---

### Path B: Manual (30 min) 🔒 Most Control

Create the EC2 instance yourself, then run hardening commands:

```bash
# SSH into your server
ssh -i your-key.pem ubuntu@YOUR_EC2_IP

# Run setup commands individually
git clone https://github.com/zuocharles/openclaw-aws-secure-deploy.git
cd openclaw-aws-secure-deploy
# Follow the step-by-step guide
```

**[→ Full Manual Guide](docs/quickstart.md#path-b-manual-setup-30-min)**

---

## 🤔 Which Path Should I Choose?

| | **Path A: Automated** | **Path B: Manual** |
|---|---|---|
| **Time** | ~15 minutes | ~30 minutes |
| **Effort** | Answer prompts | Follow each step |
| **AWS credentials** | Script uses them briefly | You create resources manually |
| **Learning** | Less | More |
| **Best for** | "Just get it working" | "I want to understand everything" |

**Concerned about giving the script AWS credentials?**  
Read our [Transparency Guide](TRUST.md) — or choose Path B!

---

## Why This Exists

Security researchers found [1,800+ exposed OpenClaw instances](https://www.shodan.io/search?query=clawdbot-gw) on the public internet with:
- Gateway control planes accessible to anyone
- DM policies allowing strangers to control bots
- Plaintext credentials readable by other processes
- No network isolation for command execution

**This project fixes all of that.**

### The Risk is Real

Security researchers have been documenting these issues:

> **[@DanielMiessler](https://x.com/DanielMiessler/status/2015865548714975475)** - Security expert discussing the broader implications of exposed AI agent infrastructure.

> **[@UK_Daniel_Card](https://x.com/UK_Daniel_Card/status/2015685932184219998)** - Shared Shodan scans showing exposed instances and documented the attack surface of publicly accessible OpenClaw gateways.

> **[@somi_ai](https://x.com/somi_ai/status/2016018694636515666)** - Highlighted the security risks of OpenClaw running wild in business environments with direct access to email, files, and system tools.

> **[@lucatac0](https://x.com/lucatac0/status/2015473205863948714)** - Documented over 1,000 exposed instances via Shodan, showing API keys (Anthropic, OpenAI, Telegram) and chat histories accessible without authentication.

> **[Cisco AI Defense Research](https://www.cisco.com)** - Found that cryptocurrency private keys could be extracted in under 5 minutes via crafted email prompt injection.

**The core problem:** OpenClaw collapses multiple trust boundaries into a single point of failure. **If compromised once, attackers inherit everything.**

---

## What You Get

After setup, you'll have:

| Security Feature | Protection |
|-----------------|------------|
| 🔒 **No Public Ports** | Firewall blocks all internet access |
| 🔑 **VPN-Only SSH** | Tailscale required for any server access |
| 🤖 **DM Allowlist** | Only your Discord user ID can control the bot |
| 🛡️ **Docker Sandbox** | Commands run in isolated containers |
| 📊 **Auto Updates** | Weekly security patches |
| 🔍 **Security Audits** | Daily automated vulnerability scans |

---

## Prerequisites

Before starting, you need:

- [ ] AWS account ([create one](docs/screenshots/01-aws-signup.md))
- [ ] Tailscale account ([free signup](https://tailscale.com))
- [ ] Discord account ([create one](https://discord.com))
- [ ] LLM API key (Anthropic, OpenAI, etc.)
- [ ] ~20 minutes of time

**Total estimated cost:** $15-30/month (AWS t3.small instance)

---

## Vulnerability Coverage

| # | Vulnerability | Status | How We Fix It |
|---|--------------|--------|---------------|
| 1 | Gateway exposed on 0.0.0.0:18789 | ✅ Fixed | UFW + Tailscale VPN + auth token |
| 2 | DM policy allows all users | ✅ Fixed | Allowlist with your user ID only |
| 3 | Sandbox disabled by default | ✅ Fixed | Docker with network=none |
| 4 | Credentials in plaintext | ⚠️ Partial | chmod 700/600 (see limitations) |
| 5 | Prompt injection via web content | ⚠️ Mitigated | Defense-in-depth (sandbox + allowlist) |
| 6 | Dangerous commands unblocked | ❌ Cannot Fix | OpenClaw doesn't support blocklists |
| 7 | No network isolation | ✅ Fixed | Docker isolated network |
| 8 | Elevated tool access | 📖 Documented | Recommend Cisco Skill Scanner |
| 9 | No audit logging | ✅ Fixed | Logging enabled + security audits |
| 10 | Weak pairing codes | ⚠️ Partial | fail2ban protects SSH only |

### Known Limitations

Some vulnerabilities cannot be fully addressed by deployment configuration:

- **#4 Credentials**: File permissions block other local users, but credentials remain plaintext on disk. If the OpenClaw process is compromised, permissions don't help.
- **#6 Commands**: OpenClaw does not support a `blockedCommands` configuration option. The Docker sandbox provides partial protection.
- **#10 Pairing**: fail2ban protects SSH, but pairing codes are application-layer. Complete pairing immediately after setup.

See [docs/vulnerabilities.md](docs/vulnerabilities.md) for detailed explanations.

---

## Security Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        PUBLIC INTERNET                          │
│                    (Blocked by UFW firewall)                    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ ❌ ALL PORTS BLOCKED
                              │    (except Tailscale UDP 41641)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      AWS EC2 INSTANCE                           │
│  ┌───────────────┐    ┌───────────────┐    ┌───────────────┐   │
│  │   UFW         │    │   Tailscale   │    │   OpenClaw    │   │
│  │   Firewall    │───▶│   VPN         │───▶│   Gateway     │   │
│  │               │    │               │    │   127.0.0.1   │   │
│  │ Default: DENY │    │ Encrypted     │    │   :18789      │   │
│  └───────────────┘    └───────────────┘    └───────────────┘   │
│                                                   │             │
│                                            ┌──────┴──────┐      │
│                                            │   Docker    │      │
│                                            │   Sandbox   │      │
│                                            │ network=none│      │
│                                            └─────────────┘      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ ✅ Encrypted Tailscale Tunnel
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     YOUR DEVICES ONLY                           │
│              (Authenticated via Tailscale account)              │
└─────────────────────────────────────────────────────────────────┘
```

---

## Documentation

- **[Quick Start](docs/quickstart.md)** - Get running in 15-30 minutes
- **[Full Guide](docs/full-guide.md)** - Complete step-by-step instructions
- **[Trust & Security](TRUST.md)** - What the script does and doesn't do
- **[Vulnerabilities](docs/vulnerabilities.md)** - Security deep-dive
- **[Screenshot Guides](docs/screenshots/)** - Visual walkthroughs for each step
- **[Advanced Topics](docs/advanced/)** - Backups, monitoring, secrets rotation

---

## What's Included

```
openclaw-aws-secure-deploy/
├── README.md                 # This file
├── TRUST.md                  # Security transparency document
├── scripts/
│   ├── setup.sh              # Master setup script
│   ├── maintenance.sh        # Weekly maintenance
│   └── check-security.sh     # Quick security check
├── terraform/                # AWS infrastructure as code
├── config-templates/         # Secure configuration files
└── docs/
    ├── quickstart.md         # 15-30 min quick start
    ├── full-guide.md         # Complete step-by-step guide
    ├── vulnerabilities.md    # Security deep-dive
    ├── screenshots/          # Visual setup guides
    │   ├── 01-aws-signup.md
    │   ├── 02-create-keypair.md
    │   ├── 03-get-credentials.md
    │   ├── 04-discord-bot.md
    │   ├── 05-discord-userid.md
    │   └── 06-discord-invite.md
    └── advanced/             # Secrets rotation, backups, etc.
```

---

## Comparison with Other Tools

| Feature | This Project | openclaw-ansible | openclaw-security-scan | SimpleClaw |
|---------|-------------|------------------|----------------------|------------|
| **AWS-specific** | ✅ Terraform included | ❌ Generic | ❌ Scanner only | ✅ Hosted service |
| **Self-hosted** | ✅ You own the server | ✅ Self-hosted | ✅ Self-hosted | ❌ They host it |
| **Privacy** | ✅ Your data stays yours | ✅ Your data stays yours | ✅ Your data stays yours | ⚠️ Third-party hosted |
| **Beginner-friendly** | ✅ Guided setup | ⚠️ Ansible knowledge needed | ✅ Simple CLI | ✅ Easiest |
| **Full deployment** | ✅ End-to-end | ✅ End-to-end | ❌ Detection only | ✅ Managed service |
| **Cost** | $15-30/month AWS | $15-30/month AWS | Free (scanner only) | Subscription |
| **Customizable** | ✅ Full control | ✅ Full control | ❌ Scanner only | ⚠️ Limited |

**Why choose this over SimpleClaw?**
- Your data stays on your server (not someone else's)
- You control the infrastructure
- Add any skills or customizations you want
- No subscription fees (just AWS costs)

---

## Support

- **Quick Start:** [docs/quickstart.md](docs/quickstart.md)
- **Issues:** [GitHub Issues](https://github.com/zuocharles/openclaw-aws-secure-deploy/issues)
- **Discussions:** [GitHub Discussions](https://github.com/zuocharles/openclaw-aws-secure-deploy/discussions)
- **OpenClaw Docs:** [docs.openclaw.ai](https://docs.openclaw.ai)

---

## Credits

- Security research by [Cisco AI Defense](https://www.cisco.com), [Vectra AI](https://www.vectra.ai), and community researchers
- Inspired by findings from [@dvulnresearch](https://twitter.com/dvulnresearch), [@UK_Daniel_Card](https://twitter.com/UK_Daniel_Card)
- Built on [OpenClaw](https://openclaw.ai)

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

**Ready to start?** → [Quick Start Guide](docs/quickstart.md)
