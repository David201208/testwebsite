# IP Inspector — Deployment Guide

A research tool that displays the public IP address and connection metadata of any device that visits the page.

---

## What It Shows

- **Public IP address** (IPv4 or IPv6 detected automatically)
- ISP / Organisation, ASN
- Country, Region, City, Timezone, Lat/Long
- Browser User-Agent, Screen Resolution, Language
- Connection type (where browser exposes it)
- Full raw JSON payload

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The complete single-page app |
| `netlify.toml` | Netlify build & header config |
| `README.md` | This guide |

---

## Step 1 — Get a Free API Token (optional but recommended)

1. Go to [https://ipapi.co](https://ipapi.co) and sign up for a free account.
2. Copy your **API key** from the dashboard.
3. Free tier: 1,000 requests/day. No token = 30 req/min, shared limit.

---

## Step 2 — Push to GitHub

```bash
# In your project folder
git init
git add .
git commit -m "Initial commit: IP Inspector"

# Create a new repo on github.com then:
git remote add origin https://github.com/YOUR_USERNAME/ip-inspector.git
git branch -M main
git push -u origin main
```

---

## Step 3 — Deploy on Netlify

### Option A: via Netlify UI (easiest)

1. Go to [https://app.netlify.com](https://app.netlify.com) → **Add new site** → **Import from Git**
2. Connect GitHub and select your `ip-inspector` repo
3. Build settings:
   - **Build command:** *(leave blank)*
   - **Publish directory:** `.`
4. Click **Deploy site**

### Option B: via Netlify CLI

```bash
npm install -g netlify-cli
netlify login
netlify init       # link to your site
netlify deploy --prod
```

---

## Step 4 — Add Environment Variable

1. In Netlify dashboard → your site → **Site configuration** → **Environment variables**
2. Click **Add a variable**
3. Set:
   - **Key:** `IPAPI_TOKEN`
   - **Value:** *(your ipapi.co API key)*
4. Click **Save**
5. Go to **Deploys** → **Trigger deploy** → **Deploy site** to rebuild with the token injected

> **Why?** The token is injected at build time via `netlify.toml`, so it never appears as plain text in your source code or git history.

---

## Step 5 — Test Desktop vs Mobile

1. Open the deployed Netlify URL on your **desktop** — note the IP shown.
2. Open the same URL on your **phone** (on mobile data, not WiFi) — you'll see the phone's IP.
3. If phone is on the same WiFi as desktop, both will show the same router IP (that's normal — it's the public egress IP).
4. Switch phone to **mobile data** to see a different IP.

---

## Notes

- The `@netlify/plugin-replace` build plugin handles the `__IPAPI_TOKEN__` replacement. If you'd rather skip it, just hardcode your token directly in `index.html` (fine for private/personal use, not for public repos).
- All data is fetched client-side — nothing is logged or stored server-side.
- CORS: `ipapi.co` supports browser requests natively.
