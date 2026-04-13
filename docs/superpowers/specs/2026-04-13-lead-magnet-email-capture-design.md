# Lead Magnet + Email Capture Design

## Overview

A free downloadable worksheet ("Building Your Main Claim") used to capture email addresses and build a marketing list on bonannoworkshop.com. Delivered automatically via MailerLite on signup.

---

## Components

### 1. Worksheet PDF

- **Source:** Adapted from `bonanno-classroom/courses/research-paper/unit-3/worksheets/components-of-argument.html`
- **File:** `/Users/sarahbonanno/Documents/Freelance/templates/worksheet-main-claim.html`
- **Format:** Print-styled HTML → exported to PDF via Cmd+P
- **Design:** BWW brand system (navy, gold, Cormorant Garamond + Inter)
- **Content changes from classroom version:**
  - Remove course-specific framing — broaden to "your essay" (academic, college app, research paper)
  - Trim to 1–2 print pages
  - Add BWW header with logo
  - Add footer: *"Want guided support? Book a free 15-minute call at bonannoworkshop.com"*
- **Uploaded to MailerLite** as the incentive file for auto-delivery on signup

### 2. MailerLite Setup

- Free plan (up to 1,000 subscribers)
- One-time manual setup:
  - Upload PDF as lead magnet incentive
  - Create signup form: email field + "Send it to me" button
  - Configure auto-delivery of PDF on signup
  - Copy embed code for site integration
- Used for: list management, lead magnet delivery, future broadcasts and announcements

### 3. Site Integration

- **Location:** New section on bonannoworkshop.com, above the pricing section (first CTA on the page)
- **Content:**
  > **Free worksheet: Building Your Main Claim**
  > A step-by-step thesis builder for any essay — academic, college application, or research paper. Enter your email and I'll send it right over.
  > `[Email address]` `[Send it to me]`
  > *No spam. Unsubscribe anytime.*
- **Implementation:** MailerLite embed code dropped into index.html

---

## What This Does Not Include

- Welcome packet revisions — deferred, revisit when client types diverge meaningfully
- App integration — bww-manager.html is for active clients only; MailerLite list is separate
- Automated email sequences — add later once list has subscribers worth nurturing

---

## Manual Steps (Sarah, one-time)

- [x] Create MailerLite account
- [ ] Upload PDF to MailerLite as lead magnet
- [ ] Create and configure signup form
- [ ] Add existing client emails to list manually

---

## Files

- Worksheet template: `/Users/sarahbonanno/Documents/Freelance/templates/worksheet-main-claim.html`
- Site: `/Users/sarahbonanno/Documents/Freelance/bonanno-writing-workshop/index.html`
