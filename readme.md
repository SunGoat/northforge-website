# Northforge Site — Deploy Package

Complete production-ready static site. Drop the entire folder onto Netlify (or any static host) and it works.

---

## What's in this package

```
northforge-v4/
├── index.html              Home
├── capabilities.html       Capabilities
├── accreditation.html      How We Work (The Forge Method™)
├── m365-e7.html            M365 E7 Readiness
├── partnering.html         Partnering with primes
├── about.html              About Northforge
├── engage.html             Contact (with Netlify form)
├── _shared.css             Single shared stylesheet
├── netlify.toml            Netlify config (redirects, headers, caching)
├── _redirects              Belt-and-braces redirect file
├── robots.txt              SEO
├── sitemap.xml             SEO
└── assets/
    ├── ng-mark.png         NG monogram (256px, transparent)
    ├── ng-mark-large.png   NG monogram (1024px, transparent)
    ├── ng-mark.svg         NG monogram (vector)
    └── favicon/            All favicon sizes + .ico + apple-touch
```

**Total deployed size: ~185 KB.** Previously each page embedded the logo as base64 (~600 KB per page). Now everything is referenced from `/assets/` once and cached aggressively.

---

## How to deploy (Netlify)

### Option A — Drag and drop
1. Zip the `northforge-v4/` folder
2. Go to https://app.netlify.com/drop
3. Drag the zip onto the page
4. Done

### Option B — Git
1. Commit the `northforge-v4/` folder to a repo
2. In Netlify, connect the repo
3. Build settings: leave blank (no build step), publish directory = `northforge-v4/` (or `.` if at root)
4. Deploy

### Custom domain (northforge.com.au)
1. In Netlify: Site settings → Domain management → Add custom domain
2. Add `northforge.com.au` and `www.northforge.com.au`
3. Update your DNS:
   - `A` record: `@` → Netlify's load balancer IP
   - `CNAME`: `www` → `<your-site>.netlify.app`
4. Wait for Netlify to provision the SSL cert (free, automatic)

---

## Form handling

The contact form on `/engage.html` uses **Netlify Forms** — no server required.

Form name: `northforge-enquiry`

To enable email notifications:
1. After deploy, go to Netlify → Forms
2. Find `northforge-enquiry`
3. Add notification → Email → enter `hello@northforge.com.au`

Submissions also visible in the Netlify dashboard. Spam protection is built in (honeypot + Akismet).

---

## Pretty URLs

Both formats work for every page:
- `/capabilities.html` ✓
- `/capabilities` ✓

Netlify handles this via `netlify.toml` and `_redirects`. Old or shortened URLs also redirect:
- `/contact` → `/engage.html`
- `/how-we-work` → `/accreditation.html`
- `/forge-method` → `/accreditation.html`

---

## Asset cache

Assets in `/assets/` are cached for 1 year (immutable). The shared CSS is cached for 24 hours. HTML pages are not cached aggressively, so content edits show up immediately.

---

## Editing content

All content lives directly in the HTML files. To edit copy:
1. Open the HTML file
2. Find the section you want to change
3. Edit the text between the tags
4. Push to Netlify (or drag the file via the Netlify dashboard)

The shared CSS controls all visual styling — colours, typography, spacing. Edit `_shared.css` once and every page picks up the change.

---

## Adding a new page

1. Copy any existing page as a template (e.g. `about.html`)
2. Rename it (e.g. `services.html`)
3. Update the `<title>` and breadcrumb
4. Update the body content
5. To add it to the nav: edit the `<nav class="nf-nav">` and `<div class="mob-menu">` blocks on **every page** to include the new link. (Or rebuild from the Python build script in `/home/claude/` if you have that.)

---

## Brand integrity

This site uses the brand pack assets:
- **Forge Orange** `#E8660A` — single accent, used for FORGE in wordmark, CTAs, eyebrow `//` prefix
- **Bone White** `#F5F2ED` — primary text
- **Carbon Black** `#080808` — primary surface (not pure black)
- **Off-Black** `#0F0F0F` — alternating sections
- **Barlow Condensed** — display + body
- **Cormorant Garamond italic** — editorial pull quotes
- **DM Mono** — eyebrows, metadata, technical labels

If anything drifts from these values, refer back to the brand guidelines PDF in the `northforge-brand/` pack.

---

## Updating placeholders before launch

Before pointing the domain at this build, replace these placeholders:
- `hello@northforge.com.au` (in `engage.html`) — confirm or replace
- `accreditation@northforge.com.au` (in `engage.html`) — confirm or replace
- `partners@northforge.com.au` (in `engage.html`) — confirm or replace

If any of these email aliases don't exist yet, set them up in your email provider before launch so enquiries don't bounce.
