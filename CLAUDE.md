# **CLAUDE.md — Local Search Ally Client Website Projects**

You are assisting Chad Gorsuch, sole founder of Local Search Ally (LSA), build custom websites for home service trade contractors in Northwest Arkansas. Read this file fully before writing any code, copy, or structure.

---

## **Who You're Building For**

**Chad** is the developer and designer. He is the only person on the project. Talk to him directly. No corporate tone, no "the team," no "we."

**The client** is a home service contractor — HVAC, plumbing, roofing, electrical, landscaping, or remodeling. Their customers are homeowners searching locally. The site's job is to turn local search visibility into booked jobs.

---

## **Tech Stack**

* **Framework:** Astro (static-first, `.astro` components)  
* **Styling:** Tailwind CSS (utility-first; custom design tokens in `tailwind.config`)  
* **Fonts:** Loaded via `@fontsource` packages or `<link>` in `<head>`  
* **Deployment:** Vercel or Netlify (static output)  
* **Images:** Use `<Image />` from `astro:assets` for all images — never raw `<img>`  
* **Icons:** Lucide or inline SVG — never icon font libraries

Do not suggest React, Vue, Next.js, or WordPress unless Chad explicitly asks. Do not add dependencies without asking first.

---

## **Priorities (in order)**

1. **Clean, maintainable code** — Chad inherits and updates these sites long-term. Write code that a solo dev can understand six months from now without re-reading every file. Prefer explicit over clever.

2. **SEO best practices** — Every page should be search-engine ready out of the box:

   * Semantic HTML (`<main>`, `<article>`, `<section>`, `<nav>`, `<header>`, `<footer>`)  
   * One `<h1>` per page, logical heading hierarchy  
   * `<title>` and `<meta name="description">` on every page  
   * LocalBusiness JSON-LD schema on homepage and contact page (minimum)  
   * Descriptive `alt` text on every image  
   * No orphaned pages — everything linked  
3. **Accessibility (a11y)** — WCAG AA as the baseline:

   * All interactive elements keyboard-navigable  
   * Focus indicators visible (never `outline: none` without replacement)  
   * Color contrast ≥ 4.5:1 for body text, ≥ 3:1 for large text/UI  
   * ARIA labels on icon-only buttons and ambiguous links  
   * Skip-to-content link at top of every page  
4. **Tailwind conventions** — Use design tokens (see Design System below). Do not hardcode hex values in class attributes or inline styles. Extend Tailwind config for project-specific tokens.

5. **Performance** — Astro is fast by default; don't break it:

   * No client-side JS unless interaction requires it  
   * Use `client:idle` or `client:visible` for any hydrated components  
   * Defer non-critical scripts  
   * Compress and size images appropriately

   ---

   ## **Design System — Dark Mode Is the Default**

**Chad's signature as a designer is dark mode.** Every client site is dark-themed. Do not suggest or build light-mode layouts under any circumstances.

The specific colors and fonts are derived per-client from their logo and brand. There are no universal LSA brand tokens applied to client sites.

---

### **Step 1: Extract the Client Palette from Their Logo**

When Chad provides a client logo (or logo colors), derive the dark theme from it:

1. **Identify the primary brand color** from the logo — this becomes `brand.accent`  
2. **Darken it \~20%** for hover states → `brand.accent-dark`  
3. **Desaturate and darken it heavily** for surface tints → `brand.tint` (used sparingly)  
4. **Build the dark surface stack** using near-blacks with a subtle hue pulled from the brand color (e.g., if the logo is warm red, surfaces lean slightly warm)  
5. **Check contrast** — `brand.accent` must hit ≥ 4.5:1 against `surface.bg` for body text; ≥ 3:1 for large text and UI elements. Lighten if needed.

If the client has **no logo or brand colors**, use a neutral near-black dark theme and ask Chad which accent direction to take before proceeding.

---

### **Token Structure (define per project in `tailwind.config.cjs`)**

Use this shape for every project — fill in values per client:

* colors: {  
*   brand: {  
*     accent:       '\[PRIMARY LOGO COLOR\]',   // CTAs, links, borders, highlights  
*     'accent-dark':'\[DARKENED \~20%\]',        // Hover/pressed state  
*     tint:         '\[HEAVILY DESATURATED\]',  // Subtle tinted backgrounds (use sparingly)  
*   },  
*   surface: {  
*     bg:     '\[NEAR-BLACK, \~\#080808–\#111\]',  // Page background  
*     base:   '\[+1 STEP LIGHTER\]',            // Cards, nav, elevated sections  
*     raised: '\[+2 STEPS LIGHTER\]',           // Secondary surface, subtle contrast  
*   },  
*   content: {  
*     primary: '\#f2f2f2',   // All headings and body text (near-white, slightly warm or cool to match brand)  
*     muted:   '\#6b7280',   // Captions, labels, supporting text  
*   },  
*   border: {  
*     DEFAULT: 'rgba(255,255,255,0.06)',  
*     strong:  'rgba(255,255,255,0.12)',  
*   },  
* }


**Surface hue guidance:**

* Cool logo (blue, teal, purple) → surfaces lean slightly blue-gray (e.g., `#0a0c10`)  
* Warm logo (red, orange, gold) → surfaces lean slightly warm (e.g., `#100a08`)  
* Neutral logo (white, black, gray) → pure near-black surfaces (e.g., `#0a0a0a`)  
* Never make surfaces colorful — the brand color punches through the dark, it doesn't tint everything

**Color rules (apply to every project):**

* `brand.accent` does all interactive work: borders, links, CTAs, highlights  
* `brand.accent-dark` is the hover variant only — never used as a default state  
* `brand.tint` is for subtle section backgrounds or badge fills — use sparingly  
* Surface stack goes bg → base → raised (darkest to lightest elevation)  
* Border tokens use white opacity, not brand color, for dividers  
* Never invert to a light theme  
  ---

  ### **Typography**

Fonts are chosen per client — there are no default LSA fonts for client sites. When Chad provides a font direction (or the client has existing brand fonts), use those. When starting from scratch, ask Chad before picking fonts.

**Structure to define per project:**

* fontFamily: {  
*   display: \['\[CLIENT HEADING FONT\]', '\[FALLBACK\]', 'serif'\],  
*   body:    \['\[CLIENT BODY FONT\]', '\[FALLBACK\]', 'sans-serif'\],  
*   mono:    \['JetBrains Mono', 'Courier New', 'monospace'\], // for data/metrics only  
* }


**Typography rules (apply to every project regardless of font choice):**

* Display font: headings only — `font-bold`, letter-spacing `-0.02em`  
* Body font: all paragraph, UI, and nav text  
* Mono: scores, metrics, phone numbers formatted as data — never body copy  
* Eyebrow/label pattern: `font-body font-semibold uppercase tracking-widest text-xs`  
* Use `clamp()` for fluid heading sizes  
  ---

  ### **Component Patterns (token-agnostic, apply to every project)**

**Cards:**

* \<div class="bg-surface-base border border-border rounded-xl p-6"\>


**Emphasized cards / feature sections:**

* \<div class="bg-surface-raised border border-border-strong rounded-xl p-6"\>


**Primary CTA button (outline style — accent color on dark):**

* \<a class="border border-brand-accent text-brand-accent font-semibold  
*           px-6 py-3 rounded-md hover:bg-brand-accent hover:text-surface-bg  
*           transition-colors duration-200"\>


**Section eyebrow label:**

* \<p class="font-body font-semibold uppercase tracking-widest text-xs text-brand-accent mb-3"\>  
    
  ---

  ## **Site Structure (Standard Client Site)**

Every client site gets this page structure unless the brief says otherwise:

* /                   Homepage — hero, services summary, trust signals, CTA  
* /services           Services overview (or per-service subpages if scope allows)  
* /about              About the contractor — who they are, service area, story  
* /contact            Contact form \+ phone \+ GBP embed  
* /\[city\]-\[service\]/  Location/service area pages (e.g., /rogers-hvac-repair/)


**Navigation:** Logo left, links right, CTA button far right ("Get a Free Quote" or similar). Mobile: hamburger menu. No mega menus.

**Footer:** Logo, brief tagline, service list, service area list, contact info, copyright. Never "designed by" attribution without Chad's explicit approval.

---

## **SEO — Required on Every Page**

### **Meta Tags (Astro frontmatter pattern)**

* \---  
* const title \= "HVAC Repair in Rogers, AR | \[Client Name\]";  
* const description \= "Fast, reliable HVAC repair in Rogers, AR. \[Client Name\] serves Benton County homeowners. Call for same-day service.";  
* \---  
* \<html lang="en"\>  
* \<head\>  
*   \<meta charset="UTF-8" /\>  
*   \<meta name="viewport" content="width=device-width, initial-scale=1.0" /\>  
*   \<title\>{title}\</title\>  
*   \<meta name="description" content={description} /\>  
*   \<link rel="canonical" href={Astro.url} /\>  
*   \<\!-- Open Graph \--\>  
*   \<meta property="og:title" content={title} /\>  
*   \<meta property="og:description" content={description} /\>  
*   \<meta property="og:type" content="website" /\>  
* \</head\>


  ### **LocalBusiness JSON-LD (Homepage \+ Contact Page)**

* \<script type="application/ld+json"\>  
* {  
*   "@context": "https://schema.org",  
*   "@type": "LocalBusiness",  
*   "name": "\[Client Business Name\]",  
*   "telephone": "\[Phone\]",  
*   "address": {  
*     "@type": "PostalAddress",  
*     "streetAddress": "\[Street\]",  
*     "addressLocality": "\[City\]",  
*     "addressRegion": "AR",  
*     "postalCode": "\[ZIP\]",  
*     "addressCountry": "US"  
*   },  
*   "areaServed": \["Rogers", "Bentonville", "Fayetteville", "Springdale"\],  
*   "url": "\[Site URL\]"  
* }  
* \</script\>


Adjust `@type` to match the trade:

* HVAC: `"HVACBusiness"`  
* Plumber: `"Plumber"`  
* Electrician: `"Electrician"`  
* Roofing: `"RoofingContractor"`  
* Landscaping: `"LandscapingBusiness"`

  ### **Heading Rules**

* One `<h1>` per page — always above the fold, keyword-targeted  
* `<h2>` for major sections, `<h3>` for subsections within those  
* Never skip levels (no `<h1>` → `<h3>`)  
* Headings describe content, not brand personality  
  ---

  ## **File & Folder Conventions**

* src/  
*   components/     Reusable .astro components (Button, Card, Nav, Footer…)  
*   layouts/        Page layout wrappers (BaseLayout.astro, etc.)  
*   pages/          One file per route  
*   styles/         global.css (Tailwind base \+ custom overrides)  
*   data/           Site config, service lists, city lists (JSON or TS)  
*   assets/         Images, SVGs (processed by astro:assets)  
* public/           Unprocessed static files (favicon, robots.txt, og-image.jpg)


**Naming:**

* Components: `PascalCase.astro`  
* Pages: `kebab-case.astro`  
* Data files: `kebab-case.json` or `kebab-case.ts`  
* CSS custom properties: `--kebab-case`  
  ---

  ## **Code Style Rules**

* **No inline styles** unless absolutely unavoidable (document why with a comment)  
* **No hardcoded colors** — use Tailwind tokens or CSS custom properties  
* **No `!important`** — fix specificity instead  
* **Component comments:** Add a one-line comment at the top of each component describing what it does and where it's used  
* **Props:** Type all Astro component props explicitly using TypeScript interfaces  
* **No dead code** — don't leave commented-out blocks; use git history instead  
* **Imports:** Sorted — Astro/framework first, then components, then utilities  
  ---

  ## **Copy Tone (for placeholder and sample content)**

When generating sample copy for client sites:

* **Problem-first.** Lead with what the homeowner is losing or risking.  
* **Plain language.** Name the actual thing — "furnace repair," not "HVAC solutions."  
* **Specific.** Name the city, the trade, a concrete outcome where possible.  
* **Low-pressure CTAs.** "Get a free quote" or "See if we serve your area" — never "Act now" or "Limited spots."  
* **No banned phrases:** "dominate," "online presence," "digital presence," "solutions," "seamless," "cutting-edge," "leverage," "game-changing."  
  ---

  ## **What NOT to Do**

* Do not add `console.log` statements to production code  
* Do not install packages silently — ask first, explain why  
* Do not generate light-mode designs or suggest toggling dark mode  
* Do not use `<table>` for layout  
* Do not use CSS frameworks other than Tailwind  
* Do not use client-side JS for anything that can be done at build time  
* Do not write generic placeholder copy ("Lorem ipsum" or "Your content here") — write real contractor-context sample copy if a placeholder is needed  
* Do not add "Designed by Local Search Ally" or footer attribution without asking  
  ---

  ## **When Starting a New Client Project**

Before writing any code, ask Chad for:

1. **Business name** and primary trade (HVAC, plumbing, roofing, etc.)  
2. **Service area** — which cities/counties they cover  
3. **Primary CTA** — call, form submission, or booking link?  
4. **Phone number and address** (for schema and footer)  
5. **Logo file** — provide the logo so the dark theme palette can be derived from it; if no logo exists, ask Chad which accent color direction to take before writing any CSS  
6. **Priority pages** — what to build first

Then confirm the approach before writing a single component.

