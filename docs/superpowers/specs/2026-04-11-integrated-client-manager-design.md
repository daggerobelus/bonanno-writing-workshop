# Bonanno Writing Workshop: Integrated Client Manager

## Overview

A single-file HTML app that replaces the separate templates (invoice, welcome packet, session summary, receipt) and income tracker spreadsheet with one unified tool. Runs locally in the browser, saves data to a JSON file on disk.

## What It Replaces

- `templates/invoice.html` — generated automatically from session data
- `templates/session-summary.html` — generated automatically from session data
- `templates/receipt.html` — generated automatically when payment is recorded
- `freelance_income_tracker.xlsx` — replaced by the app's built-in tracking
- `templates/welcome-packet.html` — KEPT as a standalone file (sent to prospective clients before they're in the system)

## File Location

- App: `/Users/sarahbonanno/Documents/Freelance/bww-manager.html`
- Data: `/Users/sarahbonanno/Documents/Freelance/bww-data.json` (auto-saved)
- Welcome packet: `/Users/sarahbonanno/Documents/Freelance/templates/welcome-packet.html` (unchanged)

## Data Persistence

**Primary:** File System Access API — on first launch, user picks a save location (bww-data.json in Freelance folder). Auto-saves on every change. Works in Chrome/Edge.

**Fallback:** If browser doesn't support File System Access (Safari), uses localStorage + manual export/import buttons.

**Safety:** "Export Backup" button always available regardless of mode.

## Design System

Same as bonannoworkshop.com:
- Colors: Navy #1c2a3a, Gold #c9a84c, Cream #f8f5ef, White #ffffff
- Fonts: Cormorant Garamond (headings) + Inter (body) via Google Fonts
- Clean, minimal UI — not a busy dashboard

## Core Screens

### 1. Dashboard (home)

The landing screen. Clean summary of the business.

**Top row — key metrics:**
- YTD Revenue (total paid)
- Tax Reserve (20% of YTD revenue)
- Net After Reserve
- Outstanding (unpaid invoices)

**Below:**
- Monthly revenue bar or list for current year (Jan–Dec, with amounts)
- Quarterly tax summary (Q1–Q4, with 20% reserve per quarter)
- Active clients count

### 2. Clients

List of all clients with status badges (Active/Inactive).

**Each client shows:** student name, parent name, service type, rate, package info.

**Add/edit client form fields:**
- Student name
- Parent name
- Email
- Service type (Academic Tutoring / Application Essays)
- Rate ($150 or $180, editable)
- Package type (Per Session / Monthly / Semester)
- If package: sessions in package, package start date, auto-calculated expiry (6 weeks for monthly, 20 weeks for semester)
- Notes

**Client detail view:** clicking a client shows their session history, invoices, total spent, package balance.

### 3. Sessions

Log of all sessions, newest first.

**Log a session (the primary action):**
- Select client (dropdown)
- Date (defaults to today)
- Duration (defaults to 1 hr, editable in 0.25 increments)
- Service type (auto-filled from client, editable)
- Rate (auto-filled from client, editable)
- Notes (what was covered)

When a session is logged:
- Amount is calculated (duration × rate)
- If client has a package, sessions used/remaining auto-update
- Data auto-saves

**Session list:** date, student, duration, amount, notes preview.

### 4. Invoices

List of all invoices with status (Unpaid / Paid / Overdue).

**Generate invoice:**
- Select client
- Select date range (or "uninvoiced sessions")
- App auto-pulls all sessions in range
- Invoice number auto-generated (BWW-YYYY-NNN, sequential)
- Due date auto-set (invoice date + 15 days)
- Package discount auto-applied if applicable

**Invoice detail view:**
- Full branded invoice matching the template design
- "Mark as Paid" button → records payment date, method (Venmo/Zelle dropdown)
- "Print / Save PDF" button → opens print dialog
- When marked paid, a receipt is auto-generated

### 5. Documents (print views)

When generating a document for print/PDF, the app renders it in the exact branded template style (navy header, gold accents, etc.) in a new view/window optimized for Cmd+P.

**Document types:**
- Invoice — full branded invoice
- Session Summary — for a specific session, pre-filled with all data
- Receipt — auto-generated when invoice is marked paid

These match the visual design of the standalone templates we already built.

## Tax Tracking

Integrated into the dashboard:

- **YTD revenue** — sum of all paid invoices in current calendar year
- **20% tax reserve** — what should be in savings
- **Quarterly breakdown:**
  - Q1 (Jan–Mar): revenue + 20% reserve
  - Q2 (Apr–Jun): revenue + 20% reserve
  - Q3 (Jul–Sep): revenue + 20% reserve
  - Q4 (Oct–Dec): revenue + 20% reserve
- **Estimated quarterly tax payment** — 20% of that quarter's revenue

## Pre-loaded Data

Migrate Will Pearlman's existing data:
- Client: Will Pearlman, parent Alice Pearlman, Academic Tutoring, $150/hr, Per Session, Active, "Sundays, Pleasantville"
- 8 sessions: Jan 11, Jan 18, Jan 25, Feb 1, Feb 15, Feb 22, Mar 1, Mar 15 — all 1hr at $150, all paid

## Navigation

Simple tab/sidebar navigation:
- Dashboard
- Clients
- Sessions
- Invoices

No deep nesting. Everything is 1-2 clicks away.

## What This App Does NOT Do

- Online/cloud sync — local only
- Payment processing — Venmo/Zelle handled outside the app
- Email sending — Sarah copies/pastes or prints to PDF and emails manually
- Welcome packets — those stay as the standalone template
- Referral tracking — removed per Sarah's request
- CRM features — no lead tracking, just active clients

## Package Policy Reference

- Monthly (4 sessions): expires 6 weeks from purchase, upfront payment required
- Semester (16 sessions): expires 20 weeks from purchase, upfront payment required
- No refunds on expired sessions
- Packages may be shared between siblings
- Cancellation: 24hr notice required, 50% late cancel fee, 100% no-show fee

## Technical Notes

- Single HTML file, no build step, no dependencies beyond Google Fonts
- All JavaScript vanilla (no frameworks)
- CSS custom properties for the design system
- Responsive but primarily designed for desktop/laptop use
- Print styles on document views hide app chrome, show only the document
