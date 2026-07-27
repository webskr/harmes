# 🚀 Master Guide: Deploying Hermes AI Agent on Hugging Face Spaces (24/7 Free)

This document provides a comprehensive, end-to-end guide for deploying and managing the **Hermes AI Agent (HuggingMes)** on **Hugging Face Spaces** with **Telegram integration**, **Cloudflare 24/7 Keep-Alive**, and **Automated State Backups**.

---

## 🎯 Architecture Overview

```
[ Telegram App ] ───(Messages)───► [ Hugging Face Space ] ───(API Request)───► [ Google Gemini / LLM ]
                                            │
                                  (Keep-Alive 24/7 Ping)
                                            │
                                  [ Cloudflare Worker ]
```

### Key Features of this Setup:
1. **100% Free 24/7 Server:** Uses Hugging Face Free CPU Space with Cloudflare Worker pinging to prevent sleep mode.
2. **Persistent Memory:** Auto-syncs chat history, skills, and settings to a private Hugging Face Dataset (`huggingmes-backup`).
3. **Multi-Interface Access:** Managed via Telegram Bot or Browser-based **Hermes Web Dashboard UI**.
4. **Security Whitelist:** Locked to your specific Telegram User ID and protected by a `GATEWAY_TOKEN` password.

---

## 🔑 Phase 1: Generating the 7 Required Secrets

Before duplicating or deploying the Space, collect these 7 secret credentials:

| Secret Name | Purpose | Where to Get | Example / Format |
| :--- | :--- | :--- | :--- |
| **`LLM_API_KEY`** | AI Brain (Gemini API) | [aistudio.google.com](https://aistudio.google.com) $\rightarrow$ *Get API Key* | `AIzaSyB...` |
| **`LLM_MODEL`** | Preferred LLM Model | Specified by user | `google/gemini-2.5-flash` or `google/gemini-2.0-flash` |
| **`TELEGRAM_BOT_TOKEN`** | Telegram Bot Connection | Telegram `@BotFather` $\rightarrow$ `/newbot` | `8822965529:AAFY4V...` |
| **`TELEGRAM_ALLOWED_USERS`** | Security Whitelist | Telegram `@userinfobot` $\rightarrow$ `/start` | `123456789` (Numeric User ID) |
| **`HF_TOKEN`** | State Backup to Dataset | [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) $\rightarrow$ *Write Access* | `hf_...` |
| **`CLOUDFLARE_WORKERS_TOKEN`** | 24/7 Keep-Alive Worker | [dash.cloudflare.com](https://dash.cloudflare.com) $\rightarrow$ *My Profile > API Tokens > Workers Edit* | `cfut_...` |
| **`GATEWAY_TOKEN`** | Web Dashboard Password | Created by you | Any strong password |

---

## 🛠️ Phase 2: Deployment Methods

### Method 1: Duplicating the Space on Hugging Face (Simplest)
1. Go to your Space on Hugging Face: `https://huggingface.co/spaces/deepak-skr/harmes`
2. **Troubleshooting - "Duplicate Button Missing / Space Paused":**
   - If the Space is paused and the `Duplicate this Space` button is hidden in the menu, use the direct URL override:
   - 👉 **`https://huggingface.co/spaces/deepak-skr/harmes?duplicate=true`**
3. Hardware: Select **CPU Basic (Free)**.
4. Fill in all **7 Secrets** in the form.
5. Click **Duplicate Space**.
6. Wait 5-10 minutes for build to complete (`Building` $\rightarrow$ `Running`).

### Method 2: Creating a New Space & Pushing from Local PC Repo
If you want 100% independent ownership using your local repository at `d:\app\HuggingMes`:

1. Go to Hugging Face $\rightarrow$ Top Right Profile $\rightarrow$ **New Space**.
2. **Name:** `harmes` | **SDK:** `Docker` | **Hardware:** `CPU Basic (Free)`.
3. In your local terminal inside `d:\app\HuggingMes`:
   ```bash
   git remote set-url origin https://huggingface.co/spaces/deepak-skr/harmes
   git push origin main --force
   ```
4. Add the 7 Secrets under **Space Settings > Secrets**.

---

## 🌐 Phase 3: Accessing & Using Your Agent

### 1. Controlling via Telegram
- Open your bot in Telegram.
- Click **Start** or send `Hi Hermes`.
- The bot will respond automatically!

### 2. Accessing the Hermes Web Dashboard UI
- Open your Space URL in the browser (e.g. `https://YOUR_HF_USERNAME-huggingmes.hf.space`).
- Enter your **`GATEWAY_TOKEN`** password to unlock.
- Here you can manage model settings, view live sessions, install skills, and monitor memory.

---

## 📂 Local Project Structure (`d:\app\HuggingMes`)

- **`Dockerfile`**: Defines the Linux OS container, Chromium browser (for web browsing), and Python environment.
- **`start.sh`**: Startup orchestration script that restores memory, launches Node health server, starts Telegram gateway, and runs sync loop.
- **`hermes-sync.py`**: Background thread that backs up chat history and memory state to your HF Dataset every 10 mins.
- **`cloudflare-keepalive-setup.py`**: Deploys a Cloudflare Worker to ping the space every 5 minutes (keeps Space 24/7 awake).
- **`health-server.js` & `env-builder.html`**: Web Dashboard proxy & password protection server.

---

## ❓ Frequently Asked Questions & Troubleshooting

### Q1: Why does Hugging Face say "This Space has been paused"?
- **Cause:** Free Hugging Face Spaces pause after inactivity.
- **Fix:** Cloudflare Keep-Alive token prevents this. If duplicating a paused space, use `?duplicate=true` in the URL bar to bypass the missing button.

### Q2: Will `d:\app\HuggingMes` interfere with the Toolza app?
- **No:** `HuggingMes` is completely isolated in `d:\app\HuggingMes` outside of `Toolza`, ensuring zero conflicts with React/React Native builds.

---
*Created on July 28, 2026.*
