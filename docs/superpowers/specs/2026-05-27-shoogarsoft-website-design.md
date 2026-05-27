# shoogarsoft.com — Website Design Spec

**Date:** 2026-05-27
**Status:** Approved

---

## Overview

A static corporate website for Shoogar Soft Inc. deployed at shoogarsoft.com. The site functions as a **credibility anchor** — a professional web presence for when prospects, partners, or investors Google the company. No aggressive calls-to-action. No personal identity (founder name is not disclosed).

---

## Positioning

**Founder brand, anonymous.** The brand voice is personal and opinionated ("building software the right way") without attributing it to an individual. The company is the face.

---

## Pages

| Page | Path | Purpose |
|------|------|---------|
| Homepage | `/` | Main credibility page |
| Privacy Policy | `/privacy` | Legal — data practices |
| Terms of Service | `/terms` | Legal — usage terms |

All three pages share the same nav and footer. No other pages at launch.

---

## Homepage Structure

### 1. Navigation (sticky, dark)
- Logo: Shoogar pixel cube + "SHOOGAR SOFT" wordmark
- Links: Work · About · Contact (all anchor links on the same page)

### 2. Hero
- Eyebrow: "Shoogar Soft Inc. · Canada"
- H1: "Building software the right way." (with "right" in amber accent)
- Subtitle: "Independent software studio. Small team, focused scope, real problems."

### 3. Featured Product — Scourr
- Badge: "Now live"
- H2: "Scourr"
- Description: AI-powered job hunt assistant. Automates job search pipeline, tracks applications, helps land interviews faster.
- CTA link: Visit scourr.app →

### 4. Portfolio Grid ("Also building")
Three equal cards:

| Product | Tagline | Status label |
|---------|---------|-------------|
| Pocket IT | MSP automation for IT teams. Remote management, monitoring, and client reporting without the bloat. | In development |
| Trift Flipper | Smart assistant for resellers and thrift flippers. Price lookup, profit tracking, and listing tools. | In development |
| Mirrrd | Social content automation for creators. Schedule, repurpose, and amplify across platforms. | Validation phase |

No product logos available at launch — use placeholder icons or stylized initials. Swap in real logos when available.

### 5. About Section (dark background)
- Left: label "About" + heading "Small team. Real software."
- Right: "Shoogar Soft is an independent software company based in Canada. We build focused, opinionated tools for real problems — no bloat, no committees, just software that works."
- Contact: corporate@shoogarsoft.com

### 6. Footer
- Left: © 2025 Shoogar Soft Inc.
- Right: corporate@shoogarsoft.com · Privacy Policy · Terms of Service

---

## Visual Style

**Warm / Indie Maker**

| Token | Value |
|-------|-------|
| Background | `#fffbf5` (warm cream) |
| Nav background | `#1c1410` (dark brown) |
| Accent | `#c97d4a` (amber) |
| Header font | Georgia (serif) |
| Body font | system-ui (sans-serif) |
| Primary text | `#1c1410` |
| Secondary text | `#8a6a50` |
| Featured card bg | `#fff8f0` |
| Featured card border | `#e8c99a` |
| About section bg | `#1c1410` |

---

## Responsive Behaviour

- Breakpoint: 600px
- Mobile nav collapses to hamburger menu (three-line toggle)
- Featured card stacks vertically; decorative visual element hidden
- Portfolio grid collapses from 3-column to single column
- About section stacks vertically

---

## Legal Pages

Both pages use the same shell (nav + footer) with minimal prose content:

- **Privacy Policy**: state that the site collects no personal data beyond what's in the contact email (corporate@shoogarsoft.com). No cookies, no tracking, no analytics at launch.
- **Terms of Service**: standard disclaimer — informational site, no warranties, links to third-party products are subject to those products' own terms.

Shoogar Soft Inc. is incorporated in Canada; legal pages should reflect Canadian jurisdiction.

---

## Assets

| Asset | Source | Notes |
|-------|--------|-------|
| Shoogar logo | `victoria/godot/assets/shoogar games.png` | Pixel art cube, circular crop, `image-rendering: pixelated` |
| Product logos | None at launch | Use emoji or stylized letter icons as placeholders |
| Favicon | Derived from Shoogar logo | 32×32 crop |

---

## Tech Stack

**Plain HTML + CSS.** No framework, no build step, no JavaScript dependencies.

- `index.html` — homepage
- `privacy.html` — privacy policy
- `terms.html` — terms of service
- `assets/shoogar-logo.png` — logo
- `assets/style.css` — shared stylesheet (nav, footer, typography tokens)

Inline styles in the mockup will be extracted to `style.css` during implementation.

---

## Hosting

Deploy as static files to shoogarsoft.com. Hosting provider TBD — Cloudflare Pages or similar static host recommended (free tier, automatic HTTPS, global CDN).

---

## Out of Scope

- Product detail pages (link out to each product's own domain)
- Blog / writing section
- Contact form (mailto link only)
- Analytics / tracking
- Social media links (GitHub, LinkedIn not ready)
- CMS
