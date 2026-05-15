# Marketing Strategy & Professional Templates Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a complete client management toolkit for Bonanno Writing Workshop — marketing strategy doc, 4 branded HTML templates (invoice, welcome packet, session summary, receipt), and an overhauled income tracker spreadsheet.

**Architecture:** All deliverables live in `/Users/sarahbonanno/Documents/Freelance/` (not in the website repo). HTML templates are single-file, self-contained documents using the same design system as bonannoworkshop.com (navy/gold/cream, Cormorant Garamond + Inter fonts). The spreadsheet is built with openpyxl for Excel compatibility. Logo images are copied from the website repo into a templates asset folder.

**Tech Stack:** HTML/CSS (Google Fonts, print-optimized), Python + openpyxl (spreadsheet), Markdown (strategy doc)

---

## File Map

```
/Users/sarahbonanno/Documents/Freelance/
├── templates/
│   ├── assets/
│   │   ├── mark-transparent.png        (copied from website repo)
│   │   └── logo-transparent.png        (copied from website repo)
│   ├── invoice.html
│   ├── welcome-packet.html
│   ├── session-summary.html
│   └── receipt.html
├── marketing-strategy.md
├── freelance_income_tracker.xlsx       (overhauled, replaces existing)
└── freelance_income_tracker_backup.xlsx (backup of original)
```

---

### Task 1: Setup — Create Directory Structure and Copy Assets

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/templates/assets/` (directory)
- Copy: `mark-transparent.png`, `logo-transparent.png` from website repo

- [ ] **Step 1: Create the templates directory and assets subfolder**

```bash
mkdir -p /Users/sarahbonanno/Documents/Freelance/templates/assets
```

- [ ] **Step 2: Copy logo assets from the website repo**

```bash
cp /Users/sarahbonanno/Documents/Freelance/bonanno-writing-workshop/mark-transparent.png /Users/sarahbonanno/Documents/Freelance/templates/assets/
cp /Users/sarahbonanno/Documents/Freelance/bonanno-writing-workshop/logo-transparent.png /Users/sarahbonanno/Documents/Freelance/templates/assets/
```

- [ ] **Step 3: Verify files are in place**

```bash
ls -la /Users/sarahbonanno/Documents/Freelance/templates/assets/
```

Expected: both `mark-transparent.png` and `logo-transparent.png` present.

---

### Task 2: Invoice Template

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/templates/invoice.html`

This is the most complex template — it has line items, subtotals, package tracking, and payment info. All fields use `contenteditable="true"` so Sarah can edit directly in the browser before printing to PDF.

- [ ] **Step 1: Create invoice.html**

The invoice includes:
- Header: logo + "INVOICE" title + invoice number (BWW-YYYY-NNN)
- From/To block: Sarah's info on left, client info (parent name, student name, email) on right
- Invoice date and due date (net 15)
- Line items table: Date | Service Type | Duration | Rate | Amount
- Subtotal, discount row (for packages), total due
- Package balance section: "X of Y sessions used, Z remaining. Expires [date]."
- Payment methods: Venmo @bonannowriting / Zelle
- Footer: "Bonanno Writing Workshop | bonannoworkshop.com | sarahcbonanno@gmail.com"

Design system:
- Navy (#1c2a3a) header bar with white text
- Gold (#c9a84c) accent on dividers, total row highlight
- Cream (#f8f5ef) background on the package balance section
- Cormorant Garamond for "INVOICE" heading, Inter for all body text
- Print-optimized: `@media print` hides browser chrome, fits to single page
- `contenteditable="true"` on all editable fields (marked with a subtle dashed border on screen, hidden on print)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Invoice — Bonanno Writing Workshop</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --navy: #1c2a3a;
            --gold: #c9a84c;
            --gold-hover: #b8953e;
            --cream: #f8f5ef;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            color: #1a1a1a;
            background: #f0f0f0;
            -webkit-font-smoothing: antialiased;
        }

        .invoice {
            max-width: 800px;
            margin: 40px auto;
            background: var(--white);
            box-shadow: 0 2px 20px rgba(0,0,0,0.08);
        }

        /* Header */
        .invoice-header {
            background: linear-gradient(170deg, #1e2f42 0%, var(--navy) 40%, #162231 100%);
            padding: 40px 48px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .invoice-header .brand {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .invoice-header .brand img {
            height: 28px;
            filter: brightness(0) invert(1);
            opacity: 0.9;
        }

        .invoice-header .brand span {
            font-family: 'Inter', sans-serif;
            font-size: 12px;
            font-weight: 600;
            letter-spacing: 2.5px;
            text-transform: uppercase;
            color: rgba(255,255,255,0.85);
        }

        .invoice-header .invoice-title {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 36px;
            font-weight: 500;
            color: var(--white);
            letter-spacing: 4px;
        }

        /* Body */
        .invoice-body { padding: 40px 48px; }

        /* Editable fields */
        [contenteditable="true"] {
            border-bottom: 1px dashed #ccc;
            outline: none;
            min-width: 40px;
            display: inline-block;
        }

        [contenteditable="true"]:focus {
            border-bottom-color: var(--gold);
        }

        /* Info blocks */
        .info-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 40px;
        }

        .info-block h3 {
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            margin-bottom: 8px;
        }

        .info-block p {
            font-size: 14px;
            font-weight: 400;
            color: #333;
            line-height: 1.8;
        }

        .info-block .label {
            font-size: 12px;
            color: #999;
            font-weight: 400;
        }

        /* Dates row */
        .dates-row {
            display: flex;
            gap: 48px;
            margin-bottom: 36px;
            padding-bottom: 20px;
            border-bottom: 1px solid #eee;
        }

        .date-item .date-label {
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            margin-bottom: 4px;
        }

        .date-item .date-value {
            font-size: 15px;
            font-weight: 400;
            color: #333;
        }

        /* Line items table */
        .line-items { width: 100%; border-collapse: collapse; margin-bottom: 24px; }

        .line-items th {
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            text-align: left;
            padding: 12px 12px 12px 0;
            border-bottom: 2px solid var(--navy);
        }

        .line-items th:last-child { text-align: right; }

        .line-items td {
            font-size: 14px;
            font-weight: 400;
            color: #333;
            padding: 14px 12px 14px 0;
            border-bottom: 1px solid #f0f0f0;
            vertical-align: top;
        }

        .line-items td:last-child { text-align: right; }

        .line-items .item-type {
            font-weight: 500;
            color: #1a1a1a;
        }

        /* Totals */
        .totals {
            display: flex;
            justify-content: flex-end;
            margin-bottom: 36px;
        }

        .totals-table { width: 280px; }

        .totals-row {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            font-size: 14px;
            color: #555;
        }

        .totals-row.discount { color: #4caf7a; }

        .totals-row.total {
            border-top: 2px solid var(--navy);
            margin-top: 8px;
            padding-top: 12px;
            font-size: 18px;
            font-weight: 600;
            color: var(--navy);
        }

        /* Package balance */
        .package-section {
            background: var(--cream);
            padding: 20px 24px;
            border-radius: 8px;
            margin-bottom: 36px;
            border-left: 3px solid var(--gold);
        }

        .package-section h4 {
            font-size: 11px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: var(--gold-hover);
            margin-bottom: 8px;
        }

        .package-section p {
            font-size: 14px;
            color: #555;
            line-height: 1.7;
        }

        /* Payment */
        .payment-section {
            padding: 24px 0;
            border-top: 1px solid #eee;
            margin-bottom: 24px;
        }

        .payment-section h4 {
            font-size: 11px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            margin-bottom: 12px;
        }

        .payment-methods {
            display: flex;
            gap: 32px;
        }

        .payment-method .method-name {
            font-size: 14px;
            font-weight: 500;
            color: #333;
        }

        .payment-method .method-detail {
            font-size: 13px;
            color: #777;
        }

        /* Footer */
        .invoice-footer {
            background: var(--cream);
            padding: 20px 48px;
            text-align: center;
            font-size: 12px;
            color: #999;
            font-weight: 300;
        }

        .invoice-footer a {
            color: #777;
            text-decoration: none;
        }

        /* Print styles */
        @media print {
            body { background: white; }
            .invoice { box-shadow: none; margin: 0; }
            [contenteditable="true"] { border-bottom: none; }
            .no-print { display: none; }
        }
    </style>
</head>
<body>
    <div class="invoice">
        <div class="invoice-header">
            <div class="brand">
                <img src="assets/mark-transparent.png" alt="">
                <span>Bonanno Writing Workshop</span>
            </div>
            <div class="invoice-title">INVOICE</div>
        </div>

        <div class="invoice-body">
            <div class="info-row">
                <div class="info-block">
                    <h3>From</h3>
                    <p>
                        Sarah Bonanno<br>
                        Bonanno Writing Workshop<br>
                        sarahcbonanno@gmail.com
                    </p>
                </div>
                <div class="info-block" style="text-align: right;">
                    <h3>Bill To</h3>
                    <p>
                        <span contenteditable="true">Parent Name</span><br>
                        <span class="label">Student:</span> <span contenteditable="true">Student Name</span><br>
                        <span contenteditable="true">email@example.com</span>
                    </p>
                </div>
            </div>

            <div class="dates-row">
                <div class="date-item">
                    <div class="date-label">Invoice No.</div>
                    <div class="date-value" contenteditable="true">BWW-2026-001</div>
                </div>
                <div class="date-item">
                    <div class="date-label">Date Issued</div>
                    <div class="date-value" contenteditable="true">April 1, 2026</div>
                </div>
                <div class="date-item">
                    <div class="date-label">Due Date</div>
                    <div class="date-value" contenteditable="true">April 16, 2026</div>
                </div>
            </div>

            <table class="line-items">
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Service</th>
                        <th>Duration</th>
                        <th>Rate</th>
                        <th>Amount</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td contenteditable="true">Apr 6</td>
                        <td class="item-type" contenteditable="true">Academic Tutoring</td>
                        <td contenteditable="true">1 hr</td>
                        <td contenteditable="true">$150.00</td>
                        <td contenteditable="true">$150.00</td>
                    </tr>
                    <tr>
                        <td contenteditable="true">Apr 13</td>
                        <td class="item-type" contenteditable="true">Academic Tutoring</td>
                        <td contenteditable="true">1 hr</td>
                        <td contenteditable="true">$150.00</td>
                        <td contenteditable="true">$150.00</td>
                    </tr>
                    <tr>
                        <td contenteditable="true">Apr 20</td>
                        <td class="item-type" contenteditable="true">Academic Tutoring</td>
                        <td contenteditable="true">1 hr</td>
                        <td contenteditable="true">$150.00</td>
                        <td contenteditable="true">$150.00</td>
                    </tr>
                    <tr>
                        <td contenteditable="true">Apr 27</td>
                        <td class="item-type" contenteditable="true">Academic Tutoring</td>
                        <td contenteditable="true">1 hr</td>
                        <td contenteditable="true">$150.00</td>
                        <td contenteditable="true">$150.00</td>
                    </tr>
                </tbody>
            </table>

            <div class="totals">
                <div class="totals-table">
                    <div class="totals-row">
                        <span>Subtotal</span>
                        <span contenteditable="true">$600.00</span>
                    </div>
                    <!-- Uncomment for package discount:
                    <div class="totals-row discount">
                        <span>Monthly Package (10% off)</span>
                        <span>-$60.00</span>
                    </div>
                    -->
                    <div class="totals-row total">
                        <span>Total Due</span>
                        <span contenteditable="true">$600.00</span>
                    </div>
                </div>
            </div>

            <!-- Include for package clients. Remove for per-session clients. -->
            <div class="package-section">
                <h4>Package Balance</h4>
                <p><span contenteditable="true">4</span> of <span contenteditable="true">16</span> sessions used &middot; <span contenteditable="true">12</span> remaining &middot; Expires <span contenteditable="true">August 30, 2026</span></p>
            </div>

            <div class="payment-section">
                <h4>Payment Methods</h4>
                <div class="payment-methods">
                    <div class="payment-method">
                        <div class="method-name">Venmo</div>
                        <div class="method-detail">@bonannowriting</div>
                    </div>
                    <div class="payment-method">
                        <div class="method-name">Zelle</div>
                        <div class="method-detail">sarahcbonanno@gmail.com</div>
                    </div>
                </div>
            </div>

            <p style="font-size: 13px; color: #999; font-weight: 300;">Payment is due within 15 days of the invoice date. Please include the invoice number with your payment.</p>
        </div>

        <div class="invoice-footer">
            Bonanno Writing Workshop &nbsp;&middot;&nbsp; <a href="https://bonannoworkshop.com">bonannoworkshop.com</a> &nbsp;&middot;&nbsp; sarahcbonanno@gmail.com
        </div>
    </div>
</body>
</html>
```

- [ ] **Step 2: Open invoice.html in a browser and verify**

```bash
open /Users/sarahbonanno/Documents/Freelance/templates/invoice.html
```

Check:
- Navy header with logo + "INVOICE" title renders correctly
- All contenteditable fields have dashed underlines and are clickable/editable
- Line items table is aligned and legible
- Package balance section has cream background with gold left border
- Payment methods section shows Venmo + Zelle
- Print preview (Cmd+P) hides dashed borders, no extra margins

---

### Task 3: Welcome Packet Template

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/templates/welcome-packet.html`

This is the longest template — it's a multi-section document that replaces the existing WelcomePacket.docx. Content is pulled from the existing draft + current website copy, with updated pricing and policies.

- [ ] **Step 1: Create welcome-packet.html**

The welcome packet includes 8 sections:
1. Welcome message (personal, warm — mentions Pleasantville/Hackley background)
2. About / Qualifications (condensed credentials)
3. Services (Academic Tutoring, Writing Tutoring, Application Essays — matching website)
4. How It Works (consultation → welcome packet → sessions)
5. Pricing table (per session, monthly, semester — all 6 price points)
6. Package policies (upfront payment, 6-week/20-week expiry, no refunds, sibling sharing)
7. Policies (net 15, Venmo/Zelle, 24hr cancel, 50% late cancel, 100% no-show, referral program)
8. Contact info

Design: matching the invoice — navy header, gold accents, cream section backgrounds for policies. NOT contenteditable — this is a read-only document sent to families. Print-optimized for multi-page PDF.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome Packet — Bonanno Writing Workshop</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --navy: #1c2a3a;
            --gold: #c9a84c;
            --gold-hover: #b8953e;
            --cream: #f8f5ef;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            color: #1a1a1a;
            background: #f0f0f0;
            -webkit-font-smoothing: antialiased;
            line-height: 1.7;
        }

        .packet {
            max-width: 800px;
            margin: 40px auto;
            background: var(--white);
            box-shadow: 0 2px 20px rgba(0,0,0,0.08);
        }

        /* Header */
        .packet-header {
            background: linear-gradient(170deg, #1e2f42 0%, var(--navy) 40%, #162231 100%);
            padding: 60px 48px;
            text-align: center;
        }

        .packet-header img {
            height: 32px;
            filter: brightness(0) invert(1);
            opacity: 0.9;
            margin-bottom: 24px;
        }

        .packet-header h1 {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 40px;
            font-weight: 400;
            font-style: italic;
            color: var(--white);
            margin-bottom: 8px;
        }

        .packet-header .tagline {
            font-size: 13px;
            font-weight: 300;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: rgba(255,255,255,0.5);
        }

        /* Sections */
        .section {
            padding: 48px;
            border-bottom: 1px solid #f0f0f0;
        }

        .section:last-child { border-bottom: none; }

        .section-label {
            font-size: 11px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 3px;
            color: var(--gold);
            margin-bottom: 20px;
        }

        .section h2 {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 28px;
            font-weight: 500;
            color: var(--navy);
            margin-bottom: 16px;
        }

        .section p {
            font-size: 15px;
            font-weight: 300;
            color: #444;
            line-height: 1.8;
            margin-bottom: 12px;
        }

        .section p:last-child { margin-bottom: 0; }

        .hairline {
            display: block;
            width: 48px;
            height: 1px;
            background: var(--gold);
            border: none;
            margin: 16px 0;
        }

        /* Services grid */
        .services-grid {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 24px;
            margin-top: 20px;
        }

        .service-item {
            padding: 24px;
            background: var(--cream);
            border-radius: 8px;
        }

        .service-item h3 {
            font-size: 16px;
            font-weight: 600;
            color: var(--navy);
            margin-bottom: 8px;
        }

        .service-item p {
            font-size: 13px;
            color: #555;
            line-height: 1.7;
        }

        /* How it works */
        .steps-list {
            counter-reset: step;
            list-style: none;
            margin-top: 16px;
        }

        .steps-list li {
            counter-increment: step;
            padding: 16px 0 16px 56px;
            position: relative;
            font-size: 15px;
            font-weight: 300;
            color: #444;
            border-bottom: 1px solid #f0f0f0;
        }

        .steps-list li:last-child { border-bottom: none; }

        .steps-list li::before {
            content: counter(step);
            position: absolute;
            left: 0;
            top: 14px;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--gold) 0%, var(--gold-hover) 100%);
            color: var(--white);
            font-size: 14px;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 8px rgba(201,168,76,0.3);
        }

        .steps-list li strong {
            font-weight: 500;
            color: #1a1a1a;
        }

        /* Pricing table */
        .pricing-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        .pricing-table th {
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            text-align: left;
            padding: 12px 16px;
            border-bottom: 2px solid var(--navy);
        }

        .pricing-table td {
            font-size: 15px;
            padding: 14px 16px;
            border-bottom: 1px solid #f0f0f0;
            color: #333;
        }

        .pricing-table .price {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 22px;
            font-weight: 600;
            color: var(--navy);
        }

        .pricing-table .savings {
            font-size: 12px;
            font-weight: 500;
            color: var(--gold-hover);
        }

        /* Policy box */
        .policy-box {
            background: var(--cream);
            padding: 28px 32px;
            border-radius: 8px;
            margin-top: 20px;
        }

        .policy-box h3 {
            font-size: 15px;
            font-weight: 600;
            color: var(--navy);
            margin-bottom: 12px;
        }

        .policy-box ul {
            list-style: none;
            padding: 0;
        }

        .policy-box li {
            font-size: 14px;
            font-weight: 300;
            color: #555;
            padding: 6px 0 6px 24px;
            position: relative;
            line-height: 1.7;
        }

        .policy-box li::before {
            content: '';
            position: absolute;
            left: 0;
            top: 13px;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--gold);
            opacity: 0.4;
        }

        /* Contact footer */
        .contact-footer {
            background: var(--cream);
            padding: 36px 48px;
            text-align: center;
        }

        .contact-footer p {
            font-size: 14px;
            color: #666;
            font-weight: 300;
            margin-bottom: 4px;
        }

        .contact-footer a {
            color: var(--navy);
            text-decoration: none;
            font-weight: 400;
        }

        /* Print */
        @media print {
            body { background: white; }
            .packet { box-shadow: none; margin: 0; }
            .section { break-inside: avoid; }
        }
    </style>
</head>
<body>
    <div class="packet">
        <div class="packet-header">
            <img src="assets/logo-transparent.png" alt="Bonanno Writing Workshop">
            <h1>Welcome Packet</h1>
            <div class="tagline">Private Writing Tutoring</div>
        </div>

        <!-- 1. Welcome -->
        <div class="section">
            <div class="section-label">Welcome</div>
            <hr class="hairline">
            <p>Thank you for your interest in working together. I believe that writing is a skill that needs to be practiced and cultivated, and my tutoring is informed by my career as a professional writing instructor and university administrator.</p>
            <p>My philosophy centers on teaching students concrete skills that demystify the writing process. I believe in a collaborative approach where students are active participants in sessions and retain ownership over their ideas and writing.</p>
            <p>I am originally from Pleasantville, NY and graduated from the Hackley School in 2014, which is the place that taught me the foundational writing skills I still use today as a PhD student. I look forward to working with you and your student!</p>
        </div>

        <!-- 2. Qualifications -->
        <div class="section">
            <div class="section-label">Qualifications</div>
            <hr class="hairline">
            <p><strong>Education:</strong> PhD Student in English, CUNY Graduate Center &middot; MA, Theater and Performance Studies, University of Chicago &middot; BA, English, Bowdoin College (cum laude, departmental honors)</p>
            <p><strong>Experience:</strong> Graduate Teaching Fellow, Brooklyn College English Department &middot; Former Assistant Director, University of Chicago Writing Program &middot; Since 2019, I have taught hundreds of students from first-year college students to PhD candidates and business professionals.</p>
        </div>

        <!-- 3. Services -->
        <div class="section">
            <div class="section-label">Services</div>
            <hr class="hairline">
            <div class="services-grid">
                <div class="service-item">
                    <h3>Academic Tutoring</h3>
                    <p>Support for English, literature, and writing-focused courses. Strategies for reading, annotating texts, analyzing literature, and preparing for discussion. High school through graduate level.</p>
                </div>
                <div class="service-item">
                    <h3>Writing Tutoring</h3>
                    <p>Help at all stages of the writing process — from understanding assignments to drafting, revising, and proofreading. Includes on-demand writing exercises to practice skills in real time.</p>
                </div>
                <div class="service-item">
                    <h3>Application Essays</h3>
                    <p>Dedicated support for Common App essays, supplements, scholarships, and statements of purpose. Guidance from brainstorming through final polish on a structured timeline.</p>
                </div>
            </div>
        </div>

        <!-- 4. How It Works -->
        <div class="section">
            <div class="section-label">How It Works</div>
            <hr class="hairline">
            <ol class="steps-list">
                <li><strong>Schedule a Free Consultation</strong> — A quick 15-minute call to discuss your student's needs and goals.</li>
                <li><strong>Review This Packet</strong> — Everything you need to decide if it's the right fit: services, pricing, and policies.</li>
                <li><strong>Start Sessions</strong> — We'll find a time that works and get started. Sessions are held over Zoom or in person in the NYC area.</li>
            </ol>
            <p style="margin-top: 16px;">Depending on assignments, students may be asked to submit drafts in advance so I can provide written feedback with specific margin notes and a general end comment. At the end of each session, students leave with personal recommendations for next steps.</p>
        </div>

        <!-- 5. Pricing -->
        <div class="section">
            <div class="section-label">Pricing</div>
            <hr class="hairline">
            <p>All sessions are one-on-one. Rates include prep time for reading and writing feedback.</p>
            <table class="pricing-table">
                <thead>
                    <tr>
                        <th>Service</th>
                        <th>Per Session</th>
                        <th>Monthly (4 Sessions)</th>
                        <th>Semester (16 Sessions)</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>Academic Tutoring</td>
                        <td><span class="price">$150</span></td>
                        <td><span class="price">$540</span> <span class="savings">Save 10%</span></td>
                        <td><span class="price">$1,920</span> <span class="savings">Save 20%</span></td>
                    </tr>
                    <tr>
                        <td>Application Essays</td>
                        <td><span class="price">$180</span></td>
                        <td><span class="price">$650</span> <span class="savings">Save 10%</span></td>
                        <td><span class="price">$2,300</span> <span class="savings">Save 20%</span></td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- 6. Package Policies -->
        <div class="section">
            <div class="section-label">Package Policies</div>
            <hr class="hairline">
            <div class="policy-box">
                <ul>
                    <li>Packages require full upfront payment to receive the discounted rate.</li>
                    <li>Monthly packages (4 sessions) must be used within 6 weeks of purchase.</li>
                    <li>Semester packages (16 sessions) must be used within 20 weeks of purchase.</li>
                    <li>Unused sessions expire at the end of the package period. No refunds are issued for expired sessions.</li>
                    <li>Packages may be shared between siblings in the same family.</li>
                </ul>
            </div>
        </div>

        <!-- 7. Policies -->
        <div class="section">
            <div class="section-label">Policies</div>
            <hr class="hairline">

            <div class="policy-box" style="margin-top: 0; margin-bottom: 16px;">
                <h3>Payment</h3>
                <ul>
                    <li>Invoices are sent monthly on the 1st. Payment is due within 15 days.</li>
                    <li>Accepted payment methods: Venmo (@bonannowriting) or Zelle (sarahcbonanno@gmail.com).</li>
                    <li>Please include the invoice number with your payment.</li>
                </ul>
            </div>

            <div class="policy-box" style="margin-top: 0; margin-bottom: 16px;">
                <h3>Cancellations</h3>
                <ul>
                    <li>Cancellations must be made at least 24 hours before the scheduled session.</li>
                    <li>Late cancellations (less than 24 hours notice) are charged at 50% of the session rate.</li>
                    <li>No-shows or missed appointments are charged at the full session rate.</li>
                </ul>
            </div>

            <div class="policy-box" style="margin-top: 0;">
                <h3>Referral Program</h3>
                <ul>
                    <li>Know a family who could benefit? Both the referring family and the new client receive $25 off a session.</li>
                </ul>
            </div>
        </div>

        <!-- 8. Contact -->
        <div class="contact-footer">
            <div class="section-label" style="margin-bottom: 16px;">Get in Touch</div>
            <p><a href="mailto:sarahcbonanno@gmail.com">sarahcbonanno@gmail.com</a></p>
            <p><a href="https://bonannoworkshop.com">bonannoworkshop.com</a></p>
            <p style="margin-top: 12px; font-size: 12px; color: #aaa;">&copy; 2026 Bonanno Writing Workshop</p>
        </div>
    </div>
</body>
</html>
```

- [ ] **Step 2: Open welcome-packet.html in a browser and verify**

```bash
open /Users/sarahbonanno/Documents/Freelance/templates/welcome-packet.html
```

Check:
- Navy header with logo, italic "Welcome Packet" title
- All 8 sections render with gold section labels and hairline dividers
- Services grid shows 3 cards in cream boxes
- How It Works has numbered gold circles
- Pricing table shows all 6 price points with "Save" badges
- Policy boxes are cream-background with gold bullet dots
- Print preview (Cmd+P) keeps sections together (break-inside: avoid)

---

### Task 4: Session Summary Template

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/templates/session-summary.html`

Shorter template — a one-page summary sent to parents after sessions. Contenteditable fields for quick fill-in.

- [ ] **Step 1: Create session-summary.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session Summary — Bonanno Writing Workshop</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --navy: #1c2a3a;
            --gold: #c9a84c;
            --gold-hover: #b8953e;
            --cream: #f8f5ef;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            color: #1a1a1a;
            background: #f0f0f0;
            -webkit-font-smoothing: antialiased;
            line-height: 1.7;
        }

        .summary {
            max-width: 800px;
            margin: 40px auto;
            background: var(--white);
            box-shadow: 0 2px 20px rgba(0,0,0,0.08);
        }

        /* Header */
        .summary-header {
            background: linear-gradient(170deg, #1e2f42 0%, var(--navy) 40%, #162231 100%);
            padding: 32px 48px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .summary-header .brand {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .summary-header .brand img {
            height: 24px;
            filter: brightness(0) invert(1);
            opacity: 0.9;
        }

        .summary-header .brand span {
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 2.5px;
            text-transform: uppercase;
            color: rgba(255,255,255,0.85);
        }

        .summary-header .title {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 28px;
            font-weight: 500;
            color: var(--white);
            letter-spacing: 2px;
        }

        /* Body */
        .summary-body { padding: 40px 48px; }

        [contenteditable="true"] {
            border-bottom: 1px dashed #ccc;
            outline: none;
            min-width: 40px;
            display: inline-block;
        }

        [contenteditable="true"]:focus {
            border-bottom-color: var(--gold);
        }

        .editable-block {
            border: 1px dashed #ddd;
            border-radius: 4px;
            padding: 12px 16px;
            min-height: 80px;
            outline: none;
            font-size: 14px;
            font-weight: 300;
            color: #444;
            line-height: 1.8;
        }

        .editable-block:focus {
            border-color: var(--gold);
        }

        /* Session info grid */
        .session-info {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 24px;
            margin-bottom: 32px;
            padding-bottom: 24px;
            border-bottom: 1px solid #f0f0f0;
        }

        .info-item .info-label {
            font-size: 10px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #999;
            margin-bottom: 4px;
        }

        .info-item .info-value {
            font-size: 15px;
            color: #333;
        }

        /* Content sections */
        .content-section {
            margin-bottom: 28px;
        }

        .content-section h3 {
            font-size: 11px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: var(--gold-hover);
            margin-bottom: 12px;
        }

        /* Package balance */
        .package-bar {
            background: var(--cream);
            padding: 16px 24px;
            border-radius: 8px;
            border-left: 3px solid var(--gold);
            margin-bottom: 28px;
            font-size: 14px;
            color: #555;
        }

        /* Next session */
        .next-session {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 16px 0;
            border-top: 1px solid #f0f0f0;
            margin-bottom: 28px;
        }

        .next-session .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #4caf7a;
            box-shadow: 0 0 0 3px rgba(76,175,122,0.2);
        }

        .next-session span {
            font-size: 14px;
            color: #555;
        }

        /* Referral */
        .referral {
            background: var(--cream);
            padding: 20px 24px;
            border-radius: 8px;
            text-align: center;
            font-size: 14px;
            color: #666;
            font-weight: 300;
        }

        .referral strong {
            font-weight: 500;
            color: var(--navy);
        }

        /* Footer */
        .summary-footer {
            background: var(--cream);
            padding: 16px 48px;
            text-align: center;
            font-size: 12px;
            color: #999;
            font-weight: 300;
        }

        .summary-footer a { color: #777; text-decoration: none; }

        @media print {
            body { background: white; }
            .summary { box-shadow: none; margin: 0; }
            [contenteditable="true"] { border-bottom: none; }
            .editable-block { border: none; }
        }
    </style>
</head>
<body>
    <div class="summary">
        <div class="summary-header">
            <div class="brand">
                <img src="assets/mark-transparent.png" alt="">
                <span>Bonanno Writing Workshop</span>
            </div>
            <div class="title">Session Summary</div>
        </div>

        <div class="summary-body">
            <div class="session-info">
                <div class="info-item">
                    <div class="info-label">Student</div>
                    <div class="info-value" contenteditable="true">Student Name</div>
                </div>
                <div class="info-item">
                    <div class="info-label">Date</div>
                    <div class="info-value" contenteditable="true">April 11, 2026</div>
                </div>
                <div class="info-item">
                    <div class="info-label">Session</div>
                    <div class="info-value"><span contenteditable="true">5</span> of <span contenteditable="true">16</span></div>
                </div>
                <div class="info-item">
                    <div class="info-label">Service</div>
                    <div class="info-value" contenteditable="true">Academic Tutoring</div>
                </div>
                <div class="info-item">
                    <div class="info-label">Duration</div>
                    <div class="info-value" contenteditable="true">1 hour</div>
                </div>
            </div>

            <div class="content-section">
                <h3>Topics Covered</h3>
                <div class="editable-block" contenteditable="true">What was covered in this session...</div>
            </div>

            <div class="content-section">
                <h3>What We Worked On</h3>
                <div class="editable-block" contenteditable="true">Specific exercises, drafts reviewed, skills practiced...</div>
            </div>

            <div class="content-section">
                <h3>Next Steps &amp; Recommendations</h3>
                <div class="editable-block" contenteditable="true">What the student should work on before the next session...</div>
            </div>

            <!-- Include for package clients. Remove for per-session clients. -->
            <div class="package-bar">
                <strong>Package Balance:</strong> <span contenteditable="true">5</span> of <span contenteditable="true">16</span> sessions used &middot; <span contenteditable="true">11</span> remaining &middot; Expires <span contenteditable="true">August 30, 2026</span>
            </div>

            <div class="next-session">
                <span class="dot"></span>
                <span>Next session: <span contenteditable="true">Sunday, April 18, 2026 at 10:00 AM</span></span>
            </div>

            <div class="referral">
                Know a family who could use writing support? <strong>Both families receive $25 off a session.</strong>
            </div>
        </div>

        <div class="summary-footer">
            Bonanno Writing Workshop &nbsp;&middot;&nbsp; <a href="https://bonannoworkshop.com">bonannoworkshop.com</a> &nbsp;&middot;&nbsp; sarahcbonanno@gmail.com
        </div>
    </div>
</body>
</html>
```

- [ ] **Step 2: Open session-summary.html in a browser and verify**

```bash
open /Users/sarahbonanno/Documents/Freelance/templates/session-summary.html
```

Check:
- Compact navy header with brand + "Session Summary"
- Info grid shows student, date, session count, service, duration
- Three editable text blocks with dashed borders
- Package balance bar in cream with gold left border
- Next session line with green dot
- Referral banner at bottom
- Print preview fits on one page

---

### Task 5: Receipt Template

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/templates/receipt.html`

Simplest template — a compact payment confirmation.

- [ ] **Step 1: Create receipt.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Receipt — Bonanno Writing Workshop</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --navy: #1c2a3a;
            --gold: #c9a84c;
            --gold-hover: #b8953e;
            --cream: #f8f5ef;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            color: #1a1a1a;
            background: #f0f0f0;
            -webkit-font-smoothing: antialiased;
        }

        .receipt {
            max-width: 560px;
            margin: 40px auto;
            background: var(--white);
            box-shadow: 0 2px 20px rgba(0,0,0,0.08);
        }

        .receipt-header {
            background: linear-gradient(170deg, #1e2f42 0%, var(--navy) 40%, #162231 100%);
            padding: 32px 40px;
            text-align: center;
        }

        .receipt-header img {
            height: 24px;
            filter: brightness(0) invert(1);
            opacity: 0.9;
            margin-bottom: 16px;
        }

        .receipt-header .title {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 28px;
            font-weight: 500;
            color: var(--white);
            letter-spacing: 3px;
        }

        .receipt-body { padding: 36px 40px; }

        [contenteditable="true"] {
            border-bottom: 1px dashed #ccc;
            outline: none;
            min-width: 40px;
            display: inline-block;
        }

        [contenteditable="true"]:focus {
            border-bottom-color: var(--gold);
        }

        /* Confirmation badge */
        .confirmed {
            text-align: center;
            margin-bottom: 32px;
        }

        .confirmed .check {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 48px;
            height: 48px;
            border-radius: 50%;
            background: rgba(76,175,122,0.1);
            margin-bottom: 12px;
        }

        .confirmed .check::after {
            content: '\2713';
            font-size: 22px;
            font-weight: 700;
            color: #4caf7a;
        }

        .confirmed p {
            font-size: 16px;
            font-weight: 500;
            color: #333;
        }

        .confirmed .sub {
            font-size: 13px;
            font-weight: 300;
            color: #999;
            margin-top: 4px;
        }

        /* Detail rows */
        .detail-row {
            display: flex;
            justify-content: space-between;
            padding: 12px 0;
            border-bottom: 1px solid #f0f0f0;
            font-size: 14px;
        }

        .detail-row:last-child { border-bottom: none; }

        .detail-label {
            color: #999;
            font-weight: 400;
        }

        .detail-value {
            color: #333;
            font-weight: 400;
            text-align: right;
        }

        .detail-row.amount-row {
            border-top: 2px solid var(--navy);
            border-bottom: none;
            margin-top: 8px;
            padding-top: 16px;
        }

        .detail-row.amount-row .detail-label {
            font-weight: 600;
            color: var(--navy);
            font-size: 15px;
        }

        .detail-row.amount-row .detail-value {
            font-family: 'Cormorant Garamond', Georgia, serif;
            font-size: 28px;
            font-weight: 600;
            color: var(--navy);
        }

        /* Package balance */
        .package-note {
            background: var(--cream);
            padding: 16px 20px;
            border-radius: 8px;
            border-left: 3px solid var(--gold);
            margin-top: 24px;
            font-size: 13px;
            color: #555;
            line-height: 1.6;
        }

        /* Thank you */
        .thank-you {
            text-align: center;
            margin-top: 28px;
            padding-top: 20px;
            border-top: 1px solid #f0f0f0;
            font-size: 15px;
            font-weight: 300;
            color: #666;
            font-style: italic;
        }

        .receipt-footer {
            background: var(--cream);
            padding: 16px 40px;
            text-align: center;
            font-size: 12px;
            color: #999;
            font-weight: 300;
        }

        .receipt-footer a { color: #777; text-decoration: none; }

        @media print {
            body { background: white; }
            .receipt { box-shadow: none; margin: 0; }
            [contenteditable="true"] { border-bottom: none; }
        }
    </style>
</head>
<body>
    <div class="receipt">
        <div class="receipt-header">
            <img src="assets/mark-transparent.png" alt="">
            <div class="title">RECEIPT</div>
        </div>

        <div class="receipt-body">
            <div class="confirmed">
                <div class="check"></div>
                <p>Payment Received</p>
                <p class="sub" contenteditable="true">April 10, 2026</p>
            </div>

            <div class="detail-row">
                <span class="detail-label">Receipt No.</span>
                <span class="detail-value" contenteditable="true">BWW-R-2026-001</span>
            </div>
            <div class="detail-row">
                <span class="detail-label">Client</span>
                <span class="detail-value" contenteditable="true">Alice Pearlman</span>
            </div>
            <div class="detail-row">
                <span class="detail-label">Invoice(s)</span>
                <span class="detail-value" contenteditable="true">BWW-2026-004</span>
            </div>
            <div class="detail-row">
                <span class="detail-label">Payment Method</span>
                <span class="detail-value" contenteditable="true">Venmo</span>
            </div>
            <div class="detail-row amount-row">
                <span class="detail-label">Amount Received</span>
                <span class="detail-value" contenteditable="true">$600.00</span>
            </div>

            <!-- Include for package clients. Remove for per-session clients. -->
            <div class="package-note">
                <strong>Package Balance:</strong> <span contenteditable="true">4</span> of <span contenteditable="true">16</span> sessions used &middot; <span contenteditable="true">12</span> remaining &middot; Expires <span contenteditable="true">August 30, 2026</span>
            </div>

            <p class="thank-you">Thank you for your payment!</p>
        </div>

        <div class="receipt-footer">
            Bonanno Writing Workshop &nbsp;&middot;&nbsp; <a href="https://bonannoworkshop.com">bonannoworkshop.com</a> &nbsp;&middot;&nbsp; sarahcbonanno@gmail.com
        </div>
    </div>
</body>
</html>
```

- [ ] **Step 2: Open receipt.html in a browser and verify**

```bash
open /Users/sarahbonanno/Documents/Freelance/templates/receipt.html
```

Check:
- Narrower width (560px) — compact receipt feel
- Green checkmark badge with "Payment Received"
- Detail rows are clean and aligned
- Amount row is visually prominent (navy, large Cormorant Garamond number)
- Package balance in cream box with gold border
- Prints to a single half-page

---

### Task 6: Income Tracker Spreadsheet

**Files:**
- Backup: `/Users/sarahbonanno/Documents/Freelance/freelance_income_tracker_backup.xlsx`
- Create: `/Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx` (overwrite)

Build with Python + openpyxl. 4 sheets: Clients, Sessions, Invoices, Summary. Migrate Will's existing data.

- [ ] **Step 1: Back up the existing spreadsheet**

```bash
cp /Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx /Users/sarahbonanno/Documents/Freelance/freelance_income_tracker_backup.xlsx
```

- [ ] **Step 2: Create the new spreadsheet with a Python script**

Run a Python script that:
1. Creates 4 sheets: Clients, Sessions, Invoices, Summary
2. Styles headers with navy background, white text, Inter-like formatting
3. Sets column widths for readability
4. Adds Will Pearlman's data to Clients sheet
5. Migrates his 8 existing sessions to the Sessions sheet
6. Adds summary formulas to the Summary sheet
7. Applies conditional formatting (green for Paid, red for Unpaid)

```python
import openpyxl
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
from openpyxl.utils import get_column_letter
from datetime import date, timedelta

wb = openpyxl.Workbook()

# -- Style definitions --
navy_fill = PatternFill(start_color="1C2A3A", end_color="1C2A3A", fill_type="solid")
cream_fill = PatternFill(start_color="F8F5EF", end_color="F8F5EF", fill_type="solid")
gold_fill = PatternFill(start_color="C9A84C", end_color="C9A84C", fill_type="solid")
white_font = Font(name="Calibri", bold=True, color="FFFFFF", size=11)
header_font = Font(name="Calibri", bold=True, color="FFFFFF", size=11)
body_font = Font(name="Calibri", size=11, color="1A1A1A")
label_font = Font(name="Calibri", size=11, color="999999")
money_format = '"$"#,##0.00'
date_format = "YYYY-MM-DD"
thin_border = Border(
    bottom=Side(style="thin", color="E0E0E0")
)

def style_header(ws, headers, widths):
    for col_idx, (header, width) in enumerate(zip(headers, widths), 1):
        cell = ws.cell(row=1, column=col_idx, value=header)
        cell.font = header_font
        cell.fill = navy_fill
        cell.alignment = Alignment(horizontal="left", vertical="center")
        ws.column_dimensions[get_column_letter(col_idx)].width = width
    ws.row_dimensions[1].height = 30

# ═══ Sheet 1: Clients ═══
ws_clients = wb.active
ws_clients.title = "Clients"
client_headers = ["Student Name", "Parent Name", "Email", "Service Type", "Rate",
                  "Package", "Package Start", "Package Expiry",
                  "Sessions in Package", "Sessions Used", "Sessions Remaining", "Status", "Notes"]
client_widths = [18, 18, 28, 18, 10, 14, 16, 16, 18, 14, 18, 10, 28]
style_header(ws_clients, client_headers, client_widths)

# Add Will's data
will_data = ["Will Pearlman", "Alice Pearlman", "", "Academic Tutoring",
             150, "Per Session", "", "", "", "", "", "Active", "Sundays, Pleasantville"]
for col_idx, val in enumerate(will_data, 1):
    cell = ws_clients.cell(row=2, column=col_idx, value=val)
    cell.font = body_font
    cell.border = thin_border
    if col_idx == 5 and isinstance(val, (int, float)):
        cell.number_format = money_format

# ═══ Sheet 2: Sessions ═══
ws_sessions = wb.create_sheet("Sessions")
session_headers = ["Date", "Student", "Service Type", "Duration (hrs)", "Rate", "Amount", "Invoice #", "Notes"]
session_widths = [14, 18, 18, 14, 10, 12, 16, 36]
style_header(ws_sessions, session_headers, session_widths)

# Migrate Will's sessions
will_sessions = [
    (date(2026, 1, 11), "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 1, 18), "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 1, 25), "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 2, 1),  "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 2, 15), "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 2, 22), "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 3, 1),  "Will Pearlman", "Academic Tutoring", 1, 150),
    (date(2026, 3, 15), "Will Pearlman", "Academic Tutoring", 1, 150),
]

for row_idx, (d, student, stype, hours, rate) in enumerate(will_sessions, 2):
    ws_sessions.cell(row=row_idx, column=1, value=d).number_format = date_format
    ws_sessions.cell(row=row_idx, column=2, value=student)
    ws_sessions.cell(row=row_idx, column=3, value=stype)
    ws_sessions.cell(row=row_idx, column=4, value=hours)
    ws_sessions.cell(row=row_idx, column=5, value=rate).number_format = money_format
    ws_sessions.cell(row=row_idx, column=6).value = hours * rate
    ws_sessions.cell(row=row_idx, column=6).number_format = money_format
    for col_idx in range(1, 9):
        ws_sessions.cell(row=row_idx, column=col_idx).font = body_font
        ws_sessions.cell(row=row_idx, column=col_idx).border = thin_border

# ═══ Sheet 3: Invoices ═══
ws_invoices = wb.create_sheet("Invoices")
invoice_headers = ["Invoice #", "Date Sent", "Client (Parent)", "Amount", "Due Date", "Status", "Date Paid", "Payment Method", "Notes"]
invoice_widths = [16, 14, 20, 12, 14, 10, 14, 16, 28]
style_header(ws_invoices, invoice_headers, invoice_widths)

# ═══ Sheet 4: Summary ═══
ws_summary = wb.create_sheet("Summary")
ws_summary.column_dimensions["A"].width = 24
ws_summary.column_dimensions["B"].width = 16

# Title
title_cell = ws_summary.cell(row=1, column=1, value="Bonanno Writing Workshop")
title_cell.font = Font(name="Calibri", bold=True, size=14, color="1C2A3A")
subtitle_cell = ws_summary.cell(row=2, column=1, value="Income Summary")
subtitle_cell.font = Font(name="Calibri", size=12, color="999999")

# Key metrics
metrics = [
    (4, "Total Sessions", '=COUNTA(Sessions!A2:A500)'),
    (5, "Total Hours", '=SUM(Sessions!D2:D500)'),
    (6, "Total Revenue", '=SUM(Sessions!F2:F500)'),
    (7, "Outstanding Invoices", '=COUNTIF(Invoices!F2:F500,"Unpaid")'),
    (8, "Amount Outstanding", '=SUMIFS(Invoices!D2:D500,Invoices!F2:F500,"Unpaid")'),
]

for row, label, formula in metrics:
    label_cell = ws_summary.cell(row=row, column=1, value=label)
    label_cell.font = Font(name="Calibri", size=11, color="555555")
    val_cell = ws_summary.cell(row=row, column=2, value=formula)
    val_cell.font = Font(name="Calibri", bold=True, size=11, color="1C2A3A")
    if "Revenue" in label or "Amount" in label:
        val_cell.number_format = money_format

# Monthly breakdown
ws_summary.cell(row=10, column=1, value="Monthly Breakdown").font = Font(name="Calibri", bold=True, size=12, color="1C2A3A")
months = ["January", "February", "March", "April", "May", "June",
          "July", "August", "September", "October", "November", "December"]

for i, month in enumerate(months):
    row = 11 + i
    ws_summary.cell(row=row, column=1, value=month).font = Font(name="Calibri", size=11, color="555555")
    formula = f'=SUMPRODUCT((MONTH(Sessions!A2:A500)={i+1})*(YEAR(Sessions!A2:A500)=YEAR(TODAY()))*Sessions!F2:F500)'
    val_cell = ws_summary.cell(row=row, column=2, value=formula)
    val_cell.number_format = money_format
    val_cell.font = body_font

# Revenue by client
ws_summary.cell(row=24, column=1, value="Revenue by Client").font = Font(name="Calibri", bold=True, size=12, color="1C2A3A")
ws_summary.cell(row=25, column=1, value="Will Pearlman").font = body_font
ws_summary.cell(row=25, column=2, value='=SUMIFS(Sessions!F2:F500,Sessions!B2:B500,"Will Pearlman")').number_format = money_format

# Freeze panes
ws_clients.freeze_panes = "A2"
ws_sessions.freeze_panes = "A2"
ws_invoices.freeze_panes = "A2"

wb.save("/Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx")
print("Spreadsheet created successfully.")
```

- [ ] **Step 3: Verify the spreadsheet**

```bash
python3 -c "
import openpyxl
wb = openpyxl.load_workbook('/Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx')
print('Sheets:', wb.sheetnames)
for name in wb.sheetnames:
    ws = wb[name]
    print(f'{name}: {ws.max_row} rows, {ws.max_column} cols')
"
```

Expected: 4 sheets (Clients, Sessions, Invoices, Summary). Sessions has 9 rows (1 header + 8 Will sessions). Clients has 2 rows (header + Will).

---

### Task 7: Marketing Strategy Document

**Files:**
- Create: `/Users/sarahbonanno/Documents/Freelance/marketing-strategy.md`

A clean reference document Sarah can use as a checklist. Content from the spec's Part 1, formatted as an actionable playbook.

- [ ] **Step 1: Create marketing-strategy.md**

This is a direct write of the marketing strategy from the spec, formatted as an actionable reference with checkboxes.

Content:
- Header with goal statement
- "Immediate (April)" section with 3 action items
- "Short-term (April-May)" section with 3 action items
- "Summer (June-August)" section with 3 action items
- "Ongoing" section with recurring habits
- Each item has a checkbox, specific action, and brief rationale

- [ ] **Step 2: Verify the file exists and is well-formatted**

```bash
cat /Users/sarahbonanno/Documents/Freelance/marketing-strategy.md | head -60
```

---

### Task 8: Final Verification

- [ ] **Step 1: Verify all files are in place**

```bash
ls -la /Users/sarahbonanno/Documents/Freelance/templates/
ls -la /Users/sarahbonanno/Documents/Freelance/templates/assets/
ls -la /Users/sarahbonanno/Documents/Freelance/marketing-strategy.md
ls -la /Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx
ls -la /Users/sarahbonanno/Documents/Freelance/freelance_income_tracker_backup.xlsx
```

Expected file tree:
```
/Users/sarahbonanno/Documents/Freelance/
├── templates/
│   ├── assets/
│   │   ├── mark-transparent.png
│   │   └── logo-transparent.png
│   ├── invoice.html
│   ├── welcome-packet.html
│   ├── session-summary.html
│   └── receipt.html
├── marketing-strategy.md
├── freelance_income_tracker.xlsx       (new)
├── freelance_income_tracker_backup.xlsx (backup of original)
└── (existing files untouched)
```

- [ ] **Step 2: Open all 4 templates in browser for visual check**

```bash
open /Users/sarahbonanno/Documents/Freelance/templates/invoice.html
open /Users/sarahbonanno/Documents/Freelance/templates/welcome-packet.html
open /Users/sarahbonanno/Documents/Freelance/templates/session-summary.html
open /Users/sarahbonanno/Documents/Freelance/templates/receipt.html
```

Verify: consistent navy headers, gold accents, Cormorant Garamond headings, Inter body text across all four. Contenteditable fields work on invoice, session summary, and receipt. Welcome packet is read-only.

- [ ] **Step 3: Open the spreadsheet and verify**

```bash
open /Users/sarahbonanno/Documents/Freelance/freelance_income_tracker.xlsx
```

Check: 4 sheets, Will's data migrated, navy headers, formulas calculate correctly on Summary sheet.
