# SEO Authority Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Harden `shoogarsoft.com`'s technical SEO and internal linking (structured data, meta tags, canonical URLs, a real Chroma Slider landing page, an expanded sitemap) and document off-site authority follow-ups, per the approved design at `docs/superpowers/specs/2026-07-28-seo-authority-restructure-design.md`.

**Architecture:** Static HTML site, no build step, no test framework. Every task edits or adds plain `.html`/`.xml`/`.md` files directly under `code/shoogar/`. "Tests" for this plan are grep-based content assertions and `node -e` JSON-LD validation — there is no app logic to unit test.

**Tech Stack:** Static HTML/CSS (no framework), existing `assets/style.css`, Node.js (already on PATH) used only as a throwaway JSON validator in verification steps.

## Global Constraints

- Do not modify `assets/style.css` or introduce new CSS classes — every new page must render using classes that already exist (`.legal`, `.legal h1`, `.legal h2`, `.legal p`, `.legal a`, `.legal-date`, nav/footer markup) per the approved "Minimal Hardening" design (no design/CSS work in scope).
- No store link for Chroma Slider is available yet — do not fabricate one. The landing page describes the app without a store CTA button.
- `partner-deck.html` keeps its existing `<meta name="robots" content="noindex, nofollow">` — it is an intentionally unindexed WIP page. It still gets a meta description and canonical tag per the design, but is **not** added to `sitemap.xml`.
- Blog/content-strategy work and a full multi-page homepage restructure are explicitly out of scope (deferred per the design doc).
- LF line endings (per repo-wide `.gitattributes` convention) — don't introduce CRLF.
- Every commit message follows the existing repo convention seen in `git log` (short imperative subject, e.g. `feat: ...`, `fix: ...`, `docs: ...`).

---

### Task 1: Chroma Slider landing page

**Files:**
- Create: `code/shoogar/apps/chroma-slider/index.html`

**Interfaces:**
- Consumes: existing CSS classes from `assets/style.css` (`.legal`, `.legal h1/h2/p/a`, `.legal-date`, nav/footer markup — same pattern as `code/shoogar/apps/chroma-slider/privacy.html`).
- Produces: a URL, `https://shoogarsoft.com/apps/chroma-slider/`, that Task 2 links to from the homepage portfolio card, and that Task 4 adds to `sitemap.xml`.

- [ ] **Step 1: Write the file**

Create `code/shoogar/apps/chroma-slider/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Chroma Slider is a free, ad-supported color puzzle game for mobile. Slide, match, and complete stages to unlock new color challenges — no account required.">
  <link rel="canonical" href="https://shoogarsoft.com/apps/chroma-slider/">
  <title>Chroma Slider — Color Puzzle Game | Shoogar Soft</title>
  <link rel="icon" type="image/png" href="../../assets/shoogar-logo.png">
  <link rel="stylesheet" href="../../assets/style.css?v=2">
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "SoftwareApplication",
    "name": "Chroma Slider",
    "applicationCategory": "GameApplication",
    "description": "A free, ad-supported color puzzle game for mobile. Slide, match, and complete stages to unlock new color challenges — no account required.",
    "url": "https://shoogarsoft.com/apps/chroma-slider/",
    "publisher": {
      "@type": "Organization",
      "name": "Shoogar Soft Inc.",
      "url": "https://shoogarsoft.com"
    }
  }
  </script>
</head>
<body>

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

<!-- CONTENT -->
<main class="legal">
  <h1>Chroma Slider</h1>
  <p class="legal-date">A color puzzle game for mobile, from Shoogar Soft Inc.</p>

  <p>Chroma Slider is a free, ad-supported color puzzle game. Slide, match, and complete stages to unlock new color challenges — no account required, no data collected beyond what's needed to serve ads.</p>

  <h2>What is Chroma Slider</h2>
  <p>Each stage presents a new color-matching puzzle. Slide pieces into place, clear the stage, and move on to the next challenge. Progress carries forward as you unlock stages and earn achievements.</p>

  <h2>Features</h2>
  <p>No account or sign-up required. Play offline. Achievements, stage completion, and settings are saved locally on your device — never sent to us.</p>

  <h2>Privacy &amp; ads</h2>
  <p>Chroma Slider shows ads served through Google AdMob to stay free to play. See the full <a href="privacy.html">Privacy Policy</a> for details on what AdMob collects.</p>

  <h2>From Shoogar Soft</h2>
  <p>Chroma Slider is built by <a href="../../index.html">Shoogar Soft Inc.</a>, an independent software studio based in Canada.</p>
</main>

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

- [ ] **Step 2: Verify the JSON-LD block is valid JSON**

Run (from `code/shoogar/`):
```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('apps/chroma-slider/index.html','utf8'); const m=html.match(/<script type=\"application\/ld\+json\">([\s\S]*?)<\/script>/); JSON.parse(m[1]); console.log('valid JSON-LD')"
```
Expected: prints `valid JSON-LD` with no error.

- [ ] **Step 3: Verify required tags are present**

Run:
```bash
grep -c 'rel="canonical"' apps/chroma-slider/index.html
grep -c 'name="description"' apps/chroma-slider/index.html
grep -c '<h1>' apps/chroma-slider/index.html
```
Expected: each command prints `1` (exactly one canonical tag, one meta description, one `<h1>`).

- [ ] **Step 4: Commit**

```bash
git add apps/chroma-slider/index.html
git commit -m "feat(shoogar): add Chroma Slider landing page"
```

---

### Task 2: Homepage — portfolio card, JSON-LD, canonical, Open Graph/Twitter tags

**Files:**
- Modify: `code/shoogar/index.html`

**Interfaces:**
- Consumes: `https://shoogarsoft.com/apps/chroma-slider/` (produced by Task 1).
- Produces: `Organization` JSON-LD block on the homepage that Task 3's pages don't duplicate (each subpage stays lightweight — no repeated Organization schema).

- [ ] **Step 1: Add canonical tag, Open Graph and Twitter Card tags, and Organization JSON-LD to `<head>`**

In `code/shoogar/index.html`, replace the current `<head>` block (lines 3–10):

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Shoogar Soft Inc. — Independent software studio based in Canada. Building focused, opinionated tools for real problems.">
  <link rel="canonical" href="https://shoogarsoft.com/">
  <title>Shoogar Soft — Building software the right way.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
  <link rel="stylesheet" href="assets/style.css?v=2">

  <meta property="og:type" content="website">
  <meta property="og:title" content="Shoogar Soft — Building software the right way.">
  <meta property="og:description" content="Independent software studio based in Canada. Building focused, opinionated tools for real problems.">
  <meta property="og:url" content="https://shoogarsoft.com/">
  <meta property="og:image" content="https://shoogarsoft.com/assets/shoogar-logo.png">
  <meta name="twitter:card" content="summary">
  <meta name="twitter:title" content="Shoogar Soft — Building software the right way.">
  <meta name="twitter:description" content="Independent software studio based in Canada. Building focused, opinionated tools for real problems.">
  <meta name="twitter:image" content="https://shoogarsoft.com/assets/shoogar-logo.png">

  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "Shoogar Soft Inc.",
    "url": "https://shoogarsoft.com",
    "logo": "https://shoogarsoft.com/assets/shoogar-logo.png",
    "sameAs": [
      "https://scourr.app",
      "https://rsp.shoogarsoft.com"
    ]
  }
  </script>
</head>
```

- [ ] **Step 2: Add a Chroma Slider portfolio card**

In `code/shoogar/index.html`, in the `.portfolio-grid` div (currently lines 62–93), add a new `.portfolio-card` after the "Research Server Patterns" card (before the closing `</div>` of `.portfolio-grid`):

```html
    <div class="portfolio-card">
      <div class="portfolio-icon" aria-hidden="true">🎨</div>
      <h3>Chroma Slider</h3>
      <p>Free, ad-supported color puzzle game for mobile. Slide, match, and complete stages to unlock new challenges.</p>
      <div class="portfolio-status live">Now live</div>
      <a class="portfolio-link" href="apps/chroma-slider/">Play Chroma Slider →</a>
    </div>
```

- [ ] **Step 3: Verify the JSON-LD block is valid JSON**

Run (from `code/shoogar/`):
```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('index.html','utf8'); const m=html.match(/<script type=\"application\/ld\+json\">([\s\S]*?)<\/script>/); JSON.parse(m[1]); console.log('valid JSON-LD')"
```
Expected: prints `valid JSON-LD` with no error.

- [ ] **Step 4: Verify required tags and the new card are present**

Run:
```bash
grep -c 'rel="canonical"' index.html
grep -c 'property="og:title"' index.html
grep -c 'Chroma Slider' index.html
```
Expected: canonical count `1`, og:title count `1`, "Chroma Slider" appears at least once (the new card).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat(shoogar): add homepage structured data, OG tags, and Chroma Slider card"
```

---

### Task 3: Meta description + canonical on remaining pages

**Files:**
- Modify: `code/shoogar/privacy.html`
- Modify: `code/shoogar/terms.html`
- Modify: `code/shoogar/partner-deck.html`
- Modify: `code/shoogar/apps/chroma-slider/privacy.html`

**Interfaces:**
- Consumes: nothing from earlier tasks.
- Produces: nothing consumed by later tasks (Task 4 only needs the URLs, which already exist).

- [ ] **Step 1: `privacy.html` — add meta description and canonical**

In `code/shoogar/privacy.html`, replace lines 3–8:

```html
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Privacy Policy for Shoogar Soft Inc. and shoogarsoft.com. We do not collect personal data through this site.">
  <link rel="canonical" href="https://shoogarsoft.com/privacy.html">
  <title>Privacy Policy — Shoogar Soft Inc.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
```

- [ ] **Step 2: `terms.html` — add meta description and canonical**

In `code/shoogar/terms.html`, replace lines 3–8:

```html
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Terms of Service for Shoogar Soft Inc. and shoogarsoft.com.">
  <link rel="canonical" href="https://shoogarsoft.com/terms.html">
  <title>Terms of Service — Shoogar Soft Inc.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
```

- [ ] **Step 3: `partner-deck.html` — add meta description and canonical (keep noindex)**

In `code/shoogar/partner-deck.html`, replace lines 4–7:

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="robots" content="noindex, nofollow">
<meta name="description" content="Partnership proposal draft for Shoogar Soft Inc. — work in progress, not for public indexing.">
<link rel="canonical" href="https://shoogarsoft.com/partner-deck.html">
<title>Shoogar Soft — Partner With Us (WIP)</title>
```

- [ ] **Step 4: `apps/chroma-slider/privacy.html` — add meta description and canonical**

In `code/shoogar/apps/chroma-slider/privacy.html`, replace lines 3–8:

```html
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Privacy Policy for the Chroma Slider mobile app by Shoogar Soft Inc. — no account required, no personal data collected beyond ad serving.">
  <link rel="canonical" href="https://shoogarsoft.com/apps/chroma-slider/privacy.html">
  <title>Chroma Slider — Privacy Policy — Shoogar Soft Inc.</title>
  <link rel="icon" type="image/png" href="../../assets/shoogar-logo.png">
```

- [ ] **Step 5: Verify every page now has exactly one canonical tag and one meta description**

Run (from `code/shoogar/`):
```bash
for f in privacy.html terms.html partner-deck.html apps/chroma-slider/privacy.html; do
  echo "== $f =="
  grep -c 'rel="canonical"' "$f"
  grep -c 'name="description"' "$f"
done
```
Expected: every file prints `1` and `1`.

- [ ] **Step 6: Commit**

```bash
git add privacy.html terms.html partner-deck.html apps/chroma-slider/privacy.html
git commit -m "feat(shoogar): add meta descriptions and canonical tags to remaining pages"
```

---

### Task 4: Expand and refresh sitemap.xml

**Files:**
- Modify: `code/shoogar/sitemap.xml`

**Interfaces:**
- Consumes: `https://shoogarsoft.com/apps/chroma-slider/` (produced by Task 1). All other URLs already existed in the sitemap.
- Produces: nothing consumed by later tasks.

- [ ] **Step 1: Rewrite `sitemap.xml`**

Replace the full contents of `code/shoogar/sitemap.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://shoogarsoft.com/</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://shoogarsoft.com/privacy.html</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://shoogarsoft.com/terms.html</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://shoogarsoft.com/apps/chroma-slider/</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
  <url>
    <loc>https://shoogarsoft.com/apps/chroma-slider/privacy.html</loc>
    <lastmod>2026-07-28</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
</urlset>
```

Note: `partner-deck.html` is intentionally **not** added — it carries `noindex, nofollow` and should not appear in the sitemap.

- [ ] **Step 2: Verify the XML is well-formed**

Run (from `code/shoogar/`):
```bash
node -e "const fs=require('fs'); const s=fs.readFileSync('sitemap.xml','utf8'); if(!s.includes('<urlset')||!s.includes('</urlset>')) throw new Error('malformed'); const opens=(s.match(/<url>/g)||[]).length; const closes=(s.match(/<\/url>/g)||[]).length; if(opens!==closes) throw new Error('mismatched <url> tags'); console.log(opens+' url entries, well-formed')"
```
Expected: prints `5 url entries, well-formed`.

- [ ] **Step 3: Verify the new landing page URL is present and partner-deck.html is absent**

Run:
```bash
grep -c 'apps/chroma-slider/</loc>' sitemap.xml
grep -c 'partner-deck' sitemap.xml
```
Expected: first command prints `1`, second prints `0`.

- [ ] **Step 4: Commit**

```bash
git add sitemap.xml
git commit -m "feat(shoogar): expand sitemap with Chroma Slider landing page"
```

---

### Task 5: Off-site authority checklist + full-repo verification

**Files:**
- Create: `code/shoogar/docs/seo-offsite-checklist.md`

**Interfaces:**
- Consumes: nothing (documentation only).
- Produces: nothing (terminal task).

- [ ] **Step 1: Write the checklist**

Create `code/shoogar/docs/seo-offsite-checklist.md`:

```markdown
# SEO Off-Site Authority Checklist

Tracks the off-site (section 3) items from `docs/superpowers/specs/2026-07-28-seo-authority-restructure-design.md`. These are manual/one-time actions outside this repo — check them off as they're done, don't remove rows.

- [ ] Verify `rsp.shoogarsoft.com` links back to `shoogarsoft.com` from its own footer/about. If missing, file the fix in that codebase (out of scope here).
- [ ] Verify `scourr.app` links back to `shoogarsoft.com` from its own footer/about. If missing, file the fix in that codebase (out of scope here).
- [ ] Submit `shoogarsoft.com` to relevant directories: Product Hunt, AlternativeTo, other SaaS/software directories.
- [ ] Submit Chroma Slider to app-relevant directories/stores once a public store listing URL exists (none available as of 2026-07-28 — see plan Task 1 note).
- [ ] Set up a separate Google Search Console property for `scourr.app` — it is a distinct root domain, **not** covered by the existing `shoogarsoft.com` domain property (verified 2026-06-17, see RS memory `shoogarsoft-gsc-setup-2026-06-17`).
- [ ] Resubmit the expanded `sitemap.xml` to Google Search Console after this work deploys.
```

- [ ] **Step 2: Commit**

```bash
git add docs/seo-offsite-checklist.md
git commit -m "docs(shoogar): add SEO off-site authority checklist"
```

- [ ] **Step 3: Full-repo verification sweep**

Run (from `code/shoogar/`) to confirm every HTML page has exactly one canonical tag and one meta description, and that all JSON-LD blocks across the site parse:

```bash
for f in index.html privacy.html terms.html partner-deck.html apps/chroma-slider/index.html apps/chroma-slider/privacy.html; do
  echo "== $f =="
  grep -c 'rel="canonical"' "$f"
  grep -c 'name="description"' "$f"
done

node -e "
const fs=require('fs');
['index.html','apps/chroma-slider/index.html'].forEach(f=>{
  const html=fs.readFileSync(f,'utf8');
  const m=html.match(/<script type=\"application\/ld\+json\">([\s\S]*?)<\/script>/);
  JSON.parse(m[1]);
  console.log(f+': valid JSON-LD');
});
"
```
Expected: every `.html` file prints `1` / `1`; both JSON-LD blocks print `valid JSON-LD` with no thrown error.

- [ ] **Step 4: Manual spot-check (not automatable)**

Open `index.html` and `apps/chroma-slider/index.html` in a browser (e.g. `file://` path or local static server) and confirm:
- Pages render without visual breakage (no new CSS was added, so this should match existing site styling).
- Nav links, the new portfolio card, and the Chroma Slider page's internal links (to `privacy.html` and back to `../../index.html`) all resolve.

This step has no pass/fail command — it's a visual confirmation before considering the plan done.

- [ ] **Step 5: Confirm `robots.txt` doesn't block `/apps/`**

`robots.txt` is Cloudflare-managed, not a file in this repo (per design doc section 2). Check the live file:
```bash
curl -s https://shoogarsoft.com/robots.txt
```
Expected: no `Disallow: /apps/` (or equivalent) line. If one exists, it needs to be removed in Cloudflare, not in this repo — flag it, don't attempt to fix it here.

---

## Post-Plan Note

After all 5 tasks are committed, validate JSON-LD with Google's Rich Results Test and resubmit `sitemap.xml` to Search Console (tracked in Task 5's checklist) — both require a live deploy and are outside what this plan's local verification can confirm.
