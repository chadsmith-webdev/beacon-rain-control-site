# Beacon Rain Control — Website

Marketing and lead-generation site for Beacon Rain Control, a seamless gutter installation company based in Joshua, TX. Built and maintained by [Local Search Ally](https://localsearchally.com).

**Live site:** beaconraincontrol.com  
**Stack:** Astro · Tailwind CSS v4 · Vercel

---

## Local Development

```sh
npm install
npm run dev        # http://localhost:4321
npm run build      # Production build to ./dist/
npm run preview    # Preview production build locally
```

---

## Project Structure

```
src/
├── components/
│   ├── Nav.astro           # Sticky top nav with mobile hamburger
│   └── Footer.astro        # Footer with services, areas, and contact
├── layouts/
│   └── BaseLayout.astro    # Shared <head>, Nav, Footer, JSON-LD schema
├── pages/
│   ├── index.astro         # Homepage
│   ├── services.astro      # Services overview
│   ├── about.astro         # About Tracy Neff and the business
│   ├── service-areas.astro # Service areas hub page
│   ├── contact.astro       # Contact form
│   ├── [area].astro        # Dynamic city landing pages
│   ├── seamless-vs-sectional-gutters.astro
│   └── 5-inch-vs-6-inch-gutters.astro
├── styles/
│   └── global.css          # Design tokens (@theme) and base styles
├── data/
│   ├── site.json           # Global config — phone, address, services, service areas
│   └── serviceAreas.json   # City-level data for dynamic landing pages
└── assets/
    └── images/             # All photos (optimized by astro:assets)
public/
    ├── logo.svg
    ├── favicon.ico
    ├── og-image.jpg
    └── robots.txt
```

---

## Content Updates

### Business info (phone, hours, rating, warranty)

Edit `src/data/site.json`. All pages pull from this file — change it once, it updates everywhere.

### Adding or editing a city landing page

Edit `src/data/serviceAreas.json`. Each entry generates a page at `/{slug}`:

```json
{
  "slug": "gutter-installation-burleson-tx",
  "city": "Burleson",
  "county": "Johnson County",
  "zip": "76028",
  "intro": "...",
  "localDetail": "..."
}
```

### Adding a new city

Add an entry to `serviceAreas.json`. The page is automatically generated — no other files need updating.

---

## Design System

**Theme:** Light — white/navy surfaces with orange accent.

| Token                     | Value     | Use                                                    |
| ------------------------- | --------- | ------------------------------------------------------ |
| `--color-brand-accent`    | `#fe8636` | Button fills, active indicators only — never body text |
| `--color-brand-navy`      | `#283b77` | Text hover, nav, borders, focus rings                  |
| `--color-surface-bg`      | `#ffffff` | Page background                                        |
| `--color-surface-base`    | `#f5f6fb` | Cards, nav, footer                                     |
| `--color-surface-raised`  | `#eceef6` | Elevated cards                                         |
| `--color-content-primary` | `#1a2340` | All body and heading text                              |
| `--color-content-muted`   | `#556080` | Labels, captions                                       |

All tokens are defined in `src/styles/global.css` via the Tailwind v4 `@theme` block. Always use `var(--color-*)` — never hardcode hex values.

**Orange fails WCAG AA as text on white (~2.3:1).** It is used as a fill color only.

---

## Deployment

The site deploys automatically to **Netlify** on every push to `main`. No manual steps required.

Build output is fully static (`output: 'static'` in `astro.config.mjs`).

### Forms

Form submissions are handled by **Netlify Forms** — no backend or third-party service required.

| Form                               | `name` attribute   | Redirects to |
| ---------------------------------- | ------------------ | ------------ |
| Hero quick-quote (homepage)        | `quick-quote`      | `/thank-you` |
| Full estimate request (`/contact`) | `estimate-request` | `/thank-you` |

Both forms use:

- `data-netlify="true"` — enables Netlify Forms interception
- `netlify-honeypot="bot-field"` — built-in spam filtering (no reCAPTCHA)
- A hidden `<input name="form-name">` field required for static site detection

**Setting up email notifications:**
Netlify Dashboard → Site → Forms → Form notifications → add Tracy's email (`estimates@beacon-gutter.com`).
