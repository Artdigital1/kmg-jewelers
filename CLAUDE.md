# KMG Fine Jewelers — Site Conventions

Local scope for `clients/kmg-jewelers/`. This is a standalone git repo,
separate from the ProValley agency repo it lives under. Read this before
touching any page here.

## Site
- Domain: kmgfinejewelers.com
- Phone: (956) 563-2006 — `tel:+19565632006` — must appear in brand bar,
  trust bar, and footer on every page
- Location: McAllen, TX / Rio Grande Valley

## Deploy
- Vercel project: `kmg-jewelers`, project ID `prj_CjqO2wagkUcNJL104RLCMZuFi6Ru`
- Org/team: `artiechrysler-7906`, org ID `team_pWVTqXLuIi2FU8d3DO1RNPL0`
- Push to `main` auto-deploys to production — no staging branch
- Adding a page requires 3 things together: the `.html` file, a rewrite in
  `vercel.json` (clean URL → `.html`), and an entry in `sitemap.xml`

## Page pattern — flat-file, no build step
Every page (homepage and product landing pages like `emerald.html`,
`piaget-dancer-80564-k81.html`) is a single self-contained `.html` file at
repo root with its own inline `<style>` block. There is no shared
stylesheet and no framework — copy the CSS from an existing page
(`emerald.html` is the reference) rather than trying to factor it out.

## Design tokens
```css
--gold:       #c9a84c
--gold-light: #e8d5a3
--gold-dark:  #a07830
--black:      #0a0a0a
--black-2:    #111111
--black-3:    #1a1a1a
--white:      #f5f5f0
--gray:       #888880
--border:     rgba(201,168,76,0.2)
```
Headings: Playfair Display. Body: Inter. Dark background, gold accents —
no purple gradients, no generic AI aesthetic.

## Analytics — all three wired the same way on every page
- **GTM container:** `GTM-5X9DZSFG` — the standard head snippet + body
  noscript iframe, copy-pasted verbatim into every page
- **GA4:** `G-3E16GFRNJF` — configured *inside* the GTM container (a GA4
  Event tag reading dataLayer pushes), not a hardcoded `gtag()` call in
  any HTML file. Don't add a separate gtag.js snippet — it's not how
  this site tracks GA4.
- **Formspree:** endpoint `https://formspree.io/f/mdabkwya`, shared
  across every inquiry form sitewide (one endpoint, not per-product)
- Every form submit pushes to `dataLayer`:
  `{ event: 'generate_lead', form_id, item_id, value, currency: 'USD' }`
  on success — GTM's GA4 Event tag reads this to fire the conversion

## Form conventions
- Honeypot field: `<input name="_gotcha" style="display:none" tabindex="-1">`
- Hidden `_subject` field naming has been inconsistent across pages
  (`"Emerald Inquiry — 12.47ct..."` vs `"New Inquiry: Piaget Dancer..."`)
  — pick one pattern going forward: `"New Inquiry: [Product Name]"`
- CTA copy is always "Inquire to Purchase," never "Submit"

## Copy style
- No em dashes — use commas or periods instead. This is a go-forward
  rule; existing pages (`index.html`, `emerald.html`, the Piaget page)
  still have legacy em-dashes/`&mdash;` that haven't been cleaned up yet
- Don't invent product specs — every fact on a product page traces back
  to what the site owner actually provided
