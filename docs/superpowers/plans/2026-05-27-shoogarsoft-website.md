# shoogarsoft.com Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a static 3-page corporate website for Shoogar Soft Inc. at shoogarsoft.com.

**Architecture:** Plain HTML + CSS, no framework, no build step. One shared stylesheet covers all three pages. The homepage uses anchor-based single-page navigation; legal pages are separate files.

**Tech Stack:** HTML5, CSS3 (custom properties, CSS Grid, Flexbox). No JavaScript except the mobile nav toggle (5 lines, inline). No dependencies.

---

## File Structure

```
shoogar/
├── index.html          — Homepage (hero, featured, portfolio, about, footer)
├── privacy.html        — Privacy Policy
├── terms.html          — Terms of Service
├── assets/
│   ├── style.css       — All shared styles (tokens, nav, footer, components)
│   └── shoogar-logo.png — Pixel art cube logo (copied from victoria assets)
├── .gitignore
└── docs/
    └── superpowers/
        ├── specs/2026-05-27-shoogarsoft-website-design.md
        └── plans/2026-05-27-shoogarsoft-website.md  ← this file
```

---

## Task 1: Scaffold + Logo Asset

**Files:**
- Create: `assets/` directory
- Create: `.gitignore`
- Copy: `assets/shoogar-logo.png` from victoria

- [ ] **Step 1: Create assets directory and .gitignore**

```bash
mkdir assets
```

Create `.gitignore`:
```
.superpowers/
.DS_Store
Thumbs.db
```

- [ ] **Step 2: Copy Shoogar logo**

```bash
cp "../victoria/godot/assets/shoogar games.png" assets/shoogar-logo.png
```

- [ ] **Step 3: Verify logo copied**

```bash
ls -la assets/
```
Expected: `shoogar-logo.png` present, size ~1MB.

- [ ] **Step 4: Commit scaffold**

```bash
git init
git add .gitignore assets/shoogar-logo.png
git commit -m "chore: scaffold shoogarsoft.com project with logo asset"
```

---

## Task 2: Shared Stylesheet (assets/style.css)

**Files:**
- Create: `assets/style.css`

This stylesheet is linked by all three pages. It defines every visual token as a CSS custom property, then implements all layout components.

- [ ] **Step 1: Create assets/style.css with the complete stylesheet**

```css
/* ============================================================
   TOKENS
   ============================================================ */
:root {
  --bg:              #fffbf5;
  --nav-bg:          #1c1410;
  --nav-bg-hover:    #2c2418;
  --accent:          #c97d4a;
  --accent-light:    #e8c99a;
  --text:            #1c1410;
  --text-secondary:  #8a6a50;
  --text-muted:      #a08060;
  --border:          #e8d5c0;
  --featured-bg:     #fff8f0;
  --featured-border: #e8c99a;
  --card-bg:         #ffffff;
  --about-bg:        #1c1410;
  --footer-bg:       #130e09;
  --footer-text:     #5a4030;
  --max-width:       720px;
  --radius:          6px;
}

/* ============================================================
   RESET
   ============================================================ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'Georgia', serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}

/* ============================================================
   NAV
   ============================================================ */
nav {
  background: var(--nav-bg);
  padding: 0 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  position: sticky;
  top: 0;
  z-index: 10;
}

.nav-logo {
  display: flex;
  align-items: center;
  gap: 10px;
  color: var(--accent-light);
  font-weight: 700;
  font-size: 14px;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-decoration: none;
}

.nav-logo img {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  image-rendering: pixelated;
}

.nav-links {
  display: flex;
  gap: 24px;
  list-style: none;
}

.nav-links a {
  color: var(--text-muted);
  text-decoration: none;
  font-size: 13px;
  font-family: system-ui, sans-serif;
  transition: color 0.2s;
}

.nav-links a:hover { color: var(--accent-light); }

.nav-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  flex-direction: column;
  gap: 5px;
  padding: 4px;
}

.nav-toggle span {
  display: block;
  width: 22px;
  height: 2px;
  background: var(--text-muted);
  border-radius: 2px;
}

@media (max-width: 600px) {
  .nav-links { display: none; }
  .nav-toggle { display: flex; }
  .nav-links.open {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 56px;
    left: 0;
    right: 0;
    background: var(--nav-bg);
    padding: 16px 24px 20px;
    border-top: 1px solid #2c2018;
    gap: 16px;
  }
}

/* ============================================================
   HERO
   ============================================================ */
.hero {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 72px 24px 56px;
}

.hero-eyebrow {
  font-family: system-ui, sans-serif;
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 16px;
}

.hero h1 {
  font-size: clamp(36px, 6vw, 58px);
  font-weight: 700;
  line-height: 1.15;
  color: var(--text);
  margin-bottom: 20px;
}

.hero h1 em {
  color: var(--accent);
  font-style: normal;
}

.hero-sub {
  font-family: system-ui, sans-serif;
  font-size: 16px;
  color: var(--text-secondary);
  max-width: 480px;
}

/* ============================================================
   DIVIDER
   ============================================================ */
.divider {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 24px;
}

.divider hr {
  border: none;
  border-top: 1px solid var(--border);
}

/* ============================================================
   SECTION LABEL (shared)
   ============================================================ */
.section-label {
  font-family: system-ui, sans-serif;
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 24px;
}

/* ============================================================
   FEATURED PRODUCT
   ============================================================ */
.featured {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 56px 24px;
}

.featured-card {
  background: var(--featured-bg);
  border: 1px solid var(--featured-border);
  border-radius: var(--radius);
  padding: 36px;
  display: flex;
  gap: 36px;
  align-items: flex-start;
}

.featured-badge {
  background: var(--accent);
  color: #fff;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
  padding: 4px 10px;
  border-radius: 20px;
  display: inline-block;
  margin-bottom: 16px;
}

.featured-card h2 {
  font-size: 32px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 12px;
}

.featured-card p {
  font-family: system-ui, sans-serif;
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.7;
  margin-bottom: 24px;
  max-width: 380px;
}

.featured-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: var(--nav-bg);
  color: var(--accent-light);
  font-family: system-ui, sans-serif;
  font-size: 13px;
  font-weight: 600;
  padding: 10px 20px;
  border-radius: 4px;
  text-decoration: none;
  transition: background 0.2s;
}

.featured-link:hover { background: var(--nav-bg-hover); }

.featured-visual {
  flex-shrink: 0;
  width: 160px;
  height: 120px;
  background: linear-gradient(135deg, #c97d4a22, #e8c99a33);
  border-radius: var(--radius);
  border: 1px solid var(--featured-border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  color: var(--accent);
}

@media (max-width: 600px) {
  .featured-card {
    flex-direction: column;
    padding: 24px;
    gap: 20px;
  }
  .featured-visual { display: none; }
}

/* ============================================================
   PORTFOLIO GRID
   ============================================================ */
.portfolio {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 24px 64px;
}

.portfolio-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

@media (max-width: 600px) {
  .portfolio-grid { grid-template-columns: 1fr; }
}

.portfolio-card {
  background: var(--card-bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 20px;
  transition: border-color 0.2s;
}

.portfolio-card:hover { border-color: var(--accent); }

.portfolio-icon {
  font-size: 24px;
  margin-bottom: 10px;
}

.portfolio-card h3 {
  font-size: 16px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 6px;
}

.portfolio-card p {
  font-family: system-ui, sans-serif;
  font-size: 12px;
  color: var(--text-secondary);
  line-height: 1.5;
  margin-bottom: 12px;
}

.portfolio-status {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #c97d4a66;
}

/* ============================================================
   ABOUT SECTION
   ============================================================ */
.about {
  background: var(--about-bg);
  padding: 56px 24px;
}

.about-inner {
  max-width: var(--max-width);
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 48px;
  align-items: center;
}

@media (max-width: 600px) {
  .about-inner { grid-template-columns: 1fr; gap: 24px; }
}

.about-label {
  font-family: system-ui, sans-serif;
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 12px;
}

.about h2 {
  font-size: 28px;
  font-weight: 700;
  color: var(--accent-light);
  line-height: 1.3;
}

.about-body {
  font-family: system-ui, sans-serif;
  font-size: 15px;
  color: var(--text-muted);
  line-height: 1.8;
}

.about-body strong { color: var(--accent-light); }
.about-body a { color: var(--accent-light); }
.about-body a:hover { color: var(--accent); }

/* ============================================================
   FOOTER
   ============================================================ */
footer {
  background: var(--footer-bg);
  padding: 28px 24px;
}

.footer-inner {
  max-width: var(--max-width);
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
}

.footer-copy {
  font-family: system-ui, sans-serif;
  font-size: 12px;
  color: var(--footer-text);
}

.footer-links {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.footer-links a {
  font-family: system-ui, sans-serif;
  font-size: 12px;
  color: var(--footer-text);
  text-decoration: none;
  transition: color 0.2s;
}

.footer-links a:hover { color: var(--text-muted); }

/* ============================================================
   LEGAL PAGES
   ============================================================ */
.legal {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 56px 24px 80px;
}

.legal h1 {
  font-size: 36px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 8px;
}

.legal .legal-date {
  font-family: system-ui, sans-serif;
  font-size: 13px;
  color: var(--text-secondary);
  margin-bottom: 40px;
}

.legal h2 {
  font-size: 20px;
  font-weight: 700;
  color: var(--text);
  margin: 36px 0 12px;
}

.legal p {
  font-family: system-ui, sans-serif;
  font-size: 15px;
  color: var(--text-secondary);
  line-height: 1.8;
  margin-bottom: 16px;
}

.legal a {
  color: var(--accent);
  text-decoration: none;
}

.legal a:hover { text-decoration: underline; }
```

- [ ] **Step 2: Verify file created**

```bash
ls -la assets/style.css
```
Expected: file present, size ~4–6 KB.

- [ ] **Step 3: Commit stylesheet**

```bash
git add assets/style.css
git commit -m "feat: add shared stylesheet with warm/indie design tokens"
```

---

## Task 3: Homepage (index.html)

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Shoogar Soft Inc. — Independent software studio based in Canada. Building focused, opinionated tools for real problems.">
  <title>Shoogar Soft — Building software the right way.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
  <link rel="stylesheet" href="assets/style.css">
</head>
<body>

<!-- NAV -->
<nav>
  <a class="nav-logo" href="#">
    <img src="assets/shoogar-logo.png" alt="Shoogar Soft logo">
    Shoogar Soft
  </a>
  <ul class="nav-links" id="navLinks">
    <li><a href="#work">Work</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#about">Contact</a></li>
  </ul>
  <button class="nav-toggle" onclick="document.getElementById('navLinks').classList.toggle('open')" aria-label="Menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-eyebrow">Shoogar Soft Inc. · Canada</div>
  <h1>Building software<br>the <em>right</em> way.</h1>
  <p class="hero-sub">Independent software studio. Small team, focused scope, real problems.</p>
</section>

<div class="divider"><hr></div>

<!-- FEATURED -->
<section class="featured" id="work">
  <div class="section-label">✦ Featured — live now</div>
  <div class="featured-card">
    <div>
      <div class="featured-badge">Now live</div>
      <h2>Scourr</h2>
      <p>AI-powered job hunt assistant. Stop applying manually — Scourr automates your job search pipeline, tracks every application, and helps you land interviews faster.</p>
      <a class="featured-link" href="https://scourr.app" target="_blank" rel="noopener">Visit scourr.app →</a>
    </div>
    <div class="featured-visual" aria-hidden="true">🎯</div>
  </div>
</section>

<!-- PORTFOLIO -->
<section class="portfolio">
  <div class="section-label">Also building</div>
  <div class="portfolio-grid">

    <div class="portfolio-card">
      <div class="portfolio-icon" aria-hidden="true">🖥️</div>
      <h3>Pocket IT</h3>
      <p>MSP automation for IT teams. Remote management, monitoring, and client reporting without the bloat.</p>
      <div class="portfolio-status">In development</div>
    </div>

    <div class="portfolio-card">
      <div class="portfolio-icon" aria-hidden="true">🏷️</div>
      <h3>Trift Flipper</h3>
      <p>Smart assistant for resellers and thrift flippers. Price lookup, profit tracking, and listing tools.</p>
      <div class="portfolio-status">In development</div>
    </div>

    <div class="portfolio-card">
      <div class="portfolio-icon" aria-hidden="true">🪞</div>
      <h3>Mirrrd</h3>
      <p>Social content automation for creators. Schedule, repurpose, and amplify across platforms.</p>
      <div class="portfolio-status">Validation phase</div>
    </div>

  </div>
</section>

<!-- ABOUT -->
<section class="about" id="about">
  <div class="about-inner">
    <div>
      <div class="about-label">About</div>
      <h2>Small team.<br>Real software.</h2>
    </div>
    <div class="about-body">
      <strong>Shoogar Soft</strong> is an independent software company based in Canada. We build focused, opinionated tools for real problems — no bloat, no committees, just software that works.<br><br>
      Want to get in touch? <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <span class="footer-copy">© 2025 Shoogar Soft Inc.</span>
    <div class="footer-links">
      <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>
      <a href="privacy.html">Privacy Policy</a>
      <a href="terms.html">Terms of Service</a>
    </div>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

```bash
# Windows — open in default browser
start index.html
```

Check:
- Logo appears in nav (pixel cube)
- Hero headline renders in Georgia serif
- "right" is amber coloured
- Scourr featured card has amber border and badge
- Three portfolio cards in a row
- About section has dark background
- Footer shows email + legal links
- No horizontal scrollbar at full width

- [ ] **Step 3: Resize browser to 400px wide and verify mobile layout**

- Hamburger menu appears (☰ three lines)
- Click hamburger → nav links drop down
- Featured card stacks vertically
- Portfolio cards stack single-column
- About section stacks vertically

- [ ] **Step 4: Commit homepage**

```bash
git add index.html
git commit -m "feat: add homepage with featured Scourr and portfolio grid"
```

---

## Task 4: Privacy Policy (privacy.html)

**Files:**
- Create: `privacy.html`

- [ ] **Step 1: Create privacy.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Privacy Policy — Shoogar Soft Inc.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
  <link rel="stylesheet" href="assets/style.css">
</head>
<body>

<!-- NAV -->
<nav>
  <a class="nav-logo" href="index.html">
    <img src="assets/shoogar-logo.png" alt="Shoogar Soft logo">
    Shoogar Soft
  </a>
  <ul class="nav-links" id="navLinks">
    <li><a href="index.html#work">Work</a></li>
    <li><a href="index.html#about">About</a></li>
    <li><a href="index.html#about">Contact</a></li>
  </ul>
  <button class="nav-toggle" onclick="document.getElementById('navLinks').classList.toggle('open')" aria-label="Menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<!-- CONTENT -->
<main class="legal">
  <h1>Privacy Policy</h1>
  <p class="legal-date">Effective date: May 27, 2025</p>

  <h2>Overview</h2>
  <p>Shoogar Soft Inc. ("Shoogar Soft", "we", "us") operates the website shoogarsoft.com (the "Site"). This page informs you of our policies regarding the collection, use, and disclosure of personal information when you use our Site.</p>
  <p>We do not collect, store, or process any personal data through this Site.</p>

  <h2>No Data Collection</h2>
  <p>This Site is purely informational. We do not use cookies, analytics tools, tracking pixels, or any third-party data collection scripts. We do not collect your name, email address, IP address, or any other identifying information through the Site itself.</p>

  <h2>Contact via Email</h2>
  <p>If you choose to contact us at <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>, we will receive the information contained in your email (such as your name and email address). This information is used solely to respond to your inquiry and is not shared with third parties.</p>

  <h2>Third-Party Products</h2>
  <p>This Site links to our products (such as Scourr, Pocket IT, and others). Those products operate under their own privacy policies, which govern any data collected through those services. We encourage you to review the privacy policy of each product you use.</p>

  <h2>Changes to This Policy</h2>
  <p>We may update this Privacy Policy from time to time. Any changes will be posted on this page with an updated effective date.</p>

  <h2>Contact</h2>
  <p>If you have questions about this Privacy Policy, contact us at <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>.</p>

  <p>Shoogar Soft Inc. is incorporated in Canada.</p>
</main>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <span class="footer-copy">© 2025 Shoogar Soft Inc.</span>
    <div class="footer-links">
      <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>
      <a href="privacy.html">Privacy Policy</a>
      <a href="terms.html">Terms of Service</a>
    </div>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

```bash
start privacy.html
```

Check:
- Nav logo links back to `index.html`
- Prose text readable, system-ui font
- Amber accent on email links
- Footer matches homepage footer

- [ ] **Step 3: Commit privacy page**

```bash
git add privacy.html
git commit -m "feat: add privacy policy page"
```

---

## Task 5: Terms of Service (terms.html)

**Files:**
- Create: `terms.html`

- [ ] **Step 1: Create terms.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Terms of Service — Shoogar Soft Inc.</title>
  <link rel="icon" type="image/png" href="assets/shoogar-logo.png">
  <link rel="stylesheet" href="assets/style.css">
</head>
<body>

<!-- NAV -->
<nav>
  <a class="nav-logo" href="index.html">
    <img src="assets/shoogar-logo.png" alt="Shoogar Soft logo">
    Shoogar Soft
  </a>
  <ul class="nav-links" id="navLinks">
    <li><a href="index.html#work">Work</a></li>
    <li><a href="index.html#about">About</a></li>
    <li><a href="index.html#about">Contact</a></li>
  </ul>
  <button class="nav-toggle" onclick="document.getElementById('navLinks').classList.toggle('open')" aria-label="Menu">
    <span></span><span></span><span></span>
  </button>
</nav>

<!-- CONTENT -->
<main class="legal">
  <h1>Terms of Service</h1>
  <p class="legal-date">Effective date: May 27, 2025</p>

  <h2>Acceptance of Terms</h2>
  <p>By accessing shoogarsoft.com (the "Site"), you agree to be bound by these Terms of Service. If you do not agree to these terms, please do not use the Site.</p>

  <h2>Informational Purpose</h2>
  <p>This Site is provided for informational purposes only. The content describes Shoogar Soft Inc. and its products. Nothing on this Site constitutes a binding offer, warranty, or guarantee of any kind.</p>

  <h2>Intellectual Property</h2>
  <p>All content on this Site — including text, design, graphics, and the Shoogar Soft name and logo — is the property of Shoogar Soft Inc. and is protected under applicable Canadian and international intellectual property laws. You may not reproduce or distribute any content from this Site without written permission.</p>

  <h2>Third-Party Products and Links</h2>
  <p>This Site links to Shoogar Soft products (such as scourr.app) and may link to third-party websites. Those products and websites are governed by their own terms of service. Shoogar Soft Inc. is not responsible for the content or practices of any linked site.</p>

  <h2>Disclaimer of Warranties</h2>
  <p>This Site is provided "as is" without any warranties, express or implied. Shoogar Soft Inc. makes no representations about the accuracy, reliability, or completeness of any information on the Site.</p>

  <h2>Limitation of Liability</h2>
  <p>To the fullest extent permitted by applicable law, Shoogar Soft Inc. shall not be liable for any indirect, incidental, special, or consequential damages arising from your use of this Site.</p>

  <h2>Governing Law</h2>
  <p>These Terms of Service are governed by the laws of Canada. Any disputes shall be subject to the exclusive jurisdiction of the courts of Canada.</p>

  <h2>Changes to These Terms</h2>
  <p>We may update these Terms of Service from time to time. Any changes will be posted on this page with an updated effective date. Continued use of the Site after changes constitutes acceptance of the updated terms.</p>

  <h2>Contact</h2>
  <p>If you have questions about these Terms of Service, contact us at <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>.</p>
</main>

<!-- FOOTER -->
<footer>
  <div class="footer-inner">
    <span class="footer-copy">© 2025 Shoogar Soft Inc.</span>
    <div class="footer-links">
      <a href="mailto:corporate@shoogarsoft.com">corporate@shoogarsoft.com</a>
      <a href="privacy.html">Privacy Policy</a>
      <a href="terms.html">Terms of Service</a>
    </div>
  </div>
</footer>

</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

```bash
start terms.html
```

Check:
- Nav logo links back to `index.html`
- Prose readable, consistent with privacy page
- Footer matches

- [ ] **Step 3: Verify all footer links work across all pages**

Open `index.html` → click Privacy Policy → lands on `privacy.html` ✓
On `privacy.html` → click Terms of Service → lands on `terms.html` ✓
On `terms.html` → click Shoogar Soft logo → returns to `index.html` ✓

- [ ] **Step 4: Commit terms page**

```bash
git add terms.html
git commit -m "feat: add terms of service page"
```

---

## Task 6: Final Review + Deployment Prep

**Files:**
- None new — verification only

- [ ] **Step 1: Validate HTML for all three pages**

Open each file in Chrome, open DevTools (F12) → Console. No errors should appear.

Also check: right-click → View Page Source on each page and confirm:
- `charset="UTF-8"` present
- `viewport` meta present
- Correct `<title>` tag
- `alt` on logo image

- [ ] **Step 2: Check all internal links**

From `index.html`:
- Click "Work" → scrolls to featured section ✓
- Click "About" / "Contact" → scrolls to about section ✓
- Click "Privacy Policy" in footer → opens `privacy.html` ✓
- Click "Terms of Service" in footer → opens `terms.html` ✓
- Click "Visit scourr.app →" → opens scourr.app in new tab ✓

From `privacy.html` and `terms.html`:
- Click logo → returns to `index.html` ✓
- Nav links work ✓

- [ ] **Step 3: Final mobile check**

Open `index.html`, open DevTools → Toggle device toolbar (Ctrl+Shift+M) → select iPhone SE (375px width).

Verify:
- Hamburger toggle appears and works
- No text overflow
- Featured card stacked
- Portfolio cards single-column
- About section stacked

- [ ] **Step 4: Final commit**

```bash
git add -A
git status
git commit -m "chore: final review pass — all links verified, mobile confirmed"
```

- [ ] **Step 5: Confirm file structure is correct**

```bash
ls -la
ls -la assets/
```

Expected output:
```
index.html
privacy.html
terms.html
assets/
  shoogar-logo.png
  style.css
.gitignore
docs/
```

---

## Deployment Note

This is a static site — no server needed. To deploy to Cloudflare Pages:

1. Push this repo to GitHub
2. Log in to Cloudflare Dashboard → Pages → Create a project
3. Connect the GitHub repo
4. Build settings: none (static HTML, no build command)
5. Output directory: `/` (root)
6. Add custom domain: shoogarsoft.com → follow Cloudflare DNS instructions

HTTPS is automatic. Global CDN is included. Free tier supports this site indefinitely.
