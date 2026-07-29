# Velocity Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a rich, self-contained landing page for Velocity at `shoogarsoft.com/apps/velocity/`, sourcing and optimizing marketing assets from `code/velocity/`, wiring it into the homepage portfolio grid and sitemap, per the approved design at `docs/superpowers/specs/2026-07-28-velocity-landing-page-design.md`.

**Architecture:** Static HTML site, no build step, no test framework. `apps/velocity/index.html` is a self-contained page with its own `<style>` block (same pattern as the existing `partner-deck.html`) — it does not use or modify the shared `assets/style.css`, except for reusing the existing literal nav/footer markup (which already renders on a dark bar, so it reads consistently against this page's dark theme). "Tests" are grep-based assertions, `node -e` JSON-LD validation, and a manual browser check — there is no app logic to unit test.

**Tech Stack:** Static HTML/CSS (no framework), Python + Pillow (already available) for one-time image optimization, Node.js for JSON-LD validation.

## Global Constraints

- `code/velocity/` is a **read-only asset source** — no file in that repo is ever created, modified, or deleted by this plan.
- Optimized assets land in `code/shoogar/assets/` with a `velocity-` filename prefix, matching the existing convention (`scourr-showcase.mp4`, `rsp-bizowner-poster.jpg`).
- Copy tone is terse, present-tense, "confident-shipped" (per the spec's creative-direction section): no future-tense language ("planned", "roadmap", "coming soon", "early access"), no genre-filler adjectives ("adrenaline-fueled", "immersive", "stunning", "epic"), no neon/synthwave word-salad in copy, no comparisons to other titles. Use the exact headline/subhead/CTA copy given in Task 2 below — do not paraphrase it.
- The CTA (`https://shoogar.itch.io/velocity-midnight-run`, `target="_blank" rel="noopener"`) appears at least 3 times on the page (hero, after the car roster, footer).
- Canonical URL is extensionless: `https://shoogarsoft.com/apps/velocity/` (matches the Cloudflare Pages redirect behavior confirmed during the prior SEO restructure — trailing slash, no `.html`).
- LF line endings (repo-wide `.gitattributes` convention) — no CRLF.
- Do not modify `assets/style.css`.

---

### Task 1: Asset preparation

**Files:**
- Create: `code/shoogar/assets/velocity-hero.jpg`
- Create: `code/shoogar/assets/velocity-01-title.jpg`
- Create: `code/shoogar/assets/velocity-04-street-run.jpg`
- Create: `code/shoogar/assets/velocity-05-near-miss.jpg`
- Create: `code/shoogar/assets/velocity-06-racer-encounter.jpg`
- Create: `code/shoogar/assets/velocity-09-high-speed-chase.jpg`
- Create: `code/shoogar/assets/velocity-10-cop-pursuit.jpg`
- Create: `code/shoogar/assets/velocity-sweep-poster.jpg`
- Create: `code/shoogar/assets/velocity-sweep.mp4`

**Interfaces:**
- Consumes: source files under `code/velocity/assets/` and `code/velocity/marketing-screenshots/v1.1/` (read-only — do not modify or delete anything in `code/velocity/`).
- Produces: 9 files in `code/shoogar/assets/` that Task 2 references by exact filename.

- [ ] **Step 1: Run the image optimization script**

From the repo root (`d:/Development/Web/research-server`), run:

```bash
python -c "
from PIL import Image
import os

src = 'code/velocity'
dst = 'code/shoogar/assets'

im = Image.open(f'{src}/assets/banner-cropped.png').convert('RGB')
im.thumbnail((1600,1600))
im.save(f'{dst}/velocity-hero.jpg', 'JPEG', quality=78, optimize=True)
print('velocity-hero.jpg', im.size, os.path.getsize(f'{dst}/velocity-hero.jpg'))

shots = ['01-title','04-street-run','05-near-miss','06-racer-encounter','09-high-speed-chase','10-cop-pursuit']
for s in shots:
    im = Image.open(f'{src}/marketing-screenshots/v1.1/{s}.png').convert('RGB')
    im.thumbnail((1280,1280))
    out = f'{dst}/velocity-{s}.jpg'
    im.save(out, 'JPEG', quality=78, optimize=True)
    print(f'velocity-{s}.jpg', im.size, os.path.getsize(out))

poster = Image.open(f'{src}/marketing-screenshots/v1.1/04-street-run.png').convert('RGB')
poster.thumbnail((960,960))
poster.save(f'{dst}/velocity-sweep-poster.jpg', 'JPEG', quality=72, optimize=True)
print('velocity-sweep-poster.jpg', poster.size, os.path.getsize(f'{dst}/velocity-sweep-poster.jpg'))
"
```

Expected output (sizes may vary slightly by a few KB, that's fine):
```
velocity-hero.jpg (1600, 1292) ~170000
velocity-01-title.jpg (1280, 720) ~42000
velocity-04-street-run.jpg (1280, 720) ~58000
velocity-05-near-miss.jpg (1280, 720) ~67000
velocity-06-racer-encounter.jpg (1280, 720) ~69000
velocity-09-high-speed-chase.jpg (1280, 720) ~62000
velocity-10-cop-pursuit.jpg (1280, 720) ~59000
velocity-sweep-poster.jpg (960, 540) ~34000
```

- [ ] **Step 2: Copy the trailer video**

```bash
cp "code/velocity/marketing-screenshots/v1.1/00-autopilot-sweep.mp4" "code/shoogar/assets/velocity-sweep.mp4"
```

- [ ] **Step 3: Verify all 9 files exist and are meaningfully smaller than their PNG sources**

```bash
ls -la code/shoogar/assets/velocity-*.jpg code/shoogar/assets/velocity-sweep.mp4
```

Expected: 9 files present. Each `.jpg` should be well under 200KB (the PNG sources ranged 200KB-2.98MB). `velocity-sweep.mp4` should be ~9.4MB (a straight copy, no re-encoding).

- [ ] **Step 4: Confirm `code/velocity/` is untouched**

```bash
git -C code/velocity status
```

Expected: no changes (this is a separate git repo from `code/shoogar/` — confirm it reports clean, proving nothing there was modified).

- [ ] **Step 5: Commit**

```bash
git add assets/velocity-hero.jpg assets/velocity-01-title.jpg assets/velocity-04-street-run.jpg assets/velocity-05-near-miss.jpg assets/velocity-06-racer-encounter.jpg assets/velocity-09-high-speed-chase.jpg assets/velocity-10-cop-pursuit.jpg assets/velocity-sweep-poster.jpg assets/velocity-sweep.mp4
git commit -m "feat(shoogar): add optimized Velocity marketing assets"
```

(Run this from `code/shoogar/` — paths above are relative to that repo root.)

---

### Task 2: Velocity landing page

**Files:**
- Create: `code/shoogar/apps/velocity/index.html`

**Interfaces:**
- Consumes: the 9 asset files from Task 1 (`../../assets/velocity-*.jpg`, `../../assets/velocity-sweep.mp4`), plus the existing `../../assets/shoogar-logo.png` and `../../assets/style.css?v=2` (only for the shared nav/footer classes — this page's own sections use a separate `<style>` block).
- Produces: the URL `https://shoogarsoft.com/apps/velocity/` that Task 3 links to from the homepage and adds to the sitemap.

- [ ] **Step 1: Write the file**

Create `code/shoogar/apps/velocity/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="VELOCITY — Midnight Endless Run. Free browser-based endless arcade street racer. Weave night-city traffic, thread near-misses, outrun the cops. Play free right now.">
  <link rel="canonical" href="https://shoogarsoft.com/apps/velocity/">
  <title>VELOCITY — Midnight Endless Run | Shoogar Soft</title>
  <link rel="icon" type="image/png" href="../../assets/shoogar-logo.png">
  <link rel="stylesheet" href="../../assets/style.css?v=2">

  <meta property="og:type" content="website">
  <meta property="og:title" content="VELOCITY — Midnight Endless Run">
  <meta property="og:description" content="One crash ends it. Everything before that is style. Free browser-based endless arcade street racer.">
  <meta property="og:url" content="https://shoogarsoft.com/apps/velocity/">
  <meta property="og:image" content="https://shoogarsoft.com/assets/velocity-hero.jpg">
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="VELOCITY — Midnight Endless Run">
  <meta name="twitter:description" content="One crash ends it. Everything before that is style. Free browser-based endless arcade street racer.">
  <meta name="twitter:image" content="https://shoogarsoft.com/assets/velocity-hero.jpg">

  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "VideoGame",
    "name": "Velocity — Midnight Endless Run",
    "description": "An endless night-city street run. Weave traffic, thread near-misses, outrun the cops. One hard crash and it's over.",
    "genre": ["Racing", "Arcade"],
    "gamePlatform": "Web Browser",
    "applicationCategory": "Game",
    "url": "https://shoogar.itch.io/velocity-midnight-run",
    "publisher": {
      "@type": "Organization",
      "name": "Shoogar Soft Inc.",
      "url": "https://shoogarsoft.com"
    }
  }
  </script>

  <style>
    .v-page {
      background: #05060f;
      color: #e8ecf5;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      line-height: 1.6;
    }
    .v-page a { color: #29e0ff; }
    .v-section { max-width: 960px; margin: 0 auto; padding: 64px 24px; }
    .v-hero { text-align: center; padding: 72px 24px; background: linear-gradient(180deg, rgba(5,6,15,0.4), #05060f 85%), url('../../assets/velocity-hero.jpg') center/cover no-repeat; }
    .v-eyebrow { font-size: 13px; letter-spacing: 2px; text-transform: uppercase; color: #ff2d78; font-weight: 700; margin-bottom: 16px; }
    .v-hero h1 { font-size: clamp(1.8rem, 5vw, 3rem); line-height: 1.15; margin-bottom: 20px; max-width: 18ch; margin-left: auto; margin-right: auto; }
    .v-hero-sub { font-size: 1.05rem; color: #b7bfd6; max-width: 52ch; margin: 0 auto 32px; }
    .v-cta { display: inline-block; background: #ff2d78; color: #ffffff; font-weight: 700; text-decoration: none; padding: 14px 32px; border-radius: 8px; font-size: 1.05rem; }
    .v-cta:hover { background: #ff5590; }
    .v-cta-note { margin-top: 14px; font-size: 0.85rem; color: #8891a8; }
    .v-trailer-wrap { max-width: 900px; margin: 0 auto; padding: 48px 24px 0; text-align: center; }
    .v-trailer video { width: 100%; border-radius: 12px; display: block; }
    .v-caption { margin-top: 12px; font-size: 0.9rem; color: #8891a8; }
    .v-how h2 { font-size: 1.6rem; text-align: center; margin-bottom: 40px; max-width: 26ch; margin-left: auto; margin-right: auto; }
    .v-how-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 32px; align-items: center; }
    .v-how-beats { display: grid; gap: 20px; }
    .v-how-beat b { color: #29e0ff; }
    .v-how-beats p { color: #b7bfd6; }
    .v-how-shot { width: 100%; border-radius: 10px; }
    @media (max-width: 720px) { .v-how-grid { grid-template-columns: 1fr; } }
    .v-roster h2 { font-size: 1.6rem; text-align: center; margin-bottom: 32px; }
    .v-roster-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; }
    @media (max-width: 720px) { .v-roster-grid { grid-template-columns: 1fr; } }
    .v-car-card { background: #0d0f1a; border: 1px solid #1c2033; border-radius: 12px; padding: 24px; }
    .v-car-card h3 { font-size: 1.15rem; margin-bottom: 4px; }
    .v-car-tag { color: #ff2d78; font-weight: 700; font-size: 0.95rem; margin-bottom: 12px; }
    .v-car-card p { color: #b7bfd6; font-size: 0.92rem; }
    .v-cta2-wrap { text-align: center; padding: 0 24px 8px; }
    .v-gallery h2 { font-size: 1.6rem; text-align: center; margin-bottom: 32px; }
    .v-gallery-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
    @media (max-width: 720px) { .v-gallery-grid { grid-template-columns: repeat(2, 1fr); } }
    .v-shot { margin: 0; }
    .v-shot img { width: 100%; border-radius: 8px; display: block; }
    .v-shot figcaption { font-size: 0.82rem; color: #8891a8; margin-top: 6px; }
    .v-outro { text-align: center; padding: 64px 24px 80px; }
    .v-outro-line { color: #8891a8; font-size: 0.92rem; max-width: 48ch; margin: 0 auto 32px; }
    .v-outro-back { margin-top: 20px; font-size: 0.9rem; }
    .v-outro-back a { color: #b7bfd6; }
  </style>
</head>
<body class="v-page">

<!-- NAV -->
<nav>
  <a class="nav-logo" href="../../index.html">
    <img src="../../assets/shoogar-logo.png" alt="Shoogar Soft logo">
    Shoogar Soft
  </a>
  <ul class="nav-links" id="navLinks">
    <li><a href="../../index.html#work">Work</a></li>
    <li><a href="../../index.html#about">About</a></li>
    <li><a href="../../index.html#contact">Contact</a></li>
  </ul>
  <button class="nav-toggle" onclick="document.getElementById('navLinks').classList.toggle('open')" aria-label="Menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<!-- HERO -->
<section class="v-hero">
  <div class="v-eyebrow">VELOCITY — Midnight Endless Run</div>
  <h1>One crash ends it. Everything before that is style.</h1>
  <p class="v-hero-sub">An endless night-city street run. Weave traffic, thread near-misses, outrun the cops. One hard crash and it's over — in slow motion.</p>
  <a class="v-cta" href="https://shoogar.itch.io/velocity-midnight-run" target="_blank" rel="noopener">Start the run</a>
  <p class="v-cta-note">No download. No sign-up. ~10 seconds to first corner.</p>
</section>

<!-- TRAILER -->
<div class="v-trailer-wrap">
  <div class="v-trailer">
    <video autoplay muted loop playsinline controls preload="metadata" poster="../../assets/velocity-sweep-poster.jpg">
      <source src="../../assets/velocity-sweep.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
  </div>
  <p class="v-caption">Real-time gameplay. Runs in your browser.</p>
</div>

<!-- HOW IT PLAYS -->
<section class="v-section v-how">
  <h2>Scrapes cost you speed. Crashes cost you everything.</h2>
  <div class="v-how-grid">
    <div class="v-how-beats">
      <div class="v-how-beat"><b>Survive the traffic.</b><p>Highways, streets, and alleys, back to back — live traffic ahead, every run.</p></div>
      <div class="v-how-beat"><b>Thread the near-misses.</b><p>Close calls score. The tighter the gap, the bigger the payout.</p></div>
      <div class="v-how-beat"><b>One crash ends it.</b><p>Hard contact ends the run in slow motion. Everything up to that point is your score.</p></div>
    </div>
    <img class="v-how-shot" src="../../assets/velocity-05-near-miss.jpg" alt="Near-miss scoring popup during a night-city street run in Velocity">
  </div>
</section>

<!-- CAR ROSTER -->
<section class="v-section v-roster">
  <h2>Three cars. One life.</h2>
  <div class="v-roster-grid">
    <div class="v-car-card">
      <h3>Kaze GT-R</h3>
      <div class="v-car-tag">Corners on rails</div>
      <p>FWD hot hatch. Built for threading tight gaps at speed.</p>
    </div>
    <div class="v-car-card">
      <h3>Rampage 440</h3>
      <div class="v-car-tag">Straight-line violence</div>
      <p>RWD muscle. Power-oversteer, straight-line brutality.</p>
    </div>
    <div class="v-car-card">
      <h3>Tempest RS</h3>
      <div class="v-car-tag">Grips everything</div>
      <p>AWD rally turbo. Planted through anything you throw at it.</p>
    </div>
  </div>
</section>

<div class="v-cta2-wrap">
  <a class="v-cta" href="https://shoogar.itch.io/velocity-midnight-run" target="_blank" rel="noopener">Start the run</a>
</div>

<!-- GALLERY -->
<section class="v-section v-gallery">
  <h2>Night city, every route</h2>
  <div class="v-gallery-grid">
    <figure class="v-shot">
      <img src="../../assets/velocity-01-title.jpg" alt="Velocity title screen with car select">
      <figcaption>Title &amp; car select</figcaption>
    </figure>
    <figure class="v-shot">
      <img src="../../assets/velocity-04-street-run.jpg" alt="Street-level chase-cam view weaving through night-city traffic">
      <figcaption>Street traffic weave</figcaption>
    </figure>
    <figure class="v-shot">
      <img src="../../assets/velocity-06-racer-encounter.jpg" alt="Rival racer encounter on a night-city highway">
      <figcaption>Rival racer challenge</figcaption>
    </figure>
    <figure class="v-shot">
      <img src="../../assets/velocity-09-high-speed-chase.jpg" alt="High-speed hood-cam view at top speed">
      <figcaption>Top-speed hood cam</figcaption>
    </figure>
    <figure class="v-shot">
      <img src="../../assets/velocity-10-cop-pursuit.jpg" alt="Cop car pursuit through night-city streets">
      <figcaption>Cop pursuit AI</figcaption>
    </figure>
  </div>
</section>

<!-- OUTRO -->
<section class="v-outro">
  <p class="v-outro-line">Every mesh, texture, and sound is generated in code — no asset files.</p>
  <a class="v-cta" href="https://shoogar.itch.io/velocity-midnight-run" target="_blank" rel="noopener">Start the run</a>
  <p class="v-outro-back">Built by <a href="../../index.html">Shoogar Soft Inc.</a>, an independent software studio based in Canada.</p>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <span class="footer-copy">© <span id="copy-year"></span> Shoogar Soft Inc.</span>
    <div class="footer-links">
      <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>
      <a href="../../privacy.html">Privacy Policy</a>
      <a href="../../terms.html">Terms of Service</a>
    </div>
  </div>
</footer>

<script>document.getElementById('copy-year').textContent = new Date().getFullYear();</script>
</body>
</html>
```

Note: the footer's Privacy/Terms links use the `.html` form, matching every other page's footer on this site (`href="privacy.html"` etc.). The prior SEO restructure's fix round deliberately changed only canonical tags and sitemap `<loc>` entries to the extensionless form — it left every `href=` link attribute untouched sitewide (Cloudflare 308-redirects the `.html` form, so the links still work, just with one redirect hop). This page follows that same established convention rather than introducing a new one.

- [ ] **Step 2: Verify the JSON-LD block is valid JSON**

Run (from `code/shoogar/`):
```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('apps/velocity/index.html','utf8'); const m=html.match(/<script type=\"application\/ld\+json\">([\s\S]*?)<\/script>/); JSON.parse(m[1]); console.log('valid JSON-LD')"
```
Expected: prints `valid JSON-LD` with no error.

- [ ] **Step 3: Verify required tags, CTA count, and heading are present**

Run:
```bash
grep -c 'rel="canonical"' apps/velocity/index.html
grep -c 'name="description"' apps/velocity/index.html
grep -c '<h1>' apps/velocity/index.html
grep -c 'shoogar.itch.io/velocity-midnight-run' apps/velocity/index.html
```
Expected: canonical `1`, description `1`, h1 `1`, itch.io link count `4` (hero, roster-repeat, outro, plus the JSON-LD `url` field).

- [ ] **Step 4: Verify no future-tense/banned copy patterns crept in**

```bash
grep -iE 'coming soon|roadmap|early access|adrenaline|immersive|stunning' apps/velocity/index.html
```
Expected: no output (no matches) — confirms the Global Constraints copy rules were followed.

- [ ] **Step 5: Commit**

```bash
git add apps/velocity/index.html
git commit -m "feat(shoogar): add Velocity landing page"
```

---

### Task 3: Homepage portfolio card + sitemap entry

**Files:**
- Modify: `code/shoogar/index.html`
- Modify: `code/shoogar/sitemap.xml`

**Interfaces:**
- Consumes: `https://shoogarsoft.com/apps/velocity/` (produced by Task 2).
- Produces: nothing consumed by later tasks.

- [ ] **Step 1: Add a Velocity portfolio card**

In `code/shoogar/index.html`, in the `.portfolio-grid` div, add a new `.portfolio-card` after the Chroma Slider card (before the closing `</div>` of `.portfolio-grid`):

```html
    <div class="portfolio-card">
      <div class="portfolio-icon" aria-hidden="true">🏎️</div>
      <h3>Velocity</h3>
      <p>Free browser-based endless arcade street racer. Night-city traffic, near-miss scoring, cop chases.</p>
      <div class="portfolio-status live">Now live</div>
      <a class="portfolio-link" href="apps/velocity/">Play Velocity →</a>
    </div>
```

- [ ] **Step 2: Add the sitemap entry**

In `code/shoogar/sitemap.xml`, add a new `<url>` entry (after the `apps/chroma-slider/` entry, before its closing `</urlset>`):

```xml
  <url>
    <loc>https://shoogarsoft.com/apps/velocity/</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
```

- [ ] **Step 3: Verify**

Run (from `code/shoogar/`):
```bash
grep -c 'Velocity' index.html
node -e "const fs=require('fs'); const s=fs.readFileSync('sitemap.xml','utf8'); const opens=(s.match(/<url>/g)||[]).length; const closes=(s.match(/<\/url>/g)||[]).length; if(opens!==closes) throw new Error('mismatched tags'); console.log(opens+' url entries, well-formed')"
grep -c 'apps/velocity/</loc>' sitemap.xml
```
Expected: "Velocity" appears at least once in `index.html`; sitemap prints `6 url entries, well-formed`; velocity `<loc>` count `1`.

- [ ] **Step 4: Commit**

```bash
git add index.html sitemap.xml
git commit -m "feat(shoogar): add Velocity portfolio card and sitemap entry"
```

---

### Task 4: Full verification sweep

**Files:**
- None (verification only, no file changes).

**Interfaces:**
- Consumes: everything produced by Tasks 1-3.
- Produces: nothing (terminal task).

- [ ] **Step 1: Full-repo grep + JSON-LD sweep**

Run (from `code/shoogar/`):
```bash
for f in index.html privacy.html terms.html partner-deck.html apps/chroma-slider/index.html apps/chroma-slider/privacy.html apps/velocity/index.html; do
  echo "== $f =="
  grep -c 'rel="canonical"' "$f"
  grep -c 'name="description"' "$f"
done

node -e "
const fs=require('fs');
['index.html','apps/chroma-slider/index.html','apps/velocity/index.html'].forEach(f=>{
  const html=fs.readFileSync(f,'utf8');
  const m=html.match(/<script type=\"application\/ld\+json\">([\s\S]*?)<\/script>/);
  JSON.parse(m[1]);
  console.log(f+': valid JSON-LD');
});
"
```
Expected: every `.html` file prints `1` / `1`; all three JSON-LD blocks print `valid JSON-LD`.

- [ ] **Step 2: Manual spot-check (not automatable)**

Serve the site locally (e.g. `python -m http.server 8931` from `code/shoogar/`) and open `apps/velocity/index.html` in a browser. Confirm:
- The hero renders with the banner background and correct headline/subhead/CTA text.
- The trailer video plays (muted, looping) and has working controls.
- The "How it plays" section shows the near-miss screenshot correctly.
- All 3 car cards render.
- The gallery shows all 5 remaining screenshots.
- The itch.io link (`https://shoogar.itch.io/velocity-midnight-run`) opens correctly from at least one of its 3 on-page occurrences.
- The new homepage portfolio card renders and links to the Velocity page.
- No visual breakage in the shared nav/footer (still styled correctly against the page's dark background).

This step has no pass/fail command — it's a visual confirmation before considering the plan done.

## Post-Plan Note

Validate the `VideoGame` JSON-LD with Google's Rich Results Test after deploy (requires a live URL, outside what local verification can confirm). No off-site checklist entry was requested for Velocity/itch.io this round — can be added to `docs/seo-offsite-checklist.md` later if wanted.
