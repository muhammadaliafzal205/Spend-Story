# SpendStory 💸

> **Your bank statement has a story. SpendStory helps you read it.**

SpendStory is an AI-powered personal finance platform that transforms raw bank statements into personalized, plain-English financial narratives. Upload your statement in any format — scanned image, digital PDF, or CSV — and get a full financial analysis in seconds. No finance degree required. No data ever leaves your browser.

---

## Live Demo

🔗 **[spendstory.netlify.app](https://spendstory.netlify.app)**

> Use the **"Sample data"** button on the upload screen to try it instantly without a real bank statement.

---

## Screenshots

| Upload | Overview Dashboard | AI Chat |
|---|---|---|
| ![Upload](https://via.placeholder.com/280x180?text=Upload+Screen) | ![Dashboard](https://via.placeholder.com/280x180?text=Overview+Dashboard) | ![Chat](https://via.placeholder.com/280x180?text=AI+Chat) |

---

## Features

### Core
- **AI Spending Narrative** — Claude AI generates a plain-English story of your monthly spending
- **Interactive Dashboard** — total spend, daily average, category breakdown, trend charts
- **Transaction Table** — searchable, filterable, sortable with PDF export
- **Financial Health Score** — 0–100 composite score with breakdown and recommendations

### Smart Analysis
- **Subscription Hunter** — auto-detects recurring charges, shows monthly + yearly cost
- **Anomaly Resolution Hub** — flags unusual spending spikes for user review
- **Compare & Forecast** — month-over-month comparison with AI spending predictions
- **What-If Sandbox** — simulate alternative spending scenarios in real time

### User Control
- **Category Manager** — create custom categories and merchant-mapping rules
- **AI Audit Trail** — every insight links back to the exact transactions that triggered it
- **Manual Override** — correct any AI categorization or anomaly decision

### Pakistani Bank Support
- Full support for **Meezan, HBL, MCB, UBL, Alfalah, JS Bank** PDF formats
- Handles Pakistani comma-decimal format (`380,00` = PKR 380.00)
- Multi-line transaction description reconstruction
- OCR support for scanned printed statements

### Sharing
- **Spending Wrapped** — Spotify-style shareable annual recap card with personality type

---

## Supported File Formats

| Format | Method | Banks |
|---|---|---|
| Digital PDF | PDF.js text extraction | Meezan, HBL, MCB, UBL, Alfalah, JS Bank |
| Scanned PDF | PDF.js → Canvas → Tesseract OCR | Any printed statement |
| JPG / PNG image | Tesseract OCR directly | Photographed statements |
| CSV export | PapaParse | Any bank worldwide |

---

## Tech Stack

```
Frontend       Vanilla JavaScript (ES6+) · HTML5 · CSS3
AI             Claude API (Anthropic) · Tesseract.js OCR
Document       PDF.js · PapaParse · Canvas API · FileReader API
Visualization  Chart.js
Typography     DM Sans · DM Serif Display (Google Fonts)
Deployment     Netlify
```



---

## How to Run Locally

No build step. No npm install. No dependencies to manage.

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/spendstory.git

# 2. Open in browser
cd spendstory
open index.html
```

> For AI features (narrative generation and chat) you need a Claude API key.
> Add it to `js/utils/api.js`:

```javascript
// js/utils/api.js  —  line 1
const ANTHROPIC_KEY = 'your-api-key-here';
```

Then add the header to the `_claudeCall` function:

```javascript
headers: {
  'Content-Type': 'application/json',
  'x-api-key': ANTHROPIC_KEY,
  'anthropic-dangerous-direct-browser-access': 'true',
}
```

Get a free API key at [console.anthropic.com](https://console.anthropic.com).

---

## How the Pakistani Bank Parser Works

Pakistani bank statements have a unique column layout and use comma as a decimal separator — a format no existing open-source parser handles correctly.

### The problem
```
Meezan Bank statement:  380,00  ≠  38,000
                        380,00  =  PKR 380.00
```

### The solution

```javascript
// Detect Pakistani decimal format
const pkMatch = raw.match(/^([\d,]+),(\d{2})$/);
if (pkMatch) {
  const intPart = pkMatch[1].replace(/,/g, '');
  return parseFloat(intPart + '.' + pkMatch[2]);
}
```

### The pipeline

```
Digital PDF  →  PDF.js extracts text items with X/Y coordinates
             →  Group by Y-position into lines (4px tolerance)
             →  Detect column positions from header row
             →  Assign each item to: Date | Description | Debit | Credit
             →  Parse Pakistani decimal amounts
             →  Reconstruct multi-line descriptions

Scanned PDF  →  PDF.js renders page to canvas at 2.5× scale
             →  Tesseract.js OCR reads the canvas
             →  Same pattern matching applied to extracted text

< 3 rows found  →  Claude AI reads raw text and extracts JSON
```

### Verified accuracy

Tested against a real Meezan Bank statement (Feb 2026):

| Metric | Statement says | SpendStory extracts |
|---|---|---|
| Total debits | PKR 185,020.00 | PKR 185,020.00 ✅ |
| Total credits | PKR 211,470.00 | PKR 211,470.00 ✅ |
| Transactions | 36 rows | 36 rows ✅ |

---

## Privacy

SpendStory processes everything **locally in your browser**.

- No server receives your bank data
- No database stores your transactions
- No account required
- No tracking or analytics on financial data
- Files are read into memory and discarded after analysis

---

## Functional Screens

SpendStory has **14 fully interactive screens**:

| # | Screen | Description |
|---|---|---|
| 1 | Welcome / Onboarding | 3-step wizard with format selection |
| 2 | Data Upload | PDF, image, CSV with real-time extraction progress |
| 3 | Analysis Progress | Animated step-by-step processing view |
| 4 | Overview Dashboard | Metrics, AI story, charts, anomalies |
| 5 | Transaction Manager | Full table with search, filter, sort, export |
| 6 | Compare & Forecast | Period comparison + AI spending forecast |
| 7 | Health Score | Animated ring chart + sub-scores + tips |
| 8 | Subscription Hunter | Recurring charge detection + savings insight |
| 9 | Smart Budget | AI-suggested budgets with actual vs target |
| 10 | What-If Sandbox | Variable manipulation for spending scenarios |
| 11 | Category Manager | Custom categories + merchant-mapping rules |
| 12 | AI Audit Trail | Every insight linked to source transactions |
| 13 | AI Chat Assistant | Natural language questions about your data |
| 14 | Spending Wrapped | Shareable Spotify-style annual recap |

---

## HCI Principles Applied

| Principle | Implementation |
|---|---|
| Progressive Disclosure | Plain narrative first, deep analytics on demand |
| Direct Manipulation | Drag-drop categorization, inline editing |
| Flexibility of Use | Bulk actions, quick-ask buttons, keyboard shortcuts |
| Error Prevention | Data validation before analysis, clear error states |
| User Control | Every AI decision is overridable |

---

## Hackathon

Built for **Frostbyte Hackathon 2026** — an international AI & tech competition.

**Themes:** FinTech & Blockchain · Artificial Intelligence & Machine Learning

---

## What's Next

- Multi-account merging (Meezan + JazzCash + EasyPaisa)
- Urdu language support with one toggle
- AI-powered what-if ("what if I stop late-night food delivery")
- Family finance mode for joint-income households
- Open-source the Pakistani bank parser as a standalone npm package

---

## License

MIT License — free to use, modify, and distribute.

---

<div align="center">
  <p>Built with curiosity, caffeine, and a genuine belief that everyone deserves to understand their own money.</p>
  <p><strong>SpendStory — Your bank statement has a story. Read it.</strong></p>
</div>
