# SEO Authority Restructure — Design

**Date:** 2026-07-28
**Status:** Approved (pending final spec review)
**Scope:** `shoogarsoft.com` (this repo, `code/shoogar/`) plus off-site checklist items for `rsp.shoogarsoft.com` and `scourr.app`.

## Context

`shoogarsoft.com` is a single-page marketing site (`index.html`, ~141 lines) with a hero, featured product (Scourr), a portfolio grid (Pocket IT, Thrift Flipper, Mirrrd, Research Server Patterns), a demo CTA, and an about section. Supporting pages: `privacy.html`, `terms.html`, `partner-deck.html`, `apps/chroma-slider/privacy.html`, `sitemap.xml`.

Current gaps identified:
- Chroma Slider has a privacy policy but no landing/marketing page — nothing exists for it to rank on.
- No JSON-LD structured data anywhere on the site.
- No Open Graph / Twitter card tags (link previews on social/Slack/etc. fall back to nothing).
- No canonical tags.
- `sitemap.xml` only lists 4 URLs and is already stale relative to the site.
- Meta descriptions/titles are missing or generic on non-homepage pages.
- GSC is set up as a **domain property** for `shoogarsoft.com` (covers `rsp.shoogarsoft.com` since it's a subdomain) but does **not** cover `scourr.app`, which is a separate root domain.

## Goals

Build SEO authority via three levers, explicitly **excluding** new content/blog work (out of scope for this pass):
1. Site architecture & internal linking
2. Technical SEO fundamentals
3. Off-site authority (backlinks, listings)

## Approach: Minimal Hardening

Keep the existing single-page homepage structure as-is (no rewrite of nav/sections into a multi-page architecture — that's explicitly deferred). Fix the concrete gaps above. A full multi-page restructure (breaking `/work`, `/about` etc. into real URLs) is deferred to a future phase 2 spec if/when there's enough unique content per section to justify it.

## 1. Architecture & Internal Linking

- **New file:** `apps/chroma-slider/index.html` — a real landing page for Chroma Slider (today the folder only contains `privacy.html`). Content: what the app does, screenshots/description, store link(s), link to its own `privacy.html`, link back to `shoogarsoft.com`.
- **Homepage:** add a Chroma Slider card to the `.portfolio` section (`index.html`), linking to `/apps/chroma-slider/`. It is not currently represented on the homepage at all.
- **`sitemap.xml`:** add the new Chroma Slider landing page; refresh `lastmod` dates.
- Reciprocal backlinks from RSP/Scourr back to shoogarsoft.com are out of this repo's scope (separate codebases) — tracked as a checklist item in section 3.

## 2. Technical SEO Fundamentals

- **JSON-LD structured data:**
  - `index.html`: `Organization` schema (name, url, logo, `sameAs` linking to product URLs/socials).
  - New Chroma Slider landing page: its own `SoftwareApplication` schema block.
  - Homepage portfolio cards for Scourr/RSP: minimal `SoftwareApplication` mentions (they don't have pages in this repo to carry full schema).
- **Meta tags:** every page (`privacy.html`, `terms.html`, `partner-deck.html`, new Chroma Slider page) gets a unique `<title>` and `<meta name="description">` — no reused/generic titles.
- **Open Graph / Twitter cards:** add `og:title`, `og:description`, `og:image`, `og:url`, and Twitter card equivalents to `index.html` at minimum (highest-value page for link previews).
- **Canonical tags:** `<link rel="canonical">` on every page, pointing at the `https://shoogarsoft.com/...` canonical URL.
- **Sitemap/robots:** expand and correct `sitemap.xml`; confirm `robots.txt` (Cloudflare-managed, not a repo file) doesn't block `/apps/`.
- **Heading hierarchy:** verify the new Chroma Slider page follows the existing single-`<h1>` pattern already used on the homepage.

## 3. Off-Site Authority (Checklist, Not Code)

This section produces a documented checklist, not implementation — these are manual/one-time actions or gaps to flag, not files to change in this repo:

- **Reciprocal backlinks:** verify `rsp.shoogarsoft.com` and `scourr.app` link back to `shoogarsoft.com` from their own footer/about. If missing, that's a fix in those separate codebases — route as a follow-up, not part of this plan.
- **Directory/listing submissions:** Product Hunt, AlternativeTo, relevant SaaS directories, app stores (for Chroma Slider). Each is a backlink + discovery surface. Documented as a checklist for manual follow-through.
- **GSC coverage gap:** `scourr.app` is a separate root domain not covered by the existing `shoogarsoft.com` domain property (verified 2026-06-17). Flag as a gap — needs its own GSC property if authority tracking matters for Scourr specifically.
- **Sitemap resubmission:** resubmit the expanded `sitemap.xml` to GSC after this work ships.

## Testing / Verification

- Validate JSON-LD with Google's Rich Results Test (manual, post-deploy).
- Confirm every page has a unique title/description via a quick grep across the repo's `.html` files.
- Confirm `sitemap.xml` URLs all 200 after deploy.
- No automated test suite exists for this static site; verification is manual inspection + Search Console validation post-deploy.

## Out of Scope

- Blog / case-study content strategy (explicitly excluded lever).
- Full multi-page restructure of the homepage (deferred to a future phase 2 spec).
- Any code changes to `rsp.shoogarsoft.com` or `scourr.app` (separate repos/domains) — off-site items there are checklist/follow-up only.
