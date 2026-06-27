# GEN C — Generation Code · Current State
_Last updated: 2026-06-06_

---

## Project overview

Single-file HTML landing page for **evisions** internal KPI showcase — **"GEN C: Generation Code"**.
Apple-style scroll-animated presentation of 3 AI creative KPIs. Czech language.

**File:** `gen_c_website/index.html`
**Preview server:** `genc` config in `presentation/assets/.claude/launch.json` → port **5511**

---

## Tech stack

- Self-contained HTML/CSS/JS (no build step, no frameworks)
- Google Fonts CDN: Inter Tight / Manrope / Lora + VCR OSD Mono (cdnfonts.com)
- Scroll animations via `IntersectionObserver`
- Light/dark mode toggled by scroll position (dark hero → light after scroll)
- Matrix rain via `<canvas>` on hero + footer

---

## Design system

| Token | Value |
|---|---|
| Orange accent | `#E5712C` |
| Amber accent | `#E5A52C` |
| Lime | `#b4d250` |
| Dark bg | `#141414` |
| Cream bg (now white) | `#ffffff` |
| Dark-2 | `#1c1c1c` |
| Dark-3 | `#262626` |

**Fonts:** Inter Tight (headings/UI), Manrope (eyebrows/labels/buttons), Lora Regular (perex/body), VCR OSD Mono (hero subtitle animation)
**Signature:** orange `.` dot at end of key titles · 4px gradient bottom-bar · `body.light` class toggled on scroll past hero

---

## Light / Dark mode

- **Hero** — always dark (`background: var(--dark)` hardcoded)
- **Scroll past hero** → `body.light` class added → all sections transition to white `#ffffff`
- **Part 2 (PPC)** — fades to dark via `IntersectionObserver` adding `.dark-in` class (`background-color .9s` transition)
- **Nav dark override** — `body.light.dark-nav` class added when scrolled into Part 2 → nav reverts to dark styles
- Footer stays dark in both modes
- All transitions: `background-color .55s`, `color .45s`, `border-color .45s`

---

## Nav

| Element | Value |
|---|---|
| Brand mark | `brandmark_darkmode.png` / `brandmark_lighmode.png` (swap on `body.light`) |
| Brand text | `Gen C.` — orange dot inside `<span class="dot">` wrapped with text in single `<span>` to prevent flex gap |
| Sub-text | `Generation Code` — hidden on mobile ≤600px |
| CTA | `Schválit KPIs` — outline style, fills orange on hover; triggers fireworks + mailto on click |
| No nav links | removed |

---

## Hero

- H1: `Jsme Gen C.` — white, orange dot
- H2: `Generation Code` — VCR OSD Mono font, forth-and-back scramble animation loop
  - Start in Manrope → scramble (orange glyphs) → resolve to VCR (white) → hold 2.6s → scramble back → hold 1.8s → repeat
- CTA: `Chci mrknout na KPIs` — solid gradient button (orange→amber)
- Background: Matrix rain canvas (lime heads, amber→orange trail) + parallax orbs

---

## Structure — 3 chapters

### KAPITOLA 01 — Produktové fotky z nástroje Nightjar (removed label)

**Heading:** `Produktové fotky z nástroje Nightjar.` — "Nightjar" white, orange dot
**Showcase:** 6-col grid → 2-col at mobile — source photo + Nightjar logo (nighjar_logo.png) in pipe col + 4 generated photos (staggered slide-in on IntersectionObserver)

| File | Role |
|---|---|
| `vstupni_foto.png` | Input — 3 Yeahrba cans on white |
| `yeahrba-fruits.webp` | Generated — Minty Ananas, marble/fruits |
| `yeahrba-hand.webp` | Generated — Minty Ananas, hand studio |
| `yeahrba-gaming.webp` | Generated — Maracuja, neon gaming room |
| `yeahrba-original.png` | Generated — Original, park bench |
| `nighjar_logo.png` | Nightjar brand logo in pipe column |
| `nightjar-mockup.png` | Nightjar UI on laptop (floating mockup) |

**Below showcase:** "Jak to funguje?" — 3 numbered steps + `$0.16 / fotka` price pill + floating laptop mockup

---

### KAPITOLA 02 — Automatizovaná tvorba PPC bannerů (removed label)

**Heading:** `Automatizovaná tvorba PPC bannerů.` — "PPC bannerů" white, orange dot
**Scroll animation:** Section fades from white → dark via `IntersectionObserver` + `.dark-in` class
**Nav:** Reverts to dark when scrolled into this section (`body.light.dark-nav`)

**Showcase:** Two-tab client selector (FTMO first, then Yeahrba) + format dimension badges + single banner preview stage
- Badges: no border/stroke, semi-transparent bg, solid orange fill on click only
- Client tabs: same style as format badges

| Client | Formats |
|---|---|
| **FTMO** (default) | 1200×628, 480×300, 1080×1080, 1080×1920 |
| **Yeahrba** | 728×90, 300×250, 300×600, 160×600, 320×100 |

Files: `ftmo-*.png`, `yeahrba-*.png` in root folder (copied from `PPC banners/` subfolders)

**Jak to funguje?** — 4 steps with staggered `data-reveal` animations
**Right column:** `figma-mockup.png` — Figma screenshot with banner formats (floating mockup)

---

### KAPITOLA 03 — Redesign značky klienta (removed label, locked)

- No badge, no "Připravujeme" label
- Perex: `Návrh redesignu brandu pro vybraného klienta.`
- Before/After drag slider (placeholder)
- **"Aktuální stav mých KPIs."** headline above roadmap
- Roadmap boxes (no perex text): Produktové fotky (HOTOVO) · PPC bannery (HOTOVO) · Redesign značky (Připravuju)

---

## Footer

- Dark always, Matrix rain canvas background
- Text: `Přišlo ti to všechno aspoň trochu cool, Tome?` / `Klikni na kouzelné tlačítko.`
- CTA: `Schválit KPIs` — solid orange gradient button
- No meta/date line

---

## CTA behaviour — Schválit KPIs

Both nav + footer CTAs intercepted. On click:
1. Prevent default scroll
2. Launch **12-burst fireworks** (brand colors: orange, amber, lime, white) — 70 particles per burst, 4.2s total
3. Open `mailto:broz@evisions.cz?subject=Schvaluju KPIs` via `<a>` element click

---

## Responsive (≤600px)

- Nav: hide `<small>Generation Code</small>`, reduce padding, logo 26px
- Nightjar pipe col + logo: hidden (`display:none`)
- Photo grid: 2-col
- "Jak to funguje": stack vertically
- Format buttons: smaller font/padding
- Section padding: 48px

---

## JS — key functions

| Function | Purpose |
|---|---|
| scroll listener | Toggles `nav.scrolled` + `body.light` past hero + `body.dark-nav` in Part 2 |
| `IntersectionObserver` (revIO) | Scroll-reveal for `[data-reveal]` elements |
| `IntersectionObserver` (njIO) | Triggers Nightjar showcase: source bloom + gen cards slide in |
| `IntersectionObserver` (part2) | Adds `.dark-in` class to `#part2` on entry |
| VCR scramble loop | Hero subtitle: Manrope ↔ VCR OSD Mono, forth-and-back, loops forever |
| Matrix rain (hero) | Canvas `#matrix-canvas` — lime/amber/orange falling chars |
| Matrix rain (footer) | Canvas `#footer-matrix` — same effect |
| client-tab click | Switches FTMO/Yeahrba panels, resets format badges |
| `.fmt-btn` click | `switchBannerImg()` — fades out/in preview with new src |
| before/after slider | Mouse + touch drag on `#ba` — updates `clip-path` of `.after` layer |
| fireworks + mailto | 12-burst canvas animation, then opens `mailto:broz@evisions.cz` |

---

## Asset inventory

| File | Used in |
|---|---|
| `vstupni_foto.png` | Part 1 — Nightjar source |
| `yeahrba-fruits.webp` | Part 1 — generated #1 |
| `yeahrba-hand.webp` | Part 1 — generated #2 |
| `yeahrba-gaming.webp` | Part 1 — generated #3 |
| `yeahrba-original.png` | Part 1 — generated #4 |
| `nighjar_logo.png` | Part 1 — pipe column logo |
| `nightjar-mockup.png` | Part 1 — floating laptop mockup |
| `yeahrba-728x90.png` | Part 2 Yeahrba |
| `yeahrba-300x250.png` | Part 2 Yeahrba |
| `yeahrba-300x600.png` | Part 2 Yeahrba |
| `yeahrba-160x600.png` | Part 2 Yeahrba |
| `yeahrba-320x100.png` | Part 2 Yeahrba |
| `ftmo-1200x628.png` | Part 2 FTMO (updated) |
| `ftmo-480x300.png` | Part 2 FTMO (updated) |
| `ftmo-1x1.png` | Part 2 FTMO (updated) |
| `ftmo-9x16.png` | Part 2 FTMO (updated) |
| `figma-mockup.png` | Part 2 — "Jak to funguje?" mockup |
| `brandmark_darkmode.png` | Nav — dark mode logo |
| `brandmark_lighmode.png` | Nav — light mode logo |

---

## What still needs to be done

- [ ] **Part 3** — real redesign content (brand identity before/after, real client)
- [ ] **Real KPI data** — placeholder price `$0.16 / fotka`, steps are real
- [ ] **Fonts offline** — currently Google Fonts + cdnfonts CDN; embed base64 woff2 if needed offline
