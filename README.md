<div align="center">

# Vestia

### *An editorial AI personal stylist.*

A mobile-first wardrobe app that composes outfits like a magazine composes a spread. Built with React, Claude, and Seedance.

[Live Demo](#) · [Setup](#setup) · [Stack](#stack) · [License](#license)

---

</div>

## What it is

Vestia photographs your wardrobe, reads the live weather where you are, and composes daily outfits with the voice of a fashion editor. Each look gets a mood word, color story, and stylist's notes. You can generate a 5-second cinematic video of yourself wearing the suggested outfit.

It looks and feels like a printed publication that became digital — not a tech app trying to look luxurious.

## Features

- **Editorial outfit composition** — Claude analyzes your wardrobe images and the live weather, then composes a look with mood, color story, and notes
- **Seven-day planning** — A full week of outfits, no piece worn more than twice
- **Cinematic video** — Seedance generates a vertical 5-second video of you in the suggested outfit
- **Real GPS weather** — Open-Meteo, no API key required
- **Local-first storage** — Wardrobe and history live in IndexedDB on your device. Nothing syncs.
- **PWA installable** — Add to home screen, runs like a native app
- **Smart compression** — Photos auto-resized to 1200px @ 85% quality before storage
- **Haptic feedback** — Tactile response on every interaction

## Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 + Vite |
| Styling | Hand-written CSS, Fraunces + Instrument Sans |
| Storage | IndexedDB (photos), localStorage fallback |
| AI — Style | [Anthropic Claude](https://www.anthropic.com) (Sonnet 4) |
| AI — Video | [ByteDance Seedance Lite](https://fal.ai/models/fal-ai/bytedance/seedance/v1/lite/image-to-video) via fal.ai |
| Weather | [Open-Meteo](https://open-meteo.com) (free, no key) |
| Hosting | [Netlify](https://netlify.com) (static + serverless functions) |
| Security | API keys held server-side via Netlify Functions |

## Setup

### Prerequisites

- Node.js 20+
- An [Anthropic API key](https://console.anthropic.com/settings/keys) ($5 free credit)
- A [fal.ai API key](https://fal.ai/dashboard/keys) (free credits)
- A [Netlify account](https://netlify.com) (free)
- A [GitHub account](https://github.com) (for git-based deploys)

### 1. Clone and install

```bash
git clone https://github.com/YOUR-USERNAME/vestia.git
cd vestia
npm install
```

### 2. Run locally

```bash
npm run dev
```

> **Note:** The Claude and Seedance proxies (`netlify/functions/`) only run when deployed to Netlify or via `netlify dev`. To test locally with functions:
> ```bash
> npm install -g netlify-cli
> echo "ANTHROPIC_API_KEY=sk-ant-..." > .env
> echo "FAL_KEY=..." >> .env
> netlify dev
> ```

### 3. Deploy to Netlify

**Option A — Git-based (recommended):**

1. Push this repo to GitHub
2. Go to [app.netlify.com/start](https://app.netlify.com/start) → "Import from Git" → select your repo
3. Click **Deploy** (settings auto-detect from `netlify.toml`)
4. Once deployed, go to **Site settings → Environment variables** and add:
   - `ANTHROPIC_API_KEY` — your Anthropic key
   - `FAL_KEY` — your fal.ai key
5. **Deploys → Trigger deploy → Deploy site**

**Option B — CLI:**

```bash
npm install -g netlify-cli
netlify login
netlify init
netlify env:set ANTHROPIC_API_KEY sk-ant-...
netlify env:set FAL_KEY ...
netlify deploy --prod
```

## Architecture

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│   React App     │────▶│  Netlify Function    │────▶│  Claude API     │
│   (browser)     │     │  /api/claude         │     │                 │
└─────────────────┘     └──────────────────────┘     └─────────────────┘
        │                                                       
        │                                                       
        │               ┌──────────────────────┐     ┌─────────────────┐
        └──────────────▶│  Netlify Function    │────▶│  fal.ai         │
        @fal-ai/client  │  /api/fal/proxy      │     │  (Seedance)     │
                        └──────────────────────┘     └─────────────────┘
```

The browser never sees your API keys. All sensitive requests are proxied through Netlify Functions where keys are stored as environment variables.

## Cost

| Action | Cost |
|---|---|
| Outfit suggestion | ~$0.005 (Claude) |
| AI video (5s @ 720p) | ~$0.18 (Seedance Lite) |
| Weather | $0 (Open-Meteo) |
| Hosting | $0 (Netlify free tier) |

A user generating 1 outfit + 1 video per day = ~$5–6/month per user.

## Project structure

```
vestia/
├── src/
│   ├── Vestia.jsx          # Main app component
│   ├── styles.css          # Editorial design system
│   └── main.jsx            # React entry point
├── netlify/
│   └── functions/
│       ├── fal-proxy.mjs   # Secure fal.ai proxy
│       └── claude.mjs      # Secure Claude proxy
├── public/
│   ├── favicon.svg
│   ├── manifest.json       # PWA manifest
│   ├── _headers            # Security headers
│   └── _redirects          # SPA routing
├── index.html
├── package.json
├── vite.config.js
├── netlify.toml
└── README.md
```

## Design philosophy

Vestia is built around the conviction that an outfit recommendation should *feel* like reading editorial copy — not like receiving an algorithm output. Every interface element follows three principles:

- **Restraint over decoration.** A single ochre accent. No purple gradients. Hairline rules instead of borders. Generous whitespace.
- **Type as architecture.** Fraunces (variable serif) for display and italics. Instrument Sans for utility text. Numbers set in tabular figures. Every label is uppercase with tracked letter-spacing.
- **Editorial structure.** Numbered sections (№ 01, № 02) like a magazine table of contents. Issue dates. Pull quotes. Dropped numerals on lists.

The result feels less like a tech product and more like a publication that happens to be interactive.

## Privacy

- ✅ Wardrobe photos stored only in your browser (IndexedDB)
- ✅ API keys live only on Netlify's servers (never sent to browser)
- ✅ Photos sent to Claude/Seedance for inference are not retained long-term by those providers
- ✅ No analytics, no tracking, no third-party scripts
- ✅ "Clear all data" button wipes everything instantly

## Troubleshooting

**"Generation failed" or "API error 500"**
→ Environment variables missing. Go to Netlify → Site settings → Environment variables, confirm `ANTHROPIC_API_KEY` and `FAL_KEY` are set, then redeploy.

**"Add profile photo first"**
→ Profile tab → tap the photo placeholder → upload a photo of yourself.

**Video takes longer than 2 minutes**
→ Normal during peak hours. Seedance can take 30s–3min depending on queue.

**Photos won't upload on mobile**
→ Make sure you're on the deployed Netlify URL (not a sandboxed iframe).

## License

MIT

## Acknowledgements

Built with [Claude](https://claude.ai), powered by [Anthropic](https://anthropic.com), [fal.ai](https://fal.ai), and [Open-Meteo](https://open-meteo.com).

Typefaces: [Fraunces](https://fonts.google.com/specimen/Fraunces) by Phaedra Charles, [Instrument Sans](https://fonts.google.com/specimen/Instrument+Sans) by Rodrigo Fuenzalida.

---

<div align="center">

*Made with restraint.*

</div>
