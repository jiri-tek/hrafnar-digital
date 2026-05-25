# Hrafnar Digital — presentation site

Single-file static site. Zero backend, zero build step.

## Run locally
Just open `index.html` in a browser. (Or: `python3 -m http.server` in this folder.)

## What's already done
- Custom Hrafnar Digital SVG mark (single raven + eye + signal strokes —
  same family as Guides, deliberately distinct). Recolorable, used as
  header lockup, faint watermark, and favicon.
- Founder photo embedded
- Brand colors: `#0F1820` (primary) / `#FC4D02` (accent)
- "Curator of the North", Cinematic & Stoic tone, Anatomy-of-the-Raven
  principles section, anonymized testimonials
- 6 services, monthly pricing (from 150,000 / 300,000 ISK / custom)
- Real contact email: staffa.ppc@gmail.com
- Fully responsive, accessible, scroll animations, mobile nav

## Replace before going live (search for `PLACEHOLDER` in index.html)
1. **Fonts** — supply licensed `Gascogne Serial` + `Sarvatrik` woff2 into
   `assets/fonts/` and uncomment the `@font-face` block. Until then it falls
   back to Fraunces + Hanken Grotesk (close visual match, free).
2. **Hero image** — swap the Unsplash URL for your own/licensed photo.
3. **Logo** — the SVG mark is a strong starting point; have it refined by a
   designer if you want a final brand asset.
4. **Form** — set `action="REPLACE_WITH_FORMSPREE_ENDPOINT"` to a real
   Formspree (or similar) endpoint.
5. **Stats & testimonials** — replace placeholder numbers/quotes with real,
   verified ones when you have them.

## Deploy
Drag the folder onto Netlify, or `vercel`, or push to GitHub Pages. Static,
nothing else needed.

## Would improve with more time
- Transparent-background SVG version of the logo
- Real photography set with consistent grade
- Icelandic (íslensku) language toggle
- GA4 / consent banner if you'll run ads to this page
