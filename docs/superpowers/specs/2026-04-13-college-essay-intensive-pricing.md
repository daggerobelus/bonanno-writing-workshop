# College Essay Intensive — Pricing Design

## Overview

Two fixed-price intensive packages for college application essay support, offered June–January 15, limited to 4 students per season (shared pool across both packages).

---

## Packages

### Common App Bootcamp — $1,400

- 5 one-hour sessions
- Written feedback on up to 5 drafts — included
- Additional draft reviews: $50 each
- **Guarantee:** a final, polished Common App essay
- *Contingent on timely draft submission and attendance at all scheduled sessions*

### Full Application Workshop — $2,000

- 7 one-hour sessions
- Written feedback on up to 8 drafts — included
- Additional draft reviews: $50 each
- **Guarantee:** a final, polished Common App essay + up to 3 supplemental essays
- *Contingent on timely draft submission and attendance at all scheduled sessions*

---

## Availability

- Available June – January 15
- **4 spots per season — shared pool across both packages**
- Accepting reservations for Summer 2026

---

## Pricing Rationale

| Package | Sessions | Draft Reviews | Raw Value | Price | Effective Rate (at 30 min/review) |
|---|---|---|---|---|---|
| Common App Bootcamp | 5 × $180 = $900 | 5 × $75 = $375 | $1,275 | $1,400 | ~$187/hr |
| Full Application Workshop | 7 × $180 = $1,260 | 8 × $75 = $600 | $1,860 | $2,000 | ~$182/hr |

Both packages price at or near the $180/hr target rate. The modest premium over raw value covers the structured timeline, scarcity, and guarantee. Prices should be revisited after the first 2–3 intensive clients.

---

## Additional Draft Reviews

- $50 per review (discount from $75 standalone async rate)
- Applies to both packages when a student exceeds the included draft count
- Logged in the manager app as "Intensive Draft Review — Additional"

---

## Payment Terms

- **Deposit:** $300 non-refundable, due at reservation — secures the spot and is applied toward the total
- **Remainder:** due before the first session
- Applies to both packages

**Fine print language:**
> *A $300 non-refundable deposit is required to reserve your spot. Remaining balance due before your first session.*

**On the site:** Displayed as fine print directly below the Reserve Your Spot button, alongside the existing guarantee contingency note.

---

## Site Fixes Required

- [ ] Update "Additional draft reviews at $75" → $50 on the current listing
- [ ] Split current single $1,200 card into two sub-packages: Common App Bootcamp and Full Application Workshop
- [ ] Move "4 students per season" to a shared line above both package cards

---

## App Updates (bww-manager.html)

Added to `PACKAGE_SPECS`:
- `Common App Bootcamp`: 5 sessions, fixed price $1,400, 28-week window
- `Full Application Workshop`: 7 sessions, fixed price $2,000, 28-week window

Added session service types:
- `Intensive Draft Review — Included` · $0 (tracks included reviews)
- `Intensive Draft Review — Additional` · $50 (charges for overages)

Fixed-price packages do not recalculate amount when rate changes.
