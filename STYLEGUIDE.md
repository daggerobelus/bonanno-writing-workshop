# Bonanno Writing Workshop — Design System

**"The draft on the desk."** Every page reads as a working manuscript that a good
teacher has marked up: warm paper, ink text, red-pencil annotations, typewriter
labels, handwritten margin notes. Shipped July 2026, replacing the navy/gold design.

This is the canonical reference for anyone (human or agent) building new pages or
documents for bonannoworkshop.com or restyling the bww-manager app. Match it exactly —
don't improvise new colors, fonts, or motifs without Sarah's sign-off.

---

## 1. Tokens

Copy this block into any new page. Pages on this site are **self-contained HTML files
with inline CSS** — no shared stylesheet, no frameworks, no build step.

```css
:root {
    --paper: #faf5ea;         /* page background (warm paper) */
    --paper-deep: #f2ead7;    /* deeper band sections (intensive, testimonials) */
    --paper-card: #fffdf6;    /* cards / sheets that sit on the paper */
    --ink: #262117;           /* primary text; also the footer background */
    --ink-soft: #5c5443;      /* secondary text */
    --ink-faint: #8a8171;     /* metadata, fine print */
    --pencil: #a83c28;        /* THE accent: teacher's red pencil */
    --pencil-soft: rgba(168, 60, 40, 0.55);
    --pencil-faint: rgba(168, 60, 40, 0.22);
    --rule: rgba(38, 33, 23, 0.14);   /* hairlines (0.16 on cards is also used) */
    --serif: 'Fraunces', Georgia, serif;
    --body: 'Newsreader', Georgia, serif;
    --hand: 'Caveat', cursive;
    --mono: 'Courier Prime', 'Courier New', monospace;
}
```

Newsletter archive accent rotation (`/writing-tips` cards only): `--teal: #2f6f62`,
`--sienna: #b35a2d`, `--gold: #b08d3c`. Inside the letters themselves the accent is
uniformly `--pencil`.

**Paper texture** — the body gets an SVG noise overlay (see any page's `body`
`background-image`, a `feTurbulence` data URI at `opacity='0.05'`).

## 2. Type

Google Fonts load (one link, all four families):

```
https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;0,9..144,600;1,9..144,400;1,9..144,500&family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,500;1,6..72,400&family=Caveat:wght@500;600&family=Courier+Prime:ital,wght@0,400;0,700;1,400&display=swap
```

| Role | Font | Usage |
|---|---|---|
| Display / headings | **Fraunces** 500–600 | h1 `clamp(44px, 5.6vw, 68px)` on the homepage hero; section h2 34px; card h3 19–24px. Italic Fraunces for emphasis words and pull-quotes. |
| Body | **Newsreader** 400/500 | 17.5px, line-height 1.75. Secondary text in `--ink-soft`. |
| Labels / meta | **Courier Prime** 700 | `.type-label`: 12.5px, `letter-spacing: 0.22em`, uppercase, `--pencil`. Softer meta (`.type-meta`): 13px, `--ink-soft`. Buttons also use Courier. |
| Handwriting | **Caveat** 600 | `.hand-note`: ~20–22px, `--pencil`, always slightly rotated (`rotate(-1.5deg)` to `-4deg`). This is "Sarah's pencil" — margin notes, asides, insertions. |

## 3. Signature motifs (copy from index.html)

- **Squiggle underline** — hand-drawn SVG under one key phrase per heading, stroke
  `--pencil`, animates via `stroke-dashoffset` on reveal. Class `.squiggle`. Use on
  at most one phrase per page section.
- **Dinkus** — three hand-drawn asterisks (SVG, `--pencil-soft`, whole thing rotated
  −2°) marking paper-to-paper section breaks. Class `.dinkus`. Vary each instance's
  per-asterisk rotations slightly.
- **Ledger section head** — `.section-head`: mono `§ N` number in pencil + Fraunces h2,
  hairline bottom border with a 64px × 3px `--pencil-soft` overstrike at the left.
- **Hand notes** — `.hand-note`/`.margin-note` Caveat asides. Wording must sound like
  Sarah's real feedback voice (see §6).
- **Torn paper edges** — `.torn` band edges via a jagged SVG strip in `--paper`,
  repeated-x, flipped (`scaleY(-1)`) for the bottom edge. Used on `--paper-deep` bands.
- **Proof-mark watermarks** — `.proofmark`: oversized ¶ / ✓ / ^ / "stet." at
  `opacity: 0.05`, absolutely positioned in section corners, `display: none` ≤1100px.
- **Sheets** — `.sheet`: `--paper-card` card, 1px `--rule` border, slight rotation
  (±0.3–2°), soft double shadow, optional masking-tape pseudo-element on top.
  Cards never use border-radius beyond 2px; this design is square-cornered.
- **Clip-out coupon** — `.clipout`: dashed `--pencil-soft` border with a rotated ✂
  scissors glyph breaking the top edge. Used for signup forms (/subscribe, /worksheet,
  homepage lead magnet).

## 4. Components

- **Buttons** (`.btn`): Courier 700, uppercase, letterspaced; solid `--ink` or
  `--pencil` background with paper text; **hard offset shadow** `3px 3px 0
  var(--pencil-faint)` that grows on hover (translate −1px,−1px) and shrinks on
  :active. Ghost variant: transparent with `--ink-soft` border. Radius 2px.
- **Links**: `.link-under` — mono uppercase with a 2px `--pencil-soft` bottom border.
  In-copy links are `--pencil`.
- **Forms**: write-on-the-line inputs — no boxes; `border-bottom: 1.5px solid
  var(--ink-soft)`, focus → `--pencil`. Textareas get ruled-line backgrounds
  (repeating-linear-gradient). Labels in mono uppercase.
- **Nav**: sticky, blurred paper background, hairline bottom + 3px-gap second hairline
  (`.nav-wrapper::after`). Brand: `Bonanno <em>Writing</em> Workshop` in Fraunces, the
  italic word in `--pencil`. Links in mono uppercase. Mobile: text "Menu" toggle,
  links in a dropdown panel.
- **Footer**: solid `--ink`, paper-toned text, Fraunces brand, mono labels.
- **Price/menu rows**: `.menu-line` — name + `(72hr)` mono aside + dotted leader +
  Fraunces price + pencil arrow.
- **Carousels** (testimonials, hero draft-card): grid-stacked slides (`grid-area: 1/1`)
  fading via `.is-active`; fade out fully before fade in (`transition-delay` on the
  incoming slide) so text never overlaps text. Auto-advance 7–10s, pause on hover.
- **Scroll reveal**: `.reveal`/`.revealed` opacity+translateY via IntersectionObserver.
  Always honor `prefers-reduced-motion` (show everything, kill animations).

## 5. Page anatomy

- Content container `.ms`: max-width 1100px, `padding: 0 40px` (28px ≤960px).
- **Never set a shorthand `padding:` on a section that carries `.ms`** — it wipes the
  horizontal padding (this bug shipped once). Use `padding-top` / `padding-bottom`.
- Section rhythm on the homepage: paper → paper (dinkus between) → deep band (torn
  edges) → paper… Vertical padding ~90–110px top-level, 60–80px on secondary sections.
- `body { overflow-x: clip }` — rotated sheets otherwise cause sideways scroll.
- Standalone documents (newsletter issues, worksheet, subscribe) are centered "sheets"
  on a `#e9e2d2` desk backdrop, no site nav — just a masthead with the brand and a
  mono label, over a 1px ink rule + 2px `--pencil-faint` double rule.

## 6. Content rules (important)

- **Copy belongs to Sarah.** Never rewrite, tighten, or "improve" her site copy,
  testimonials, or newsletter text. Port verbatim. Trimming for a layout requires
  flagging it to her.
- **Pedagogy is hers alone.** Handwritten notes, worksheet language, example claims,
  and anything that sounds like teaching must come from her own materials (worksheet,
  newsletter issues, the "Imposter Claims" deck) — never invented. When in doubt, use
  placeholder text and ask her to supply wording. She's an experienced teacher; do not
  add teaching suggestions.
- Facts that must stay accurate: rates ($150/$180 per session; intensive $1,400/$2,000;
  async review $75/$110/$100/$150), package rules (monthly & semester packages include
  free rescheduling; semester adds 4 async essay reviews), payment methods, the
  guarantee footnotes.
- Newsletters: monthly "Advice from the Workshop"; issues live at
  `/writing-tips/issue-NN.html` and follow the issue template (see issue-01 for the
  canonical structure; 02 and 03 for the callback and spectrum variants).

## 7. Accessibility & quality bar

- Contrast: `--ink` and `--ink-soft` on paper both pass. Buttons are paper-on-ink or
  paper-on-pencil (both pass). Never put white text on gold-like mid tones — the
  failure mode of the old design.
- All decorative SVGs get `aria-hidden="true"`. Forms keep `aria-label`s.
- `prefers-reduced-motion: reduce` must fully disable animation on anything animated.
- Test at 390px, ~768px, ~1000px, 1440px. Watch for: nowrap marked-up spans
  overflowing (use `text-decoration: underline wavy` via `.mark-wavy` for long
  phrases), absolutely-positioned notes colliding with text.

## 8. Print / PDF documents

The worksheet (`worksheet-main-claim.html` → PDF via headless Chrome
`--print-to-pdf --no-pdf-header-footer`) is the model: `@page { size: letter; margin:
0.45in }`, tightened print font sizes, `print-color-adjust: exact`, explicit
`page-break-before` between sheets. Verify page count after any content change.

**bww-manager app** (separate repo, hosted on Cloudflare): fully rebranded July 2026.
The admin UI runs on remapped legacy token names (`--navy` now holds ink, `--gold`
holds pencil, etc. — see its `:root`) with Fraunces replacing Cormorant for headings;
client-facing documents (session summary, invoice, receipt) additionally get a scoped
override block — search "CLIENT-FACING DOCUMENTS" in its index.html.

## 9. Site inventory

| Path | What it is |
|---|---|
| `/` (index.html) | Homepage; animated "imposter claims" hero card (6 slides, content from Sarah's deck) |
| `/writing-tips/` | Newsletter archive; rotating accent card spines |
| `/writing-tips/issue-NN.html` | Individual letters (standalone sheets) |
| `/subscribe/`, `/worksheet/` | Clip-out signup pages (MailerLite form posts) |
| `worksheet-main-claim.html/.pdf` | The lead-magnet worksheet document (unlinked; PDF uploaded to MailerLite) |
| `/welcome/academic-tutoring/` | Client welcome packet — **still old navy/gold**, restyle pending |
| `/newsletter/` | Redirect to /writing-tips |

Head requirements for every public page: real `<title>` + meta description, OG/Twitter
tags (og-image.png — note: still the old-brand image, refresh pending), favicon
(`/favicon.ico` — also old-brand, refresh pending), Google Analytics `G-KL55FCMMMT`,
and MailerLite Universal on pages with signup forms. `noindex` only on /worksheet and
utility documents.
