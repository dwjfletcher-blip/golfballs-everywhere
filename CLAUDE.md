# CLAUDE.md — Golf Balls Everywhere Website Rebuild

## Project Overview
Rebuilding the frontend for **golfballseverywhere.com** — a Shopify-based e-commerce store selling used/recycled premium golf balls. The new site must:
- Match brand identity (colors, fonts, voice)
- Be fully SEO-optimized (technical + on-page)
- Connect to the existing Shopify store via the Storefront API
- Be hosted as a custom storefront (headless Shopify) or as a Shopify theme replacement

---

## Always Do First
1. **Invoke the `frontend-design` skill** before writing any frontend code, every session, no exceptions.
2. Check the `brand_assets/` folder for logos, color guides, or images before designing anything.

---

## Brand Identity

### Colors (exact hex values — do not deviate)
| Token | Hex | Usage |
|-------|-----|-------|
| `--color-background` | `#fbf7ef` | Page background (warm cream) |
| `--color-foreground` | `#1c1c26` | Primary text (dark charcoal) |
| `--color-accent` | `#e1c489` | Gold accent, highlights, borders |
| `--color-button-bg` | `#1c1c26` | Primary CTA buttons |
| `--color-button-text` | `#ffffff` | Button label |
| `--color-secondary-bg` | `#fbf7ef` | Secondary button background |

### Typography
- **Headings:** Oswald (700 bold) — tight tracking (`letter-spacing: -0.02em`), uppercase where used as labels
- **Body:** Assistant (400/700) — `line-height: 1.6`, `font-size: 1rem`
- Load both via Google Fonts: `Oswald:wght@700` and `Assistant:wght@400;700`
- Never use the same font for headings and body

### Voice & Tone
- Professional but approachable
- Trust-focused: emphasize quality grading, satisfaction guarantee, free returns
- Sustainability-conscious: recycled balls reduce manufacturing waste
- Performance-oriented: premium brands, rigorous testing

---

## Site Architecture

### Pages to Build
| Page | Route | Priority |
|------|-------|----------|
| Homepage | `/` | P0 |
| Shop All | `/shop` | P0 |
| Brand Collection | `/brand/[brand-slug]` | P0 |
| Product Detail | `/product/[handle]` | P0 |
| Grading Scale | `/grading-scale` | P1 |
| About Us | `/about` | P1 — **currently 404, must be created** |
| Contact | `/contact` | P1 |
| Shipping Policy | `/shipping-policy` | P2 |
| Returns & Refunds | `/returns` | P2 |
| Search Results | `/search` | P1 |

### Navigation Structure
**Primary Nav:** Home · Shop All · Brands (dropdown: 14 brand links) · About · Contact · [Cart icon] · [Search icon]
**Footer:** Quick Links (Grading Scale, Bulk Order, Shipping, Returns) · Social Icons · Legal

### Brands to Include in Dropdown
Titleist, Callaway, TaylorMade, Srixon, Bridgestone, Vice, Volvik, Kirkland, Nike, MaxFli, Top Flite, Wilson, Pinnacle, Slazenger

---

## Data & Shopify Integration

### Current Phase: Static Mockup
**No Shopify token is available yet.** All product data must be hardcoded as realistic mock data in a `data/products.js` file (or inline JS object). Do NOT attempt to fetch from any API. Do NOT leave placeholder "TODO: fetch from API" stubs — the mockup must fully render with the mock data.

When the Shopify token is provided in the future, the mock data layer will be swapped for live Storefront API calls. Write the data layer in a way that makes this swap easy (e.g., a `getProducts()` / `getCollection()` function that returns the mock data now and can be re-pointed to the API later).

### Mock Product Data to Use
Use these realistic products — do not invent different ones:

**Titleist:**
- Pro V1 — Mint (5A) — $34.99/dz
- Pro V1x — Near Mint (4A) — $29.99/dz
- AVX — Good (3A) — $19.99/dz

**Callaway:**
- Chrome Soft — Mint (5A) — $32.99/dz
- Supersoft — Near Mint (4A) — $18.99/dz
- ERC Soft — Good (3A) — $16.99/dz

**TaylorMade:**
- TP5 — Mint (5A) — $31.99/dz
- TP5x — Near Mint (4A) — $27.99/dz

**Srixon:**
- Z-Star — Mint (5A) — $28.99/dz
- Soft Feel — Good (3A) — $14.99/dz

**Bridgestone:**
- Tour B XS — Mint (5A) — $30.99/dz

**Vice:**
- Pro — Near Mint (4A) — $22.99/dz

**Bulk / Assorted:**
- 3-Dozen Assorted Mix — Good (3A) — $39.99
- 8-Dozen Extreme Value Pack — Good (3A) — $89.99
- 13-Dozen Bulk Pack — Mixed grades — $118.99

Use `https://placehold.co/400x400/e1c489/1c1c26?text={Brand}` for all product images.

### Cart & Checkout (Mockup Behavior)
- Cart state lives in JS (no API calls)
- "Add to Cart" updates a local cart count in the nav icon
- Cart drawer/slide-over opens and shows items
- "Checkout" button shows a disabled state with tooltip: "Connect your Shopify store to enable checkout"
- No `localStorage` persistence needed for the mockup

### Future Shopify API Notes (for when token is available)
- Store domain: `golfballseverywhere.com`
- Platform: Shopify Dawn v13.0.1
- API endpoint: `https://golfballseverywhere.com/api/2024-10/graphql.json`
- Token goes in `.env` as `SHOPIFY_STOREFRONT_TOKEN`
- Required scopes: `unauthenticated_read_product_listings`, `unauthenticated_read_collection_listings`, `unauthenticated_write_checkouts`, `unauthenticated_read_checkouts`

---

## SEO Requirements — Critical

### Technical SEO (must implement on every page)

#### Meta Tags (per-page dynamic)
```html
<title>{page-specific title} | Golf Balls Everywhere</title>
<meta name="description" content="{150-160 char description with primary keyword}">
<link rel="canonical" href="https://golfballseverywhere.com{path}">

<!-- Open Graph -->
<meta property="og:title" content="{title}">
<meta property="og:description" content="{description}">
<meta property="og:image" content="{product or hero image URL}">
<meta property="og:url" content="https://golfballseverywhere.com{path}">
<meta property="og:type" content="website"> <!-- or "product" on PDPs -->
<meta property="og:site_name" content="Golf Balls Everywhere">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{title}">
<meta name="twitter:description" content="{description}">
<meta name="twitter:image" content="{image URL}">
```

#### Page Title Templates
| Page | Title Template |
|------|---------------|
| Homepage | `Used Golf Balls - Premium Recycled Balls from Top Brands \| Golf Balls Everywhere` |
| Shop All | `Shop All Used Golf Balls - Titleist, Callaway, TaylorMade & More \| Golf Balls Everywhere` |
| Brand Collection | `Used {Brand} Golf Balls - Recycled & Graded \| Golf Balls Everywhere` |
| Product | `{Product Name} - Used {Brand} Golf Balls \| Golf Balls Everywhere` |
| Grading Scale | `Golf Ball Grading Scale: Pristine, Mint, Near Mint, Good \| Golf Balls Everywhere` |
| About | `About Golf Balls Everywhere - Quality Recycled Golf Balls \| Golf Balls Everywhere` |

#### Structured Data (JSON-LD — required per page type)

**Homepage / Sitewide:**
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Golf Balls Everywhere",
  "url": "https://golfballseverywhere.com",
  "logo": "https://golfballseverywhere.com/logo.png",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "6 Ormond Street",
    "addressLocality": "Marcus Hook",
    "addressRegion": "PA",
    "postalCode": "19061",
    "addressCountry": "US"
  },
  "sameAs": [
    "https://twitter.com/golfballsevery",
    "https://www.facebook.com/golfballseverywhere",
    "https://www.instagram.com/golfballseverywhere"
  ]
}
```

**Product Pages (PDP):**
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "{product title}",
  "description": "{description}",
  "brand": { "@type": "Brand", "name": "{brand}" },
  "offers": {
    "@type": "Offer",
    "price": "{price}",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "url": "https://golfballseverywhere.com/product/{handle}"
  }
}
```

**FAQ Page (Grading Scale):**
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    { "@type": "Question", "name": "What is Pristine grade?", "acceptedAnswer": { "@type": "Answer", "text": "..." } },
    ...
  ]
}
```

### On-Page SEO

#### Homepage Content Requirements
- H1: `Premium Used Golf Balls at Unbeatable Prices`
- Subheading (H2): `Shop Top Brands Like Titleist, Callaway & TaylorMade`
- Short intro paragraph (~100 words) using keywords: "used golf balls", "recycled golf balls", "premium golf balls", brand names
- Featured collections section (at minimum: Titleist, Callaway, TaylorMade, Srixon)
- Trust signals: "4-Tier Quality Grading", "100% Satisfaction Guarantee", "Free Returns", "Free Shipping over $79"
- About/sustainability blurb (100–150 words)

#### Collection Pages
- H1: `Used {Brand} Golf Balls`
- Descriptive paragraph above product grid (80–100 words, keyword-rich)
- Breadcrumbs: Home > Shop All > {Brand}
- Filter sidebar: Grade (Pristine / Mint / Near Mint / Good), Price range, Quantity

#### Product Pages
- H1: full product title
- Detailed description (150+ words covering: ball model, condition grade, what's included, brand info)
- Breadcrumbs: Home > Shop All > {Brand} > {Product}
- Related products section (same brand, same grade)
- "What grade is this?" link → Grading Scale page (internal link)

#### Internal Linking
- Every brand collection page links back to Shop All
- Every product page links to its brand collection
- Homepage links to all 14 brand collections
- Grading Scale page linked from every product page description

### URL Structure
- Collections: `/brand/titleist`, `/brand/callaway`, etc. (not `/collections/titleist`)
- Products: `/product/titleist-pro-v1-used-mint` (descriptive, keyword-rich slugs from Shopify handle)
- No duplicate content from filter params — use canonical tags on filtered views

### Performance (Core Web Vitals)
- LCP target: < 2.5s — lazy-load all below-fold images, preload hero image
- CLS: 0 — define image dimensions, avoid layout shifts from cart updates
- FID/INP: < 200ms — defer non-critical JS, load Shopify cart API async
- Use `<link rel="preconnect">` for: Google Fonts, Shopify CDN, analytics

---

## Page-Specific Requirements

### Homepage
- Hero section: full-width with golf course image + headline + CTA ("Shop Now" → /shop)
- Social proof bar: brand logos (Titleist, Callaway, TaylorMade, Srixon, etc.)
- "Why Golf Balls Everywhere" section: 3-4 trust icons (Quality, Value, Sustainability, Guarantee)
- Featured Products: 8-product grid pulled from Shopify (featured collection or best-sellers)
- Brand Collections: horizontal scroll or grid of 14 brand cards
- Grading explainer teaser: visual 4-tier breakdown → link to full grading page
- Sustainability section: short copy + stat (e.g., "X million balls recycled")

### Product Listing Page (PLP)
- Filtering: Grade, Brand, Price — update URL params (`?grade=mint&brand=titleist`)
- Sorting: Price (low/high), Newest, Best Match
- Product cards: image, title, grade badge, price, "Add to Cart" button
- Pagination or infinite scroll
- Out-of-stock items shown but grayed out with "Notify Me" option

### Product Detail Page (PDP)
- Image gallery (swipe on mobile)
- Grade badge + link to grading scale
- Quantity selector + "Add to Cart" → fires cart mutation
- Shipping teaser: "Free shipping on orders over $79"
- Accordion: Description / What's Included / Shipping & Returns
- Related products (same brand, 4 cards)

### Grading Scale Page
- Visual 4-tier breakdown:
  - **Pristine (5A):** Like new, no visible wear
  - **Mint (4A):** Minimal play marks, excellent condition
  - **Near Mint (3A):** Light scuffs, great playability
  - **Good (2A):** Visible wear, budget option
- FAQ schema implemented (see SEO section)

### About Page (currently 404 — must be created)
- Brand story: founded in Marcus Hook, PA; mission around sustainability
- How it works: balls collected → cleaned → graded → shipped
- Team/company credibility signals
- CTA: Shop Now

---

## Local Server & Screenshot Workflow
- Serve on localhost: `node serve.mjs` (project root → `http://localhost:3000`)
- Screenshot: `node screenshot.mjs http://localhost:3000`
- Screenshots saved to `./temporary screenshots/screenshot-N.png`
- After screenshotting, read the PNG with the Read tool and compare against reference
- Do minimum 2 comparison rounds before stopping

## Output Defaults
- Single `index.html` for static prototype; component-based if using a framework
- Tailwind CSS via CDN for prototypes: `<script src="https://cdn.tailwindcss.com"></script>`
- For production: use a framework (Astro, Next.js, or plain HTML/CSS/JS depending on scope)
- Mobile-first responsive, breakpoints: 640px / 768px / 1024px / 1280px

## Anti-Generic Guardrails
- **Colors:** Use only the brand palette above. Never use default Tailwind blue/indigo/purple.
- **Shadows:** Use layered, color-tinted shadows (`box-shadow: 0 4px 24px rgba(225,196,137,0.15), 0 1px 4px rgba(28,28,38,0.08)`)
- **Typography:** Oswald headings, Assistant body — never the same font for both
- **Gradients:** Warm cream gradients using `#fbf7ef` → `#f0e8d5` — no harsh color stops
- **Animations:** Only animate `transform` and `opacity`. Never `transition-all`
- **Interactive states:** Every button/link needs hover, focus-visible, and active states
- **Spacing:** Use consistent 8px base grid (8/16/24/32/48/64/96px)
- **Golf aesthetic:** Earthy, premium feel — think country club meets modern e-commerce

## Hard Rules
- Do not add sections or content not listed in this spec
- Do not use `transition-all`
- Do not use default Tailwind blue/indigo as primary color
- Do not stop after one screenshot pass
- **Mockup phase:** Use hardcoded mock data — do not attempt any Shopify API calls
- **Mockup phase:** Cart drawer is UI-only — no real checkout
- The `/about` page must be built (it is currently a 404 on the live site)
- All 14 brand collection pages must render from mock data, filterable by brand
