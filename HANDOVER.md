# Styal Lodge — Developer Handover Brief

_Last updated: handover v1. Keep this file up to date as the project evolves._

This document lets a new developer pick up the Styal Lodge website exactly where the
previous work left off, and — importantly — **continue with the same build habits**.
Read it top to bottom before touching anything.

---

## 1. What this is

A brand-new marketing website for **Styal Lodge**, a family-run wedding venue in
Cheshire (Station Road, Styal, Cheshire SK9 4JN). It replaces an older WordPress site.

- **Live preview:** https://styal-lodge-preview.netlify.app
- **Old site (for content + SEO reference):** https://www.styallodge.co.uk
- **Aesthetic:** calm, editorial, "Kinfolk" — spacious, understated, magazine-like.
  Information should be easy to reach without feeling busy or loud.

**The client (Oli) is non-technical.** All guidance to the client must be in plain,
step-by-step language. The client edits via the VS Code terminal and pushes to GitHub;
Netlify auto-deploys. Design decisions are made iteratively (build → review → refine),
not specified fully up front.

---

## 2. Brand — the single source of truth

There is an official **Styal Lodge Digital Brand Palette v1.0**. Everything must obey it.
These values are encoded as CSS custom properties in `src/styles/tokens.css` — **always
use the tokens, never hardcode a hex value.**

### Palette
| Role | Name | Hex | Token |
|---|---|---|---|
| Background (signature) | Oat | `#F4F1EA` | `--bg` |
| Headings + body text | Charcoal | `#3F3A36` | `--ink` / `--ink-strong` |
| Secondary / supporting copy | Stone | `#706962` | `--stone` (`--muted`) |
| Captions / category headings | Warm Taupe | `#A3968B` | `--taupe` |
| Primary accent / buttons | Sage | `#7B8573` | `--accent` |
| Button hover | Sage (dark) | `#6E7868` | `--accent-hover` |
| Secondary accent | Olive | `#818B78` | `--olive` |
| Card / quote background | Ivory | `#FBF9F5` | `--surface` |
| Footer / dark sections | Charcoal | `#3F3A36` | `--surface-dark` |
| Dividers / hairlines | soft | `#D8D2C8` | `--hairline` |
| Text on Sage button | White | `#FFFFFF` | `--on-accent` |

Rules from the brand doc, enforced in code:
- **Never pure black** (`#000000`) and **never grey** — warm Charcoal only.
- **Never pure white** as a background — Oat everywhere; Ivory for slight contrast.
- **Primary button** = Sage bg, white text, `#6E7868` hover.
- **Secondary button** (`.btn--ghost`) = transparent, Charcoal outline + text, hover =
  Charcoal bg / Oat text.

### Typography
- **One typeface: Larken** (Ellen Luff foundry), served via **Adobe Fonts**.
  - Web project (kit) ID: **`shu6iux`** — loaded in `src/layouts/Base.astro` via
    `<link rel="stylesheet" href="https://use.typekit.net/shu6iux.css">`.
  - Weights in use: Light (~300) body, Regular (400) display, plus **true italics**
    for accent words (e.g. _Experience_, _Feasting_, the tagline).
  - CSS family: `"larken", Georgia, serif` (see `--font-serif`).
- **Adobe Fonts domain allow-list:** Larken only renders on domains added to the
  `shu6iux` web project. `styal-lodge-preview.netlify.app` is already added. **When the
  real domain (styallodge.co.uk) goes live, add it too**, or type falls back to a serif.

### Image style (from brand doc)
Warm, soft contrast, creamy whites, muted greens, charcoal blacks, natural skin tones.
**Avoid** bright whites, blue shadows, high saturation, heavy HDR, strong sharpening.
> ⚠️ **Open decision:** the current hero uses a striking **black-and-white** confetti
> photo. This is deliberately kept for now but is *not* what the brand doc calls for
> (warm colour). The photography direction (warm colour vs B&W) is the next big call and
> will shape every section. Do not roll out lots of B&W imagery without confirming.

### Brand feel + the "Styal Test"
Quiet, editorial, spacious, relaxed, natural, understated, confident. Never busy, never
loud. Large margins, one focal point per section, max 2 typefaces, lots of breathing room.
**Before publishing any page, ask:** Oat background? Charcoal headings? Sage buttons?
Warm editorial photography? Enough white space? Could anything be removed rather than
added? If yes to all — it feels like Styal.

---

## 3. Tech stack

- **Astro** (static site generator) — chosen for component reuse (shared nav/footer) and
  built-in responsive image optimisation (WebP generation). Ships plain static HTML.
- **Node 20** (pinned in `netlify.toml`). Local dev needs Node LTS installed.
- **GitHub** (private repo) for version control: `Ogoldwait91/styal-lodge`.
- **Netlify** for hosting + CI. Every push to `main` auto-builds and deploys.
- **Netlify Forms** will handle the "Book a visit" enquiry form once live (the form in
  `book-a-visit.astro` already has `data-netlify` + honeypot wired).
- No React/heavy framework — keep it lightweight. Vanilla JS only, minimal, progressive.

---

## 4. Where everything lives

| Thing | Location |
|---|---|
| Code repo | GitHub → `Ogoldwait91/styal-lodge` (private) |
| Hosting / deploys | Netlify → project `styal-lodge-preview` |
| Live URL | https://styal-lodge-preview.netlify.app |
| Fonts | Adobe Fonts web project `shu6iux` (Larken) |
| Contact email for forms | info@styallodge.co.uk |

---

## 5. Local development

Prereqs: **Node.js LTS** and **VS Code** installed; a **GitHub** account with access to
the repo.

```bash
git clone https://github.com/Ogoldwait91/styal-lodge.git
cd styal-lodge
npm install          # one time (and whenever package.json changes)
npm run dev          # dev server at http://localhost:4321
npm run build        # production build into /dist (ALWAYS run before shipping)
```

Note: Larken may show as a fallback serif on `localhost` unless `localhost` is on the
Adobe Fonts allow-list — layout is still correct regardless.

---

## 6. Project structure

```
styal-lodge/
├── netlify.toml                # build command, publish dir (dist), Node 20 — do not remove
├── astro.config.mjs
├── package.json / package-lock.json
├── tsconfig.json               # extends astro strict
├── .gitignore                  # ignores node_modules, dist, .astro
├── HANDOVER.md                 # this file
├── public/                     # served as-is (absolute /paths)
│   ├── logo-dark.svg           # stacked wordmark, Charcoal + Stone subtitle (hero)
│   ├── logo-light.svg          # stacked wordmark, Oat (dark footer / reversed)
│   ├── logo-wordmark.svg       # horizontal single-line lockup (header, fades in)
│   └── favicon.svg
└── src/
    ├── assets/
    │   └── hero.jpg             # optimised by Astro's <Image> at build
    ├── styles/
    │   ├── tokens.css           # ⭐ palette, type scale, spacing — single source of truth
    │   └── global.css           # reset, utilities (.container/.btn/.eyebrow/.display), a11y
    ├── layouts/
    │   └── Base.astro           # <head> (Adobe Fonts link), Nav, <slot/>, Footer, FloatingCta
    ├── components/
    │   ├── Nav.astro            # sticky header, nav links, header logo fade-in, mobile menu
    │   ├── Footer.astro         # Charcoal footer, reversed logo, contact, socials
    │   ├── FloatingCta.astro    # "Book a visit" pill, reveals on scroll
    │   └── Hero.astro           # ⭐ separate desktop + mobile hero (see §7)
    └── pages/
        ├── index.astro          # single scroll page: Hero + anchored sections
        ├── book-a-visit.astro   # enquiry form (Netlify Forms ready)
        └── details-pricing.astro# pricing page (placeholder)
```

---

## 7. Key design decisions & conventions (read before editing the hero/nav)

**Design tokens.** Change colour/spacing/type in `tokens.css` and it flows everywhere.
Never hardcode hexes in components.

**Two distinct hero designs — this is intentional, do not "unify" them:**
- **Desktop (≥64em):** full-bleed, one-screen hero. Cream/oat masthead with the big
  Charcoal `STYAL LODGE` wordmark above a full-width photo that flexes to fill the
  remaining viewport height; italic tagline sits _on_ the image bottom-right. The client
  signed this off — leave it.
- **Mobile (≤64em):** the "editorial page" model (Kinfolk-style). Oat background, big
  Charcoal wordmark in generous space, a **contained, framed image** (4:5, side margins —
  _not_ full-bleed, no text over it), then a Stone italic tagline and one Sage
  "Book a visit" button below. All in `Hero.astro` under the `@media (max-width: 64em)`
  block.

**Header logo fade-in.** The header shows only the menu/links at the top; the small
horizontal `logo-wordmark.svg` **fades into the header centre once the big hero wordmark
has scrolled up behind the bar.** Implemented in `Nav.astro` with a deterministic scroll
measurement (`getBoundingClientRect().bottom <= header.offsetHeight`) — **not** an
IntersectionObserver (an earlier IO version showed the logo at the top on iOS, causing a
duplicate wordmark; do not revert to it). On inner pages (no hero) the header logo shows
always.

**Header on mobile** is a normal solid Oat sticky bar (menu button opens a full-screen
Oat menu containing the CTAs + nav links). It is _not_ transparent/over-photo — that was
tried and dropped when the mobile hero became the oat editorial page.

**Accessibility baked in:** skip-link (hidden until focused), `aria-expanded` on the menu
toggle, `focus-visible` outlines, `prefers-reduced-motion` guards, `scroll-margin-top` so
anchor links clear the sticky header. Preserve these.

**Nav labels / sections:** header + footer = The Experience, Feasting, Stories, FAQs
(anchors on the home page) plus Book a visit + Details & Pricing (separate pages).

---

## 8. Build & deploy workflow — **follow these habits exactly**

The whole point of this section: a new dev should ship changes the same reliable way.

**The loop (every change):**
```bash
# 1. make the edit in VS Code
npm run build            # 2. ALWAYS confirm it builds locally first
git add -A               # 3. stage everything (.gitignore keeps node_modules out)
git commit -m "Clear, descriptive message"   # 4. commit
git push                 # 5. push — Netlify auto-deploys in ~1 min
```
- `git status` should **never** list `node_modules/`, `dist/`, or `.astro/`. If it does,
  the `.gitignore` isn't being respected — stop and fix before committing.
- Netlify rebuilds automatically on push to `main`. Watch the deploy go green.
- The `main` branch is production. For anything risky, use a feature branch + PR —
  Netlify gives a preview URL per PR.

**Netlify build settings** (already set; recorded in `netlify.toml` so they can't drift):
- Build command: `npm run build`
- Publish directory: `dist`  ← _the #1 gotcha. If the site 404s, this is almost always
  the cause (Netlify publishing the repo root instead of `dist`)._
- Node version: `20`

**Verification habit (learned the hard way — do not skip):**
When you programmatically edit files (scripts, find-and-replace), a non-matching
find/replace can **silently do nothing**. Always verify the change actually landed:
```bash
grep -c "the-thing-you-added" src/path/to/file      # source has it
npm run build                                        # it compiles
grep -rl "the-thing-you-added" dist/                 # it's in the built output
```
If you hand the client a zip, also confirm the change is inside the zip before sending.
Symptom of a missed edit: the client copies your files and `git status` says
"nothing to commit" — that means your delivered files were identical to what's deployed.

**How files are delivered to the non-technical client:**
The client is given a full-project **`Styal Lodge.zip`**. They must **not** delete/replace
the whole working folder (it holds the hidden `.git` link to GitHub/Netlify). Instead:
unzip → open the folder → copy the _contents_ → paste over the real working folder →
"Replace files in destination" → then `git add -A && git commit && git push`.
The "LF will be replaced by CRLF" warnings on Windows are harmless — ignore them.

**Netlify visibility:** the deployed site was switched to **public** (Netlify defaults to
private now). Keep it public so links are shareable and Adobe Fonts can load.

---

## 9. Current status

**Done & live:**
- Full brand palette applied site-wide (desktop + mobile).
- Desktop hero (full-bleed one-screen) — signed off by client.
- Mobile hero (oat editorial page) — signed off, no duplicate logo, logo fades in on scroll.
- Header (logo fade-in), footer, floating "Book a visit", skip-link, mobile menu.
- Logos regenerated in exact brand colours (Charcoal wordmark, Stone subtitle).
- Book a visit page with Netlify-ready form; Details & Pricing placeholder page.
- GitHub + Netlify pipeline live and auto-deploying.

**Placeholders (not built yet):**
- Home sections: The Experience, Feasting, Stories, FAQs (currently eyebrow + heading +
  one placeholder line each in `index.astro`).
- Details & Pricing page content.

---

## 10. Roadmap / next steps (in rough order)

1. **Photography direction decision** (warm colour vs B&W) — blocks section design.
2. **The Experience** section — the wedding-day narrative (morning → ceremony → garden
   drinks → wedding breakfast → golden hour → the party). Content can be lifted/polished
   from the client's "The Styal Journal" brochure. Build editorial, per the Styal Test.
3. **Feasting** — Follow the Smoke catering; menu tiles (Seasonal / Fire / Sharing /
   Woodfire Pizza); The Styal Table tasting evening.
4. **Stories** — real weddings / testimonials / galleries.
5. **FAQs** — accordion of common questions (pull real answers from the client).
6. **Details & Pricing** page — exclusive use, what's included, hire fees.
7. **Wire the enquiry form** to email `info@styallodge.co.uk` (Netlify Forms + spam).
8. **SEO migration** — the old WordPress site ranks. Before pointing the domain over,
   set up **301 redirects** from old URLs (`/weddings`, `/stories`, `/gallery`,
   `/brochure`, `/contact`, etc.) to the new anchors/pages. Add sitemap + meta.
9. **Custom domain** — point `styallodge.co.uk` at Netlify; add it to the Adobe Fonts
   `shu6iux` allow-list; verify on phones + tablets.
10. Optional: brochure (Journal PDF) download; analytics; cookie/privacy page.

---

## 11. Content + assets reference

- Copy source: **"The Styal Journal"** brochure (client-supplied) — most section wording
  can be adapted from it.
- Real contact details: Styal Lodge, Station Road, Styal, Cheshire SK9 4JN ·
  info@styallodge.co.uk · 01625 416 376 · Instagram + TikTok.
- Catering partner: **Follow the Smoke**. In-house stylist: **Emma, Special Days**.
- Accommodation partners: The Stanneylands Hotel, Dakota.
- Registrar: Cheshire East Registration Service (bookings up to ~2 years ahead; the
  enquiry form's wedding-year dropdown offers 2027, 2028, 2029 "subject to availability").

---

## 12. Golden rules (TL;DR)

1. Obey the brand palette; use tokens, never hardcode colours; never grey/pure black/pure white.
2. Keep it calm and spacious — pass the Styal Test on every page.
3. Desktop and mobile heroes are intentionally different designs — don't merge them.
4. `npm run build` before every push; never commit `node_modules`.
5. Verify edits actually landed (source → build → output/zip) before shipping.
6. Client is non-technical: plain-language, step-by-step instructions; deliver full zips;
   never have them wipe the working folder (protect `.git`).
7. Add new domains to the Adobe Fonts `shu6iux` project or Larken won't load.
8. If the live site 404s, check Netlify publish dir = `dist` first.
