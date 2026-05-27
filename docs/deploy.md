# shoogarsoft.com — Deployment

Hosted on **Cloudflare Pages** (free tier). Static files served from CDN, HTTPS automatic.

## Live URLs

| URL | Purpose |
|-----|---------|
| https://shoogarsoft.com | Production (custom domain) |
| https://shoogarsoft-com.pages.dev | Cloudflare Pages URL (always works) |

## Deploy

Requires `wrangler` logged in (run from a real terminal, not a non-interactive shell):

```bash
wrangler login   # one-time, opens browser OAuth
```

To deploy:

```bash
cd code/shoogar
wrangler pages deploy . --project-name shoogarsoft-com --commit-dirty=true
```

This uploads all files in the directory and deploys to production immediately. No build step.

## GitHub

Repo: https://github.com/shukumei7/shoogarsoft-com (public, under `shukumei7` account)

GitHub is not connected to Cloudflare Pages for CI/CD — deploys are manual via `wrangler pages deploy`.

## Custom Domain Setup (already done)

Domain `shoogarsoft.com` is on Cloudflare (zone `d70e71eb9c5b8146502fd33ee7c59ef5`).

DNS record added manually:

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| CNAME | `@` | `shoogarsoft-com.pages.dev` | Proxied |

Custom domain added to Pages project via Cloudflare API:

```bash
curl -X POST "https://api.cloudflare.com/client/v4/accounts/c6febc11f6617e9adcab57ccbbddbc0b/pages/projects/shoogarsoft-com/domains" \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"name":"shoogarsoft.com"}'
```

## Files

```
index.html      — Homepage
privacy.html    — Privacy Policy
terms.html      — Terms of Service
assets/
  style.css     — Shared stylesheet
  shoogar-logo.png — Pixel art logo (from victoria/godot/assets/)
```
