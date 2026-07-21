# Free Intro Session Landing Pages — Design

**Date:** 2026-07-21
**Goal:** Lead-magnet landing pages for a free intro session for rising seniors
(class of 2027) applying to college, timed to the Common App opening August 1,
2026. Shared in Facebook parent groups. Primary sales target: the College Essay
Intensive.

## Decisions (settled with Sarah)

- **Two variants**, identical except the locality section and title/meta. URLs
  are topic-specific so a fall academic-tutoring intro can slot in later
  (`/intro/academic-tutoring/…`):
  - `/intro/college-essays/westchester/index.html` — Pleasantville native,
    Hackley graduate, travels to Westchester for in-person sessions (already
    true per homepage FAQ)
  - `/intro/college-essays/brooklyn/index.html` — Greenpoint resident, CUNY PhD
    student, teaches at Brooklyn College
- Both lead with **"Former Assistant Director, University of Chicago Writing
  Program"** as the headline credential.
- **Audience:** parents (they hold the money; these are parent groups). Copy makes
  clear students and their families are all encouraged to attend.
- **Session format:** a few scheduled group dates (registration via MailerLite),
  with Sarah's existing bookable 1:1 Calendly intro as the fallback CTA at the
  bottom.
- **Pricing emphasis:** Intensive-first. The College Essay Intensive
  ($1,400 / $2,000, available through January 15) is the centerpiece;
  per-session rates listed quietly below it.

## Page structure (both variants)

Self-contained HTML with inline CSS per house rules; full design-system landing
page (paper, dinkuses, torn deep band) but a **slim masthead instead of the site
nav** — minimal exit links. Footer links back to bonannoworkshop.com.

1. **Hero** — mono kicker (free session · rising seniors · class of 2027);
   Fraunces h1 with squiggle; credential line under it; sub-copy hung on the
   Common App opening Aug 1 (Sarah's wording); button → #register.
2. **Locality line** — the variant section (Sarah's wording).
3. **What the session covers** — Sarah's wording + students-and-parents-attend
   line.
4. **Registration form** — clip-out coupon motif (as /worksheet). Fields: parent
   name, email, date choice (styled radios → MailerLite custom field). Posts to a
   **new dedicated MailerLite form/group** (separate from newsletter),
   `target="_blank"` jsonp post like existing forms.
5. **About Sarah** — headshot + credentials block ported verbatim from homepage.
6. **Intensive band** — torn `--paper-deep` band mirroring homepage `#intensive`:
   description, $1,400 / $2,000 tiers, available through January 15.
7. **Secondary pricing** — brief `.menu-line` rows with per-session rates
   ($150 / $180).
8. **Testimonials** — 1–2 ported verbatim from homepage if college-essay
   relevant (Claude proposes, Sarah vetoes).
9. **Calendly fallback** — "Can't make these times?" → 1:1 intro link.
10. **Slim footer** — main site + newsletter links.

## Head/meta requirements

Per STYLEGUIDE §9: real title + meta description per variant, OG/Twitter tags
(og-image.png), favicon, GA `G-KL55FCMMMT`, MailerLite Universal. **`noindex`**
on both (Facebook-shared lead pages, like /worksheet).

## Content rules in force

- No invented copy or teaching language: session description, locality lines,
  and hero sub-copy get **clearly marked placeholders** until Sarah supplies
  wording. Pricing/credentials ported verbatim from homepage.

## Materials gating final build (from Sarah)

1. Session dates/times, duration, Zoom confirmation
2. Session description in her words (+ title)
3. Calendly link for the 1:1 intro
4. New MailerLite form embed code (form action URL + field names; date custom
   field)
5. Westchester locality sentence (new copy — Hackley/Pleasantville not on site)
6. MailerLite confirmation automation (her dashboard; can trail launch prep)

## Testing

STYLEGUIDE §7: 390 / 768 / 1000 / 1440 px, `prefers-reduced-motion`, contrast,
no horizontal overflow, decorative SVGs `aria-hidden`, form `aria-label`s.
