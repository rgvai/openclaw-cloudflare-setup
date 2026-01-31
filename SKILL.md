---
name: openclaw-cloudflare-setup
description: Step-by-step guide for deploying OpenClaw (AI assistant) on Cloudflare Workers. Use when users want to set up OpenClaw, deploy Moltworker to Cloudflare, configure Cloudflare Workers with R2 storage, or need help with the Zero Trust configuration required for OpenClaw deployment. Covers account setup, billing upgrades, GitHub integration, API key configuration, and the critical post-deployment Zero Trust settings.
---

# OpenClaw Cloudflare Setup Guide

Deploy OpenClaw (formerly Clawdbot/Moltbot) on Cloudflare Workers with R2 storage.

## Prerequisites

Before starting, ensure:
- Cloudflare account (free tier works, but paid Workers plan required)
- GitHub account (free)
- Anthropic API key from console.anthropic.com

## Phase 1: Cloudflare Account & Billing Setup

### 1.1 Account Setup
1. Navigate to cloudflare.com and sign up for free account (or login if existing)
2. **Verify logged in before proceeding**

### 1.2 Initial Deploy Attempt
1. Go to https://github.com/cloudflare/moltworker
2. Click "Deploy to Cloudflare"
3. Two "Upgrade" links appear: Workers and R2 Storage

### 1.3 Upgrade Workers Plan
1. Click the "Upgrade" link for Workers
2. Select the $5/month plan
3. Enter payment information and complete purchase

### 1.4 Upgrade R2 Storage
1. Return to https://github.com/cloudflare/moltworker
2. Click "Deploy to Cloudflare" again
3. One "Upgrade" link remains for R2 Storage
4. Click "Upgrade" for R2 (pay-per-use, no fixed cost)

## Phase 2: GitHub & Deployment Configuration

### 2.1 Connect GitHub
1. Return to https://github.com/cloudflare/moltworker
2. Click "Deploy to Cloudflare"
3. Form appears to connect GitHub account
4. If no GitHub account, create free account at github.com
5. **Login to GitHub before connecting in Cloudflare**
6. Authorize Cloudflare to access GitHub

### 2.2 Add Anthropic API Key
1. Enter your Anthropic API key in the configuration form
2. Obtain key from console.anthropic.com if needed

### 2.3 Generate and Add Token
Create a secure random token (this authenticates your requests):

**Windows PowerShell:**
```powershell
$bytes = New-Object Byte[] 32; (New-Object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($bytes); [Convert]::ToBase64String($bytes).Replace('+', '-').Replace('/', '_').TrimEnd('=')
```

**macOS/Linux:**
```bash
openssl rand -base64 32 | tr '+/' '-_' | tr -d '='
```

**Save this token securely—required for accessing OpenClaw later.**

### 2.4 Deploy
1. Paste token into the form
2. Click "Deploy"

## Phase 3: Zero Trust Configuration (Critical)

**This step is frequently missed and causes deployment failures.**

After deployment, you land on the worker sandbox dashboard. Now configure Zero Trust:

### 3.1 Get Zero Trust Values
1. Click **Zero Trust** in left-hand menu
2. Click **Settings** at bottom of menu
3. Copy your **Team domain** (format: `EXAMPLE.cloudflareaccess.com`)
4. Copy your **Application Audience (AUD)** value

### 3.2 Add Environment Variables
1. Return to Workers dashboard
2. Click **Settings**
3. Under **Variables & Secrets**, click "Add"
4. Add two secret variables:

| Type | Variable Name | Value |
|------|--------------|-------|
| Secret | `CF_ACCESS_TEAM_DOMAIN` | `EXAMPLE.cloudflareaccess.com` |
| Secret | `CF_ACCESS_AUD` | Your AUD value |

5. Click **Deploy**

## Phase 4: First Access

### 4.1 Navigate to Site
1. After deployment, click "View site"
2. **Expected:** Token error appears (token not in URL yet)

### 4.2 Add Token to URL
1. Error message shows example URL ending with `token=`
2. Copy this URL
3. Append your saved token after `token=`
4. Navigate to complete URL

### 4.3 Approve Device
1. Device pairing error appears
2. Click the link in error message
3. Click approval button to authorize device

## Phase 5: Fix "R2 Storage Not Configured" Error

After initial access, the Moltbot Admin may show:
> R2 Storage Not Configured: Missing R2_ACCESS_KEY_ID, R2_SECRET_ACCESS_KEY, CF_ACCOUNT_ID

### 5.1 Get Your CF_ACCOUNT_ID
1. Go to Cloudflare Dashboard
2. Your Account ID is in the URL: `dash.cloudflare.com/{CF_ACCOUNT_ID}/...`

### 5.2 Create/Roll R2 API Token
1. Navigate to: **Storage & databases** → **R2 object storage** → **Overview**
2. Scroll to **Account Details** → **API Tokens** and click **Manage**
3. Create new token or roll existing one:
   - Click `...` menu next to existing token → **Roll** → **Roll** to confirm
   - OR click **Create User API token** / **Create Account API token**
4. **Copy credentials immediately** (won't be shown again):
   - Access Key ID → use as `R2_ACCESS_KEY_ID`
   - Secret Access Key → use as `R2_SECRET_ACCESS_KEY`

### 5.3 Add Environment Variables to Worker
1. Navigate to: **Workers & Pages** → **moltbot-sandbox** → **Settings**
2. Under **Variables and Secrets**, click **+ Add**
3. Add three variables:

| Variable Name | Type | Value |
|---------------|------|-------|
| `CF_ACCOUNT_ID` | Secret | Your account ID from Step 5.1 |
| `R2_ACCESS_KEY_ID` | Secret | Access Key ID from Step 5.2 |
| `R2_SECRET_ACCESS_KEY` | Secret | Secret Access Key from Step 5.2 |

4. Click **Deploy**

### 5.4 Verify R2 Configuration
1. Refresh the Moltbot Admin page
2. Error should be replaced with: "R2 storage is configured. Your data will persist across container restarts."

## Phase 6: Complete

OpenClaw is now fully configured. Your paired devices and conversations will persist across container restarts.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Upgrade" links don't disappear | Wait 1-2 minutes, refresh page, retry deploy |
| Token error on first visit | Append your token to URL after `token=` |
| Device pairing error | Click link in error, approve device |
| R2 Storage Not Configured | Add CF_ACCOUNT_ID, R2_ACCESS_KEY_ID, R2_SECRET_ACCESS_KEY (see Phase 5) |
| GitHub connection fails | Ensure logged into GitHub first, then retry |
| Zero Trust settings missing | Navigate: Left menu → Zero Trust → Settings |
| Variables not saving | Ensure you click "Deploy" after adding variables |

## Historical Note

OpenClaw was originally named "Clawdbot" but renamed to avoid confusion with Anthropic's Claude. Briefly called "Moltbot" before settling on "OpenClaw."
