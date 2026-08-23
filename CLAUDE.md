# bonannoworkshop.com

Static site for Sarah Bonanno's tutoring business (GitHub Pages; `CNAME` → bonannoworkshop.com). Self-contained HTML pages with inline CSS — no shared stylesheet, no framework, no build step.

**Before any design or page work, read `STYLEGUIDE.md`** — the canonical design system ("the draft on the desk": paper/ink/red-pencil, Fraunces + Newsreader + Courier Prime + Caveat). It includes non-negotiable content rules: never rewrite Sarah's copy, and never invent teaching language — handwritten notes and examples come from her own materials or from her directly.

Pushing to `main` deploys the live site. Do not push without Sarah's explicit go-ahead. The bww-manager repo (client app, Cloudflare-hosted) shares this brand for its client-facing documents — see STYLEGUIDE.md §8.

## Pre-push hook

`hooks/pre-push` (activated via `git config core.hooksPath hooks`) blocks any push where `index.html` has fewer than four `buy.stripe.com` checkout links — the essay-review pricing rows lost them in the July 2026 redesign and the break went unnoticed for six weeks. On a fresh clone, re-run `git config core.hooksPath hooks` once. If the pricing structure changes intentionally, update the expected count in the hook.
