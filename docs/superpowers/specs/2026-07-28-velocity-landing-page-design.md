# Velocity Landing Page — Design

**Date:** 2026-07-28
**Status:** Approved (pending final spec review)
**Scope:** `shoogarsoft.com` (this repo, `code/shoogar/`) — a new landing page for Velocity, plus SEO/homepage wiring. Asset sourcing from `code/velocity/` (read-only source, no edits to that repo).

## Context

Velocity ("Midnight Endless Run") is an NFS-Underground-inspired browser racing game, built as its own project under `code/velocity/`. It is live and playable on itch.io at `https://shoogar.itch.io/velocity-midnight-run`. It is not currently represented anywhere on `shoogarsoft.com` — no portfolio card, no dedicated page.

Unlike Chroma Slider (a small mobile game whose landing page reused the site's plain `.legal` text layout), Velocity has substantial marketing asset depth: a hero banner, a curated screenshot set (`code/velocity/marketing-screenshots/v1.1/`), a trailer/sweep video, and a documented 3-car roster. This warrants a richer, self-contained landing page rather than the minimal text pattern.

## Goals

1. A visually rich landing page for Velocity at `shoogarsoft.com/apps/velocity/`, with itch.io as the primary CTA.
2. Full SEO/homepage treatment matching the precedent set for Chroma Slider: sitemap entry, JSON-LD structured data, canonical tag, and a homepage portfolio card.

## Approach: Rich Self-Contained Page

`apps/velocity/index.html` carries its own `<style>` block — the same pattern `partner-deck.html` already uses on this site — rather than reusing the shared `assets/style.css` classes. This lets the page use a dark neon theme matching the game's actual branding, distinct from the site's light amber/cream theme, without touching shared CSS or affecting any other page.

## 1. Asset Preparation

Source assets live in `code/velocity/` (read-only — no changes to that repo). Several are oversized for web use and need optimization before copying into `code/shoogar/assets/`:

- **Hero background**: `code/velocity/assets/banner-cropped.png` (1783×1440, 2.98MB) → resized/compressed to a web-appropriate JPEG (target: under ~300KB, similar quality bar to the site's existing photo assets).
- **Screenshot gallery** (6 curated shots from `code/velocity/marketing-screenshots/v1.1/`, each currently 200-425KB PNG at 1280×720): `01-title.png`, `04-street-run.png`, `05-near-miss.png`, `06-racer-encounter.png`, `09-high-speed-chase.png`, `10-cop-pursuit.png` → compressed to JPEG.
- **Trailer video**: `code/velocity/marketing-screenshots/v1.1/00-autopilot-sweep.mp4` (9.4MB) → copied as-is (in line with the site's existing `scourr-showcase.mp4`/`rsp-bizowner.mp4`, both ~8.5-8.9MB). A poster JPEG is generated from a representative frame (or reuses one of the compressed screenshots), matching the ~30KB poster convention already used for the Scourr/RSP videos.
- **Car roster**: no images — presented as text/stat cards, data pulled from `code/velocity/README.md`'s existing car table (Kaze GT-R / Rampage 440 / Tempest RS: drivetrain, handling character).

All optimized assets land in `code/shoogar/assets/` with a `velocity-` prefix (e.g. `assets/velocity-hero.jpg`, `assets/velocity-screenshot-01-title.jpg`, `assets/velocity-sweep.mp4`, `assets/velocity-sweep-poster.jpg`), matching the existing naming convention (`scourr-showcase.mp4`, `rsp-bizowner-poster.jpg`).

## 2. Page Content (`apps/velocity/index.html`)

- **Hero**: full-width banner background, `<h1>VELOCITY — Midnight Endless Run</h1>`, tagline ("NFS-Underground-inspired endless arcade street racer"), primary CTA button → `https://shoogar.itch.io/velocity-midnight-run` (`target="_blank" rel="noopener"`).
- **Trailer**: embedded `<video>` (controls, preload="metadata", poster) — same markup pattern as the homepage's existing Scourr/RSP video embeds.
- **Screenshot gallery**: responsive grid of the 6 compressed screenshots.
- **Car roster**: 3 cards (Kaze GT-R — FWD hot hatch; Rampage 440 — RWD muscle; Tempest RS — AWD rally turbo), each with a one-line handling description pulled from the README.
- **Footer CTA**: itch.io link repeated + a link back to `shoogarsoft.com` (mirrors Chroma Slider's "From Shoogar Soft" pattern, reusing the same wording style).

## 3. SEO / Homepage Wiring

- **JSON-LD**: `VideoGame` schema on the Velocity page (name, description, genre, `gamePlatform`, publisher Organization block, and the itch.io URL as the primary `url`/play link).
- **Meta description + canonical tag**: `https://shoogarsoft.com/apps/velocity/` (extensionless, consistent with the Cloudflare Pages redirect behavior confirmed during the prior SEO restructure).
- **Open Graph + Twitter Card tags**: full treatment on this page (title/description/url/image), since it's a genuinely shareable page — unlike the "index.html only" minimum applied to Chroma Slider.
- **`sitemap.xml`**: new entry for `https://shoogarsoft.com/apps/velocity/`.
- **Homepage portfolio card**: new `.portfolio-card` in `index.html`'s `.portfolio-grid`, linking to `apps/velocity/`, matching the existing card pattern (icon, title, description, status, link).

## Testing / Verification

- Validate JSON-LD with `node -e` parsing (same pattern as the prior SEO work).
- Confirm every optimized asset is meaningfully smaller than its source while remaining visually acceptable (spot-check file sizes).
- Confirm the page has exactly one canonical tag, one meta description, one `<h1>`.
- Confirm `sitemap.xml` stays well-formed and the new URL resolves.
- Manual browser check (via claude-in-chrome against a local static server) that the page renders, the video plays, the gallery and car roster display correctly, and the itch.io link is correct — no automated test suite exists for this static site.

## Out of Scope

- Any changes to `code/velocity/` itself (read-only asset source).
- The scroll-driven "car following a winding road" animation concept raised during brainstorming — explicitly dropped by the user in favor of this richer static page.
- A full multi-page restructure of `shoogarsoft.com` (still out of scope per the prior SEO restructure spec).
- Off-site authority checklist additions for Velocity/itch.io — not requested this round; can follow the pattern in `docs/seo-offsite-checklist.md` later if wanted.
