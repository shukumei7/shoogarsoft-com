---
design_system: shoogar-editorial
colors:
  bg: "#fffbf5"
  nav_bg: "#1c1410"
  nav_bg_hover: "#2c2418"
  accent: "#c97d4a"
  accent_light: "#e8c99a"
  text: "#1c1410"
  text_secondary: "#8a6a50"
  text_muted: "#a08060"
  border: "#e8d5c0"
  featured_bg: "#fff8f0"
  featured_border: "#e8c99a"
  card_bg: "#ffffff"
  about_bg: "#1c1410"
  footer_bg: "#130e09"
  footer_text: "#5a4030"
typography:
  body_font: "'Georgia', serif"
  nav_font: "system-ui, sans-serif"
  ui_font: "system-ui, sans-serif"
  nav_logo:
    letter_spacing: "2px"
    text_transform: uppercase
    weight: 700
spacing:
  max_width: "720px"
  nav_height: "56px"
radii:
  default: "6px"
  pill: "20px"
  small_badge: "4px"
  avatar: "50%"
components:
  nav:
    background: nav_bg
    logo_color: accent_light
    link_color: text_muted
    link_hover: accent_light
source_of_truth:
  - assets/style.css
---

# Shoogar — Design System (editorial)

Warm, light editorial theme (cream/terracotta) for the main body content, paired with a dark near-black nav/footer/about band — a light-mode page bookended by dark chrome, rather than a single global theme.

## Palette

CSS custom properties in `:root` (`assets/style.css:4-21`), single `--radius` and `--max-width` tokens shared across the whole stylesheet — always reference these two instead of repeating `6px` / `720px` literals.

- **Content areas** (`bg`, `card-bg`, `featured-bg`) are warm off-white/cream.
- **Dark chrome** (`nav-bg`, `about-bg`, `footer-bg`) — nav and footer share the near-black `#1c1410`/`#130e09` family; `about-bg` matches `nav-bg` exactly, so the "about" section is treated as chrome, not content.
- **Accent** (`accent` / `accent-light`) is terracotta/orange — used for the nav logo, hover states, and featured-post borders. `accent-light` is specifically the on-dark variant (nav/about), `accent` the on-light variant.
- **Text** steps `text` (primary, near-black) → `text-secondary` → `text-muted`, all warm-brown tinted rather than neutral gray — don't substitute generic grays here.

## Typography

Two-font split: **Georgia serif** for body copy (the editorial voice), **system-ui sans** for UI chrome (nav links, buttons, labels). This mirrors the content/chrome color split above — serif = content, sans = interface. Nav logo is uppercase, letterspaced, weight 700.

## Layout

Single `--max-width: 720px` content column — narrow, blog-like measure. Most sections wrap in `max-width: var(--max-width)` rather than fluid full-width layouts.

## Radii

One default radius (6px, via `--radius`) covers cards/buttons/inputs. Pills (nav-adjacent tags, filters) use fully-rounded 20px. Small badges use 4px. Avatars/logos are circular (50%). Stay on this scale — don't add a new arbitrary radius value.
