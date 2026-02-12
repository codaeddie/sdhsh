# sdhsh — Setup Guide

Starting point: repo is already pushed to GitHub, GitHub Pages is currently
serving as a demo. Goal is to move to Vercel (matching 1728Bancroft), then
shut down GitHub Pages and make the repo private.

---

## WHERE YOU ARE NOW

- [x] Repo on GitHub: `codaeddie/sdhsh`
- [x] GitHub Pages active (demo)
- [ ] Vercel — not connected yet
- [ ] Custom domain — not pointed yet

---

## 1. Import into Vercel

### A. Via Vercel Dashboard (same way you did 1728Bancroft)

1. Go to **https://vercel.com/dashboard**
2. Click **Add New... > Project**
3. Under "Import Git Repository", find **codaeddie/sdhsh**
4. Click **Import**
5. Configure:
   - **Framework Preset:** `Other` (it's a plain HTML site, not Next.js)
   - **Root Directory:** `./` (leave default)
   - **Build Command:** leave blank (no build needed)
   - **Output Directory:** `./` (the root, since `index.html` is at root)
6. Click **Deploy**

Vercel will give you an instant preview URL like:
```
https://sdhsh-XXXX.vercel.app
```

Verify the site loads and the video plays.

### B. Via CLI (alternative)

If you have the Vercel CLI installed (same as 1728Bancroft):

```bash
cd sdhsh
vercel
```

It will ask:
- **Set up and deploy?** → `Y`
- **Which scope?** → your account
- **Link to existing project?** → `N`
- **Project name?** → `sdhsh` (or whatever you want)
- **Directory with code?** → `./`
- **Override settings?** → `N`

Then deploy to production:
```bash
vercel --prod
```

---

## 2. Add Custom Domain in Vercel

1. Go to your project in Vercel: **https://vercel.com/codaeddie/sdhsh**
   (the project name may differ — check your dashboard)
2. Click **Settings** > **Domains**
3. Type `sandiegohomesweethome.com` and click **Add**
4. Vercel will show you the DNS records you need. It will ask you to choose:

### Option A: Recommended — Use Vercel nameservers (simplest)

Vercel may offer to manage DNS entirely. If you transfer nameservers:
- Go to your domain registrar
- Change nameservers to the ones Vercel provides (e.g., `ns1.vercel-dns.com`, `ns2.vercel-dns.com`)
- Vercel handles everything after that, including SSL

### Option B: Keep your registrar's DNS — Add records manually

If you prefer to keep DNS at your current registrar (GoDaddy, Namecheap,
Cloudflare, etc.), Vercel will tell you to add these:

| Type    | Name  | Value                              | TTL  |
|---------|-------|------------------------------------|------|
| A       | @     | `76.76.21.21`                      | 3600 |
| CNAME   | www   | `cname.vercel-dns.com`             | 3600 |

**IMPORTANT:** These are Vercel's IPs, not GitHub's. You are replacing the
old GitHub Pages records:

| DELETE these old GitHub records |                                |
|---------------------------------|--------------------------------|
| ~~A  @  185.199.108.153~~      | ← remove                      |
| ~~A  @  185.199.109.153~~      | ← remove                      |
| ~~A  @  185.199.110.153~~      | ← remove                      |
| ~~A  @  185.199.111.153~~      | ← remove                      |
| ~~CNAME www codaeddie.github.io~~ | ← remove                   |

| ADD these Vercel records        |                                |
|---------------------------------|--------------------------------|
| A  @  `76.76.21.21`            | ← add                         |
| CNAME  www  `cname.vercel-dns.com` | ← add                     |

5. Also add `www.sandiegohomesweethome.com` in Vercel Domains settings
   - Vercel will auto-configure it to redirect to the apex (`sandiegohomesweethome.com`)

6. Wait for DNS propagation (usually 5-30 minutes, can take up to 48 hours)

7. Vercel auto-provisions **SSL/HTTPS** — no checkbox needed, it just works.

### Verify:

```
https://sandiegohomesweethome.com      → site loads via Vercel
https://www.sandiegohomesweethome.com  → redirects to above
```

---

## 3. Disable GitHub Pages

Once Vercel is serving the domain correctly:

1. Go to **https://github.com/codaeddie/sdhsh/settings/pages**
2. Under **Source**, change to **None** (or click "Unpublish")
3. If GitHub created a `CNAME` file in your repo, delete it:

```bash
cd sdhsh
git rm CNAME 2>/dev/null
git commit -m "Remove GitHub Pages CNAME — site now on Vercel"
git push
```

The `codaeddie.github.io/sdhsh/` URL will stop working.
That's expected — Vercel is your host now.

---

## 4. Make the GitHub Repo Private

Once everything is live on Vercel and you no longer need the public demo:

1. Go to **https://github.com/codaeddie/sdhsh/settings**
2. Scroll all the way to the bottom — **Danger Zone**
3. Click **Change visibility**
4. Select **Make private**
5. Type the repo name to confirm

**This won't break Vercel.** Vercel has its own connection to your GitHub
account via the Vercel GitHub App. Private repos deploy to Vercel the same
as public ones — every `git push` still triggers a redeploy.

**What changes:**
- `codaeddie.github.io/sdhsh/` → 404 (already disabled above)
- Source code no longer visible to the public
- Vercel still deploys normally on every push

---

## 5. Wire Up Contact Forms

The 4 forms currently show a client-side "thank you" but don't send emails.

### Formspree (easiest — 50 submissions/mo free)

1. Go to **https://formspree.io** — sign up with nada.benny@gmail.com
2. Create a **new form** — name it "SDHSH Contact"
3. Copy your endpoint: `https://formspree.io/f/xABCDEFG`
4. In `index.html`, update each form:

```html
<!-- BEFORE -->
<form id="contact-form" onsubmit="handleFormSubmit(event, 'contact')">

<!-- AFTER -->
<form id="contact-form" action="https://formspree.io/f/YOUR_ID" method="POST">
```

5. Remove the `onsubmit="handleFormSubmit(...)"` from each form
6. You can use the same Formspree endpoint for all 4 forms, or create
   separate forms to organize submissions by type

### Forms to wire (4 total):
| Form ID             | Purpose                    |
|---------------------|----------------------------|
| `#newsletter-form`  | Newsletter signup           |
| `#valuation-form`   | Home valuation request      |
| `#contact-form`     | Main contact form           |
| `#modal-form`       | Modal popup contact form    |

After wiring, commit and push — Vercel auto-redeploys:
```bash
git add index.html
git commit -m "Wire forms to Formspree"
git push
```

---

## 6. Favicon

1. Go to **https://favicon.io/favicon-generator/**
2. Text: `NB`, Background: `#E7191F`, Font color: `#FFFFFF`
3. Download the zip, extract `favicon.ico` and `favicon-32x32.png`
4. Put them in the project root (`sdhsh/`)
5. Add to `<head>` in `index.html`:

```html
<link rel="icon" type="image/x-icon" href="favicon.ico">
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">
```

6. Commit + push.

---

## 7. Google Analytics (optional)

1. Go to **https://analytics.google.com**
2. Create a property for `sandiegohomesweethome.com`
3. Get your Measurement ID (`G-XXXXXXXXXX`)
4. Add before `</head>` in `index.html`:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

5. Commit + push.

---

## Quick Reference — File Structure

```
sdhsh/
├── index.html              ← entire site (HTML + CSS + JS inline)
├── .gitignore
├── SETUP.md                ← this file
└── assets/
    ├── images/
    │   ├── logo.jpg
    │   ├── nada-benny-headshot.jpg
    │   ├── hero-poster.jpg
    │   ├── hero-poster-alt.jpg
    │   ├── featured-1728-bancroft.jpg
    │   ├── sell-home.jpg
    │   ├── happy-home-hunting.jpg
    │   ├── neighborhood-el-cajon.jpg
    │   ├── neighborhood-san-carlos.jpg
    │   ├── neighborhood-spring-valley.jpg
    │   ├── neighborhood-la-mesa.jpg
    │   ├── neighborhood-san-diego.jpg
    │   ├── neighborhood-santee.jpg
    │   ├── bg-home-valuation.jpg
    │   ├── bg-testimonials.jpg
    │   ├── bg-cta.jpg
    │   ├── footer-logo.jpg
    │   ├── eho-logo.png
    │   └── realty-executives-logo.jpg
    └── video/
        ├── hero-aerial.mp4
        └── hero-aerial.webm
```

---

## Deploy Workflow (ongoing)

Every time you make changes:
```bash
git add -A
git commit -m "describe what changed"
git push
```
Vercel auto-redeploys in ~10 seconds. No build step, no CI config.
Same workflow as 1728Bancroft.

---

## Checklist

- [x] Repo pushed to GitHub
- [x] GitHub Pages demo active
- [ ] Import project into Vercel
- [ ] Verify Vercel preview URL works
- [ ] Add `sandiegohomesweethome.com` domain in Vercel settings
- [ ] Update DNS: delete GitHub A records, add Vercel A + CNAME
- [ ] Verify `https://sandiegohomesweethome.com` loads via Vercel
- [ ] Disable GitHub Pages (Settings > Pages > None)
- [ ] Delete `CNAME` file if GitHub created one
- [ ] Make repo private (Settings > Danger Zone > Change visibility)
- [ ] Wire forms to Formspree
- [ ] Add favicon
- [ ] Add Google Analytics (optional)
