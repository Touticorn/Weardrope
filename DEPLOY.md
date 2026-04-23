# Deploy to GitHub & Netlify — Step by Step

This is the complete guide from zero to live URL. Takes about 15 minutes.

---

## Part 1 · Push to GitHub (5 min)

### 1. Create a GitHub account

If you don't have one: https://github.com/signup

### 2. Install Git (if you don't have it)

- **Mac:** Open Terminal, type `git --version`. If not installed, run `xcode-select --install`
- **Windows:** Download from https://git-scm.com/download/win
- **Linux:** `sudo apt install git`

### 3. Create a new repository on GitHub

1. Go to https://github.com/new
2. Repository name: `vestia`
3. Description: `An editorial AI personal stylist`
4. Choose **Public** or **Private**
5. **Do NOT** check "Add a README" — we already have one
6. Click **Create repository**

You'll land on a page with setup commands. Keep it open.

### 4. Push the code

Open a terminal **inside the `vestia` folder** and run these commands one at a time:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/vestia.git
git push -u origin main
```

> Replace `YOUR-USERNAME` with your actual GitHub username.

When prompted for your password, use a **Personal Access Token** (not your GitHub password):
1. Go to https://github.com/settings/tokens
2. **Generate new token (classic)**
3. Name it "vestia", check **`repo`** scope
4. Generate and copy the token — paste it as your password

### 5. Verify

Refresh `https://github.com/YOUR-USERNAME/vestia` — you should see all the files.

---

## Part 2 · Deploy to Netlify (5 min)

### 1. Sign up for Netlify

Go to https://app.netlify.com/signup — sign up with your GitHub account (one click).

### 2. Import the repository

1. Click **Add new site → Import an existing project**
2. Choose **GitHub**
3. Authorize Netlify (one-time)
4. Find and click **vestia** in the list

### 3. Configure build settings

Netlify auto-reads `netlify.toml` — you don't need to change anything. Just click:

**Deploy vestia**

Wait ~1 minute. You'll get a live URL like `https://lyrical-dragon-abc123.netlify.app`.

---

## Part 3 · Add API keys (3 min)

The site is live but the AI features won't work yet. Add your keys:

### 1. Get the keys

- **Anthropic Claude:** https://console.anthropic.com/settings/keys
  - Sign up (free, $5 credit)
  - Create a key, copy it (starts with `sk-ant-`)

- **fal.ai:** https://fal.ai/dashboard/keys
  - Sign up with Google or GitHub
  - Create a key, copy it

### 2. Add them to Netlify

1. In your Netlify dashboard, click your site
2. **Site configuration → Environment variables → Add a variable**
3. Add the first one:
   - Key: `ANTHROPIC_API_KEY`
   - Value: paste your `sk-ant-...` key
   - Click **Create variable**
4. Click **Add a variable** again:
   - Key: `FAL_KEY`
   - Value: paste your fal.ai key
   - Click **Create variable**

### 3. Trigger a redeploy

1. Click **Deploys** in the left menu
2. **Trigger deploy → Deploy site**
3. Wait ~1 minute

---

## Part 4 · Use the app (2 min)

1. Open your Netlify URL on your **phone**
2. Go through the onboarding
3. Tap **Wardrobe** → upload a few clothing photos
4. Tap **Today** → **Compose Today's Look**
5. Add your profile photo in **Profile** tab
6. Back on Today → scroll down → **Generate Video**

To install as a PWA:
- **iOS:** Safari → Share → Add to Home Screen
- **Android:** Chrome → menu → Add to Home Screen

---

## Updating your site

Any time you want to push changes:

```bash
git add .
git commit -m "what you changed"
git push
```

Netlify auto-redeploys within ~30 seconds.

---

## Troubleshooting

**"Generation failed" / "API error 500"**
→ Environment variables missing or wrong. Re-check Netlify → Site configuration → Environment variables. Both `ANTHROPIC_API_KEY` and `FAL_KEY` must be set. Trigger a redeploy after.

**Git push asks for password and rejects it**
→ Use a Personal Access Token (see step 4 above). GitHub stopped accepting passwords in 2021.

**"Permission denied" when running git**
→ On Mac, you may need `sudo`. Or use GitHub Desktop instead: https://desktop.github.com

**Want a custom domain like `vestia.com`?**
→ Netlify → Domain management → Add custom domain. They'll walk you through DNS.

---

That's it. You now have a deployed AI fashion app on your own URL.
