# SDHSH — San Diego Home Sweet Home

Personal real estate website for **Nada Benny**, a San Diego-based real estate agent.

- **Brokerage:** Realty Executives Dillon
- **CA DRE#:** 01938540
- **Phone:** (619) 987-5080
- **Email:** nada.benny@gmail.com
- **Office:** 2240 Otay Lakes Rd, Chula Vista, CA 91915
- **Live site:** sandiegohomesweethome.com

## About

This site replaces a previous Luxury Presence-hosted website. It is a single-file HTML site (all CSS and JS inline, no build step) designed to be fast, portable, and easy to maintain. Live on **Vercel** with automatic redeploys from the GitHub repo on every push.

## Tech Stack

- Single `index.html` — zero dependencies, no build tools, no framework
- **Fonts:** Bodoni Moda (headings), Roboto Flex (body) via Google Fonts
- **Hosting:** Vercel + GitHub integration (`codaeddie/sdhsh`)
- **Forms:** Client-side submission with inline success messages
- **Design tokens:** `#E7191F` (red), `#004475` (navy), `#1A1A1A` (black), `#EEEDE9` (beige)

## Site Sections

- **Hero** — Full-viewport aerial drone video (mp4/webm), dark overlay, tagline, CTA, scroll indicator, and mute/unmute toggle
- **Header** — Fixed nav that transitions from transparent over the hero to solid white on scroll; hamburger + slide-out side menu on mobile
- **Gallery Grid** — 3-tile grid: featured 1728 Bancroft St listing (links to standalone property site), "Sell my home fast!" CTA, "Happy Home Hunting!" CTA
- **Agent Bio** — Two-column layout with headshot, full bio, inline stats (12+ years experience, 100+ families helped), license info, and social links (LinkedIn, Instagram, YouTube, Facebook)
- **Newsletter Signup** — Dark-background name/email capture form
- **Neighborhoods** — Tabbed image slider for 6 areas: El Cajon, San Carlos, Spring Valley, La Mesa, San Diego, Santee
- **Home Valuation** — Navy-overlay lead capture form (address, name, phone, email, selling timeframe)
- **Testimonials** — Paginated two-up testimonial cards over a navy background
- **CTA Banner** — "Let's Chat!" call-to-action with contact modal trigger and phone number
- **Contact Modal** — Full-screen popup with agent info + contact form (name, email, phone, interest, message)
- **Footer** — Contact info, nav links, social icons, brokerage/DRE disclaimer, Equal Housing Opportunity logo
- **Mobile CTA Bar** — Sticky bottom bar with call button and "Let's Connect" (small screens only)

## Project Structure

```
sdhsh/
├── index.html          ← entire site (HTML + CSS + JS, all inline)
├── README.md
└── assets/
    ├── images/
    │   ├── logo.jpg
    │   ├── footer-logo.jpg
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
    │   ├── eho-logo.png
    │   └── realty-executives-logo.jpg
    └── video/
        ├── hero-aerial.mp4
        └── hero-aerial.webm
```

## Deploy

Push to `main` and Vercel auto-redeploys. No build step, no CI config.

```bash
git add -A && git commit -m "describe changes" && git push
```

## Socials

- [Facebook](https://www.facebook.com/BennySellsSanDiego/)
- [Instagram](https://www.instagram.com/ask_nerda/)
- [LinkedIn](https://www.linkedin.com/in/nada-benny/)
- [YouTube](https://www.youtube.com/@Ask_Nerda)

## Author

Built by [codaeddie](https://github.com/codaeddie) for Nada Benny.

---

## Change Log

### 2026-02-12 — Move stats into bio section, update experience numbers

**What changed:**
- Removed the standalone 3-column Stats section (`#stats`) entirely
- Moved the stats inline into the Agent Bio section as compact red-accent figures displayed below the bio copy, above the license
- Updated years of experience from "6+" to **"12+"** (calculated from license date 09/17/2013)
- Updated families helped from "47+" to **"100+"**
- Removed the 5-star reviews stat (decorative stars with no real info)
- Cleaned up related CSS: replaced `.stats-section` / `.stats-row` / `.stat-card` / `.stat-value` / `.stat-label` styles with new `.agent-stats` / `.agent-stat` / `.agent-stat-value` / `.agent-stat-label` styles scoped to the bio
- Removed stats responsive overrides from the 768px breakpoint
- Updated top-of-file HTML comment to reflect removed Stats section
- Deleted `SETUP.md` (deployment is complete — Vercel, DNS, and GitHub Pages migration all done)
- Rewrote `README.md` to reflect the finished, live state of the site
