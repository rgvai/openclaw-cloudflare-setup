# OpenClaw

Deploy an AI assistant on Cloudflare Workers with persistent R2 storage.

## Prerequisites

- Cloudflare account (paid Workers plan required — $5/month)
- GitHub account
- Anthropic API key from [console.anthropic.com](https://console.anthropic.com)

## Quick Start

### 1. Upgrade Cloudflare Plans

1. Go to the [Moltworker repository](https://github.com/cloudflare/moltworker)
2. Click **Deploy to Cloudflare**
3. Upgrade both **Workers** ($5/month) and **R2 Storage** (pay-per-use)

### 2. Connect GitHub & Configure

1. Return to the deploy page and authorize Cloudflare to access your GitHub
2. Enter your **Anthropic API key**
3. Generate a secure token:

   **macOS/Linux:**
   ```bash
   openssl rand -base64 32 | tr '+/' '-_' | tr -d '='
   ```

   **Windows PowerShell:**
   ```powershell
   $bytes = New-Object Byte[] 32; (New-Object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($bytes); [Convert]::ToBase64String($bytes).Replace('+', '-').Replace('/', '_').TrimEnd('=')
   ```

4. Paste the token into the form and click **Deploy**

> ⚠️ **Save your token** — you'll need it to access OpenClaw.

### 3. Configure Zero Trust (Critical)

1. In the Cloudflare dashboard, navigate to **Zero Trust** → **Settings**
2. Copy your **Team domain** (e.g., `example.cloudflareaccess.com`)
3. Copy your **Application Audience (AUD)** value
4. Go to **Workers & Pages** → your worker → **Settings** → **Variables & Secrets**
5. Add these secrets:

   | Variable Name | Value |
   |---------------|-------|
   | `CF_ACCESS_TEAM_DOMAIN` | Your team domain |
   | `CF_ACCESS_AUD` | Your AUD value |

6. Click **Deploy**

### 4. First Access

1. Click **View site** — a token error will appear
2. Copy the URL and append your token after `token=`
3. Approve your device when prompted

## R2 Storage Configuration

If you see "R2 Storage Not Configured," add these environment variables:

1. Find your **Account ID** in your Cloudflare dashboard URL: `dash.cloudflare.com/{ACCOUNT_ID}/...`
2. Navigate to **Storage & databases** → **R2 object storage** → **Overview** → **Manage API Tokens**
3. Create or roll a token and copy the credentials
4. Add to your worker's **Variables & Secrets**:

   | Variable Name | Value |
   |---------------|-------|
   | `CF_ACCOUNT_ID` | Your account ID |
   | `R2_ACCESS_KEY_ID` | Access Key ID |
   | `R2_SECRET_ACCESS_KEY` | Secret Access Key |

5. Click **Deploy** and refresh the admin page

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Upgrade links persist | Wait 1–2 minutes, refresh, retry |
| Token error on first visit | Append your token to the URL |
| Device pairing error | Click the link in the error and approve |
| R2 not configured | Add the three R2 environment variables |
| GitHub connection fails | Log into GitHub first, then retry |
| Zero Trust settings missing | Left menu → Zero Trust → Settings |

## License

MIT

---

*OpenClaw was previously known as Clawdbot and Moltbot.*
