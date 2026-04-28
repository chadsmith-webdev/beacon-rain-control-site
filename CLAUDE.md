# CLAUDE.md — Beacon Rain Control Website

You are assisting Chad Smith (Local Search Ally) maintain and improve the Beacon Rain Control website. Read this file fully before writing any code or copy.

---

## Project

**Client:** Beacon Rain Control  
**Owner:** Tracy Neff  
**Location:** Joshua, TX 76058  
**Phone:** (817) 681-6041  
**Email:** estimates@beacon-gutter.com  
**Site:** beaconraincontrol.com  
**Service area:** Johnson County and southern Tarrant County, TX

Seamless aluminum gutter installation, repair, and gutter guards. Owner-operated. Tracy does the work himself — no subcontractors.

---

## Tech Stack

| Layer | Choice |
|---|---|
| Framework | Astro (static output) |
| Styling | Tailwind CSS v4 — design tokens in `src/styles/global.css` via `@theme` block |
| Images | `<Image />` from `astro:assets` — never raw `<img>` |
| Icons | Inline SVG only — no icon font libraries |
| Deployment | Vercel |

Do not suggest React, Vue, Next.js, or WordPress. Do not add dependencies without asking.

---

## Folder Structure

```
src/
  components/     Nav.astro, Footer.astro
  layouts/        BaseLayout.astro
  pages/          One file per route + [area].astro for city pages
  styles/         global.css — all design tokens and base styles
  data/           site.json (global config), serviceAreas.json (city pages)
  assets/images/  All photos (processed by astro:assets)
public/           favicon, logo.svg, robots.txt, og-image
```

**Key data files:**
- `src/data/site.json` — phone, address, hours, rating, reviewCount, warranty, serviceAreas list, services list
- `src/data/serviceAreas.json` — array of city objects (`slug`, `city`, `county`, `zip`, `intro`, `localDetail`)

---

## Design System

### Theme: Light — white/navy surfaces, orange accent

The logo is orange (#fe8636) and navy (#283b77). The site uses a clean light theme derived from those colors.

### Design tokens (defined in `src/styles/global.css`)

```css
--color-brand-accent: #fe8636;       /* Orange — fill-only (buttons, active indicators) */
--color-brand-accent-dark: #d96b24;  /* Darker orange — hover state on orange buttons */
--color-brand-navy: #283b77;         /* Navy — text, nav, borders, interactive hover */
--color-brand-navy-light: #3a52a0;   /* Lighter navy — secondary interactive states */
--color-brand-tint: #eef0f8;         /* Very light navy tint — subtle section backgrounds */

--color-surface-bg: #ffffff;         /* Page background */
--color-surface-base: #f5f6fb;       /* Cards, nav, footer */
--color-surface-raised: #eceef6;     /* Elevated cards, feature sections */
--color-surface-navy: #283b77;       /* Full navy sections (future use) */

--color-content-primary: #1a2340;    /* All body and heading text */
--color-content-muted: #556080;      /* Labels, captions, secondary text */
--color-content-on-navy: #ffffff;    /* Text on navy backgrounds */

--color-border: rgba(40,59,119,0.10);
--color-border-strong: rgba(40,59,119,0.20);
```

### Color rules

- **Orange is fill-only.** Never use orange as body text or link text on white/light surfaces — it fails WCAG AA (~2.3:1 contrast). Orange works as: button backgrounds, the active nav underline indicator, icon fills.
- **Navy handles all interactive text.** Nav hover, footer link hover, focus rings, outlined buttons — all navy.
- **Hero photo overlays stay dark.** The gradient overlays on hero sections (`rgba(9,11,18,...)`) are for text legibility over photos — do not lighten them.
- **Surface stack:** bg (white) → base (#f5f6fb) → raised (#eceef6) — lightest to most elevated.
- **Borders** use navy opacity, not white opacity.

### Typography

```css
--font-family-display: Georgia, "Times New Roman", serif;   /* Headings only */
--font-family-body: "Helvetica Neue", Arial, sans-serif;    /* All body/UI text */
```

- Display: `font-bold`, `letter-spacing: -0.02em`, size via `clamp()`
- Eyebrow labels: `text-xs font-semibold uppercase tracking-widest`, color `var(--color-brand-accent)`
- Use `var(--color-*)` custom properties everywhere — never hardcode hex values outside hero overlay gradients

### Component patterns

**Standard card:**
```html
<div style="background-color: var(--color-surface-raised); border-color: var(--color-border-strong);"
     class="rounded-xl p-6 border">
```

**Primary CTA:**
```html
<a class="cta-primary inline-flex items-center justify-center px-7 py-3.5 rounded font-semibold text-sm">
```

**Secondary CTA:**
```html
<a class="cta-secondary inline-flex items-center justify-center px-7 py-3.5 rounded font-semibold text-sm border">
```

**Section eyebrow:**
```html
<p class="text-xs font-semibold uppercase tracking-widest mb-3" style="color: var(--color-brand-accent);">
```

**Hero section:** Full-bleed photo + dark gradient overlay + bottom fade to `var(--color-surface-bg)`. Min-height: `clamp(560px, 70vh, 760px)` — consistent across all pages.

---

## Pages

| Route | File | Notes |
|---|---|---|
| `/` | `pages/index.astro` | Homepage — hero, services, owner bio, reviews, FAQ |
| `/services` | `pages/services.astro` | All 4 services with detail sections |
| `/about` | `pages/about.astro` | Tracy's story, owner photo, service area tags |
| `/service-areas` | `pages/service-areas.astro` | Hub page — links to all city pages |
| `/contact` | `pages/contact.astro` | Contact form + phone + address |
| `/[area]` | `pages/[area].astro` | Dynamic city landing pages from `serviceAreas.json` |
| `/seamless-vs-sectional-gutters` | `pages/seamless-vs-sectional-gutters.astro` | Educational article |
| `/5-inch-vs-6-inch-gutters` | `pages/5-inch-vs-6-inch-gutters.astro` | Educational article |

**Navigation links:** Services, About, Areas, Learn, Contact. CTA button: "Get a Free Estimate in Joshua, TX."

---

## Copy Voice

Tracy speaks directly to homeowners. First person singular throughout — **"I" always, never "we."**

- Lead with the problem (water damage, overflowing gutters, foundation risk)
- Name the city, county, or specific detail — never vague
- CTAs are low-pressure: "Get a Free Estimate," "Call," "Text for a Quote"
- Back up claims: 5-year written warranty, free same-day estimates, on-site fabrication

**Banned words:** seamless (ironic for a gutter company — use "no seams" or "continuous"), solutions, leverage, cutting-edge, dominate, digital presence, game-changing.

---

## SEO Requirements

Every page must have:
- Unique `<title>` and `<meta name="description">` set in frontmatter
- `<link rel="canonical">` via `BaseLayout`
- One `<h1>` above the fold, then `<h2>` → `<h3>` — never skip levels
- Descriptive `alt` text on every image (`aria-hidden="true"` on decorative hero photos)

Homepage and contact page have `LocalBusiness` JSON-LD with `@type: "HomeAndConstructionBusiness"`.

---

## Code Rules

- **No hardcoded colors** outside hero overlay gradients — always `var(--color-*)`
- **No inline styles** unless a CSS variable or dynamic value requires it
- **No `!important`** — fix specificity
- **No `console.log`** in production
- **No dead code** — use git history instead of commented-out blocks
- **`<Image>` always** — never raw `<img>` tags
- One-line component comment at the top of each `.astro` file describing its role
- Mobile-first — homeowners often browse on phones in the driveway

---

## What NOT to Do

- Do not suggest dark mode or toggle between themes — this site is light-themed
- Do not use orange text on white or light surfaces — it fails WCAG contrast
- Do not install packages without asking
- Do not add footer attribution ("Built by Local Search Ally") without asking
- Do not write "we" anywhere in copy — always "I"
