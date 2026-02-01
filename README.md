# OpenClaw

Deploy your own AI assistant powered by Anthropic's Claude on Cloudflare Workers with persistent R2 storage.

## What is OpenClaw?

OpenClaw (formerly Clawdbot/Moltbot) is a self-hosted AI assistant that runs on Cloudflare's edge network. It provides a personal Claude-powered chat interface with device pairing and persistent conversation storage.

## Prerequisites

- **Cloudflare account** — Free tier works, but requires paid Workers plan ($5/month)
- **GitHub account** — Free
- **Anthropic API key** — From [console.anthropic.com](https://console.anthropic.com)

## Quick Start

1. **Deploy from GitHub** — Visit the [moltworker repository](https://github.com/cloudflare/moltworker) and click "Deploy to Cloudflare"

2. **Upgrade Plans** — Enable the Workers paid plan ($5/month) and R2 storage (pay-per-use)

3. **Configure Secrets** — Add your Anthropic API key and generate a secure access token

4. **Set Up Zero Trust** — Configure `CF_ACCESS_TEAM_DOMAIN` and `CF_ACCESS_AUD` environment variables

5. **Enable R2 Storage** — Add `CF_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, and `R2_SECRET_ACCESS_KEY`

6. **Access Your Instance** — Navigate to your worker URL with your token appended

## Setup Phases Overview

| Phase | Description |
|-------|-------------|
| 1 | Cloudflare account & billing setup |
| 2 | GitHub connection & initial deployment |
| 3 | Zero Trust configuration (critical!) |
| 4 | First access & device pairing |
| 5 | R2 storage credentials |

## Common Issues

- **Token error on first visit** — Append your token to the URL
- **Device pairing error** — Click the approval link in the error message
- **R2 Storage Not Configured** — You need to add the R2 credentials as environment variables
- **Zero Trust settings missing** — Check Left menu → Zero Trust → Settings

## Environment Variables Reference

| Variable | Purpose |
|----------|---------|
| `CF_ACCESS_TEAM_DOMAIN` | Your Zero Trust team domain |
| `CF_ACCESS_AUD` | Application Audience tag |
| `CF_ACCOUNT_ID` | Your Cloudflare account ID |
| `R2_ACCESS_KEY_ID` | R2 API access key |
| `R2_SECRET_ACCESS_KEY` | R2 API secret key |

## Need Detailed Instructions?

For step-by-step guidance including screenshots and troubleshooting, refer to the full **OpenClaw Cloudflare Setup Guide**.

## License

MIT
