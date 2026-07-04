# KMG Fine Jewelers — Site Conventions

Local scope for `clients/kmg-jewelers/`. This is a standalone git repo,
separate from the ProValley agency repo it lives under. Read this before
touching any page here.

## Site
- Domain: kmgfinejewelers.com
- Phone: (956) 563-2006 — `tel:+19565632006`
- Email: info@kmgfinejewelers.com
- Location: McAllen, TX
- Phone/email must appear in brand bar, trust bar, and footer on every page

## Deploy
- Vercel project: `kmg-jewelers` (`artiechrysler-7906`)
- GitHub repo: `Artdigital1/kmg-jewelers`
- Push to `main` auto-deploys to production — no staging branch
- `vercel.json` controls routing and clean URLs — every page needs a rewrite
  entry (clean URL → `.html`)
- `sitemap.xml` must be updated whenever a new page is added

## Stack — flat HTML files, no build step
Every page (`index.html`, `emerald.html`, `piaget-dancer-80564-k81.html`, and
any future product page) is a single self-contained `.html` file at the
project root with its own inline `<style>` block. No shared stylesheet, no
framework — copy an existing page's CSS rather than trying to factor it out.
Product photos live at the project root too, referenced directly by filename.

## Design system — dark luxury
```css
--gold:       #c9a84c;
--gold-light: #e8d5a3;
--gold-dark:  #a07830;
--black:      #0a0a0a;
--black-2:    #111111;
--black-3:    #1a1a1a;
--white:      #f5f5f0;
--gray:       #888880;
--border:     rgba(201,168,76,0.2);
```
Headings: Playfair Display (serif). Body: Inter. Dark background, gold
accents — no purple gradients, no generic AI aesthetic.

## Analytics — wired the same way on every page
- **GTM container:** `GTM-5X9DZSFG` — standard head snippet + body noscript
  iframe, copied verbatim into every page
- **GA4:** `G-3E16GFRNJF` — configured *inside* the GTM container (a GA4
  Event tag reading dataLayer pushes), not a hardcoded `gtag()` call in any
  HTML file
- **Formspree:** endpoint `https://formspree.io/f/mdabkwya`, shared across
  every inquiry form sitewide (one endpoint, not per-product)
- Every form submit pushes to `dataLayer` on success:
  `{ event: 'generate_lead', value, currency: 'USD', ... }`

## Form conventions
- Fields: `fullName`, `email`, `phone`, `message`
- Hidden field per product: `_subject` — set to that product's inquiry line
- Honeypot: `<input name="_gotcha" style="display:none" tabindex="-1">`
- CTA copy is always "Inquire to Purchase," never "Submit"

## Copy style
- **No em dashes anywhere in copy — hard rule.** Use commas or periods instead.
  (Note: existing pages predate this rule and still contain legacy
  em-dashes/`&mdash;` — don't copy those patterns into new pages.)
- Don't invent product specs — every fact on a product page traces back to
  what the site owner actually provided

## New product page checklist
1. Create the flat `.html` file at project root, copying design system + head
   boilerplate from an existing page
2. Add product photos at project root
3. Add rewrite entry to `vercel.json`
4. Add entry to `sitemap.xml`
5. Wire Formspree form with the standard fields + per-product `_subject`
6. Commit as: `Add [product name] product landing page`
