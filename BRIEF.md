# Build Brief: Hrafnar — Presentation Website

## 0. How to use this document
You are Claude Code. Build a complete, production-ready, single-page presentation
website from this brief. Read the whole document first, then build. Make
reasonable decisions where details are missing. Prioritize a polished,
distinctive result over speed. At the end, give me a short README with how to
run it locally and how to deploy.

---

## 1. Project summary

**Brand:** Hrafnar Digital (sister brand to "Hrafnar Guides" — keep the
"Hrafnar [X]" family naming consistent)
**Wordmark on site:** HRAFNAR DIGITAL (logo symbol + wordmark lockup in header)
**Tagline:** "Digital growth for Icelandic hospitality."
**Brand idea (inherited from Hrafnar Guides blueprint):** "The Curator of
the North" — the one who separates signal from noise and shows only what
matters. This drives the tone: clear, premium, restrained, no noise, no
buzzword soup. Curated, not cluttered.

### Brand DNA (translated from Hrafnar Guides → Digital)
Hrafnar Digital is the sister brand of Hrafnar Guides (expeditions). Reuse
the DNA, re-express it for digital marketing — do NOT copy expedition copy.

- **Essence:** The Curator of the North. Signal over noise.
- **WHY:** Real growth is never in the noise (vanity metrics, trends).
- **HOW:** Hard data + deep knowledge of Icelandic hospitality (season,
  tourist, platform).
- **WHAT:** Curated digital work — not an agency package.
- **Values — Anatomy of the Raven (use as a 3-principle section):**
  - The Eye → **Insight**: clarity in the chaos; decisions start in data.
  - The Wing → **Guardianship**: the client's budget protected like our own.
  - The Bone → **Essence**: beauty in reduction; fewer channels, done well.
- **Tone of voice:** Cinematic & Stoic. Short, blunt sentences. Strong
  verbs. A narrator with authority. "Less is more." Every line earns its
  place. No filler, no hype, no exclamation marks.
- **Brand-face echo:** the Guides guarantee "I know the way out and the
  way back" → founder line "I know the way in — and the way back."
- **Visual codes:** Charcoal (#0F1820), Off-white (#F2EFE9), Red-Orange
  "Světlice" flare accent (#FC4D02). Gascogne Serial headlines, Sarvatrik
  body. Dark Premium.
**What we do:** Online marketing, consulting, and business management for
Icelandic hospitality businesses — guesthouses, hotels, restaurants, cafés,
tour operators. We handle everything end to end: websites, Google Ads,
and platform management (Airbnb, Booking.com, Bókun, etc.).

**Positioning:** International-level marketing expertise (5+ years, dozens of
satisfied clients, large ad budgets, performance/data focus) delivered to the
Icelandic hospitality market with full transparency. The message is NOT
"cheap" or "budget". It is a quiet, confident stance: **fair, transparent
pricing — fairer than typical local agencies — and you pay for outcomes,
not overhead.** Dark Premium in tone, honest in price. Never beg on price;
state it plainly and let the value carry it.

**Founder:** Jiří Štaffa — performance marketer, 5 years experience in the
Czech Republic, specializing in data-driven campaigns and paid acquisition.
LinkedIn: https://www.linkedin.com/in/ji%C5%99%C3%AD-%C5%A1taffa-33289b215/
Legal entity: an s.r.o. (Ltd.) company — mention "Operated by a registered
Ltd. company" in the footer for trust, no fake registration numbers.

**Pricing anchor:** Monthly retainers. Entry tier ≈ 10 hours/month of work
at ~15,000 ISK/h → display "from 150,000 ISK / month". Mid tier "from
300,000 ISK / month". Top tier custom. Pricing is a transparent starting
point tied to real hours, not a discount play.

---

## 2. Tech stack & constraints

- **Single-page static site.** Plain HTML + CSS + vanilla JS, OR Astro/Vite if
  you prefer a build step — your call, but keep it simple to deploy on
  Netlify/Vercel/GitHub Pages with zero backend.
- No framework lock-in required. No database. No backend.
- Fully responsive (mobile-first, test 375px → 1440px+).
- Accessible: semantic HTML, alt text, keyboard nav, prefers-reduced-motion
  respected, color contrast AA minimum.
- Fast: lazy-load images, no heavy libraries. Lighthouse 90+ target.
- Single contact form that opens the user's mail client via `mailto:` OR
  posts to a Formspree-style endpoint left as a clearly marked placeholder
  (`REPLACE_WITH_FORMSPREE_ENDPOINT`).
- All placeholder content clearly marked with `<!-- PLACEHOLDER -->` comments
  so I can find and replace it.

---

## 3. Visual direction & brand identity

**Aesthetic:** Dark, Nordic, premium, curated. "The Curator of the North."
Deep blue-black backgrounds, generous negative space, large editorial
typographic moments, one decisive warm accent. Restrained and confident —
nothing cluttered. The raven/vegvísir logo is the brand soul: used with
respect, never busy.

**Brand assets (provided — in `assets/` folder):**
- `assets/hrafnar-logo.png` — this is the **Hrafnar Guides** logo (twin
  ravens + Norse/vegvísir staves). It is a REFERENCE for the brand family
  look, NOT to be reused as-is. Hrafnar Digital needs its OWN distinct mark
  that clearly belongs to the same family but is visibly different — e.g. a
  single stylized raven, a minimal raven-eye/wing geometric glyph, or a
  refined monogram, in the same off-white-on-charcoal Dark Premium style.
  Keep it simple, scalable, works as favicon. Deliver as inline SVG so it's
  crisp and recolorable. Do NOT copy the exact Guides twin-raven sigil.
- `assets/founder.png` — founder photo (Jiří Štaffa at his desk). Use in
  the Founder section. Apply a subtle dark grade/duotone so it harmonizes
  with the dark theme; tasteful, keep it natural.

**Logo usage:** Header = logo symbol (small, ~32–40px) + "HRAFNAR DIGITAL"
wordmark beside it. Footer = symbol + wordmark, smaller. Optional: very
faint, large logo as a background texture element in ONE section (e.g.
behind the closing CTA), low opacity, never competing with text.

**Color system (these are the brand colors — use exactly):**
```
--bg:      #0F1820   /* primary — deep Nordic blue-black, main background */
--surface: #16222B   /* lifted panels/cards (derived, adjust ± slightly)  */
--text:    #F2EFE9   /* warm off-white (matches the logo's cream)         */
--muted:   #8B97A1   /* cool grey for secondary text (derived)            */
--accent:  #FC4D02   /* brand orange — CTAs, hover, key emphasis ONLY     */
--line:    #243038   /* hairline borders/dividers (derived)               */
```
Use `--accent` (#FC4D02) sparingly and decisively: primary CTAs, link
hovers, one or two key highlights per section. It should feel like ember
against the cold dark — high impact because it's rare. Never large fills
of orange.

**Typography (brand fonts — commercial, licensed):**
- **Display / headlines / wordmark:** "Gascogne Serial"
- **Body / UI:** "Sarvatrik"
- Load via `@font-face` from `assets/fonts/` (woff2). The actual font
  files are NOT included and must be supplied by the client from their
  valid web licenses (Sarvatrik is by Universal Thirst; Gascogne Serial
  by Typodermic). Leave the `@font-face` rules in place with file paths
  `assets/fonts/gascogne-serial.woff2` and `assets/fonts/sarvatrik.woff2`,
  and add clearly-marked `<!-- PLACEHOLDER -->` notes plus robust
  fallbacks so the site looks good even before fonts are added:
  - Display fallback stack: `"Gascogne Serial", "Fraunces", Georgia, serif`
  - Body fallback stack: `"Sarvatrik", "Hanken Grotesk", system-ui,
    sans-serif`
  - Pull the two named free fallbacks (Fraunces, Hanken Grotesk) from
    Google Fonts so the unfinished site still looks intentional.
- Big confident display sizes, tight leading on headlines, comfortable
  leading on body. Editorial rhythm.

**Imagery:** High-quality Icelandic hospitality / landscape photography
(guesthouses, cozy restaurant interiors, northern lights, black sand,
Reykjavík, geothermal). Unsplash hotlink URLs or clearly-marked placeholder
slots with descriptive alt + a comment naming the intended subject. Apply a
consistent dark grade/duotone (toward the #0F1820 / slight #FC4D02 warmth)
so all imagery feels like one curated set.

**Motion:** Tasteful, curated. One orchestrated page-load with staggered
reveals. Scroll-triggered fade/translate via IntersectionObserver. Subtle
hover states with the accent. A very subtle raven/feather or aurora accent
motion is welcome if elegant. Respect `prefers-reduced-motion`.

---

## 4. Site structure (single page, anchored nav)

Sticky minimal header: HRAFNAR wordmark left, nav links right
(Services · Approach · Pricing · Results · Contact), a subtle accent CTA
button "Get a free audit".

### 4.1 Hero
- Full-viewport, dark, atmospheric Icelandic image with grade + subtle grain.
- Large editorial headline. Suggested copy (refine, keep the spirit):
  "Marketing that fills your rooms — and your tables."
- Sub-line: "We help Icelandic guesthouses, hotels and restaurants grow
  online. Websites, Google Ads, Booking, Airbnb, Bókun — handled end to end."
- Two CTAs: primary "Get a free audit", secondary "See what we do".
- Small trust strip under CTAs: "Performance marketing · 5+ years ·
  Dozens of clients · You pay for outcomes, not overhead".

### 4.2 Services
Editorial grid (NOT identical cards). SIX services:
1. **Websites & landing pages** — fast, conversion-focused sites that turn
   visitors into direct bookings.
2. **Performance marketing** — Meta Ads, Google Ads, Bing and more.
   Data-driven paid acquisition, real budgets, measurable return. (Founder's
   core strength — give it the most weight.)
3. **Social media** — content, presence and growth across the channels that
   matter for hospitality (Instagram, Facebook, etc.).
4. **Booking platform management** — Airbnb, Booking.com, Bókun, Expedia:
   optimized listings, pricing, visibility, reviews.
5. **Data & reporting** — full transparency: tracking setup, dashboards,
   clear monthly reporting. The client always has the complete picture.
6. **Consulting & business management** — strategy and end-to-end
   operational support for owners who'd rather run their business.
Each with a short, stoic, benefit-led description (strong verbs, no jargon).

### 4.3 Approach / Why Hrafnar
A short, confident section contrasting Hrafnar vs. typical local agencies:
- Performance & data first (not vanity metrics)
- International experience, transparent reporting
- Transparent, clear packages — premium work, every króna accounted for
- We understand Icelandic hospitality: seasonality, tourists, platforms
Present as a clean comparison or a tight value-points layout, not a boring
"about us" paragraph.

### 4.4 Pricing
Three tiers, monthly retainers, framed as fair and transparent (tied to real
hours), positioned quietly below typical local agency rates — but the lead
message is "pay for outcomes, not overhead", NOT "we're cheap".
- **Starter** — from 150,000 ISK/mo: ≈10 hours/month. One core channel
  managed (e.g. performance marketing OR platforms) + monthly reporting.
- **Growth** — from 300,000 ISK/mo: multi-channel — performance marketing +
  platforms + social, full dashboards + monthly strategy call.
- **Full Service** — custom: website + all channels + data & reporting +
  ongoing consulting. "We run your digital, you run your business."
Each tier: short stoic feature list, one CTA. Add: "No long contracts.
Cancel anytime." and "Pricing in ISK, VAT excluded. Tied to real hours —
you pay for results, not retainer padding." A subtle one-liner is allowed,
e.g. "Fair rates. Fairer than most. Always transparent." — understated,
never a discount shout.

### 4.5 Results / Testimonials
**Anonymized, no fake names, no fake faces.** Use the format:
> "Quote about results."
> — Guesthouse owner · North Iceland
3–4 short, credible, results-flavored quotes (e.g. more direct bookings,
lower commission dependence, better ad ROI). Mark clearly as
`<!-- PLACEHOLDER: replace with real testimonials when available -->`.
Optionally a small stats band: "+X% direct bookings · −Y% reliance on OTAs
· ROAS Z×" with placeholder numbers clearly marked.

### 4.6 Founder
Short, human section. Name: Jiří Štaffa. Performance marketer, 5 years in
the Czech market, data & campaigns focus, large budgets, dozens of happy
clients — now bringing that expertise to Iceland at local-friendly prices.
Use `assets/founder.png` with a subtle dark grade so it fits the theme.
Link the LinkedIn URL above (opens in new tab, rel="noopener noreferrer").
Keep copy first-person and grounded — the "Curator of the North" voice:
clear, no fluff, signal over noise.

### 4.7 Contact / CTA
Strong closing section. Headline like "Let's grow your business."
Simple form: name, business name, business type (select: guesthouse / hotel
/ restaurant / café / tour operator / other), email, message. Submit via
the placeholder endpoint described in section 2. Also show the direct email
**staffa.ppc@gmail.com** (real, use as the mailto: target) and the
LinkedIn link.

### 4.8 Footer
HRAFNAR wordmark, tagline, "Operated by a registered Ltd. company",
copyright year (dynamic via JS), nav repeat, LinkedIn. Keep it minimal
and on-brand.

---

## 5. Copywriting rules
- Language: **English** (primary audience: Icelandic hospitality owners +
  international stakeholders). Keep islandic-friendly, simple, confident.
- Tone: premium but warm, direct, no marketing fluff, no buzzword soup.
- Never overclaim. Spine: "International standard. Fair, transparent pricing. You pay for outcomes, not overhead." Do NOT call the service cheap or budget; never beg on price.
- Every section earns its place — cut anything generic.

## 6. Deliverables
1. Complete working site, runnable locally with a single command (or just
   open index.html if static).
2. Clean, commented code. CSS variables for the whole color/spacing system.
3. All placeholders clearly tagged for easy find-and-replace.
4. A short README: run locally, where to replace content/images/endpoint,
   how to deploy to Netlify or Vercel.
5. A brief list of what you'd improve with more time.

## 7. Explicit don'ts
- No fake reviewer names, faces, or fabricated company logos.
- No invented legal/registration numbers or false certifications.
- No generic AI-agency look (purple-on-white, Inter, identical cards).
- No backend, no tracking scripts beyond a clearly-marked optional GA4
  placeholder snippet (commented out).
