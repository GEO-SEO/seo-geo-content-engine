---
name: link-building-outreach
description: Use this skill when the user wants to automate link building outreach, send outreach emails for backlinks, find contact information for article authors, generate personalized outreach emails, process a list of link opportunities, or run an outreach campaign. Trigger this skill whenever the user mentions "outreach", "link building", "邮件外链", "contact author", "backlink campaign", "find email for", or asks to process a spreadsheet of outreach targets. Also trigger when the user says "run outreach", "setup outreach", or wants to scale their link building efforts.
---

# Link Building Outreach Skill

Automates the full link building outreach pipeline: reads your opportunity list, finds author contacts, analyzes target articles, and generates personalized outreach emails — ready to send.

## When to Use

- Before or during a link building campaign
- Processing a batch of outreach opportunities from a spreadsheet
- Finding contact info for article authors
- Generating personalized emails at scale
- Single-opportunity quick outreach

## Quick Start

**First time:** Tell Claude:
> "Setup outreach — my product is [URL], my audience is [description], my name is [name], my title is [title]"

**Every time after:**
> "Run outreach" or "Run outreach on [spreadsheet URL]"

---

## Setup (One-Time Only)

Ask the user for these four things if not already provided:

| Input | Description | Example |
|-------|-------------|---------|
| **Product URL** | Main product/service page | `https://yourproduct.com` |
| **Target Audience** | Who uses the product | `SEO professionals, growth hackers` |
| **Your Name** | For email signature | `Jane Smith` |
| **Your Title** | Job title | `Founder at Acme SEO` |

### During Setup, Claude Will:

1. **Fetch product page** — Extract product name, features, USPs, tone/voice
2. **Identify differentiators** — What makes this product stand out vs. alternatives
3. **Generate base email templates** — One per article type (Best/Top, How-to, Review, Resource)
4. **Confirm with user** — Show a summary before proceeding

> ⚠️ **Note:** Competitor analysis requires either a manually provided competitor list or a connected SEO API. Claude will ask if none is available.

Store all setup output in memory for the session. If context is lost, re-run setup.

---

## Outreach Pipeline (6 Steps)

### Step 1 — Read Opportunity List

**Source:** Google Sheets (via connected Google Sheets tool) or a CSV/table pasted directly.

**Required columns:**

| Column | Description | Example |
|--------|-------------|---------|
| `URL` | Target article to get a link from | `https://example.com/best-seo-tools` |
| `Target Page` | Your page to link to | `https://yourproduct.com` |
| `Status` | Processing flag | `pending` |

**Auto-detected optional columns:** `Article Type`, `Anchor Text`, `Notes`

Filter to rows where `Status = pending`. If no Google Sheets connection, ask user to paste the list.

---

### Step 2 — Detect Article Type

Fetch each target URL and classify:

| Type | Detection Signals |
|------|-------------------|
| **Best / Top List** | Title: `best`, `top`, `N tools`, `review of` |
| **How-to / Tutorial** | Title: `how to`, `guide`, `tutorial`, `step-by-step` |
| **Review / Comparison** | Title: `review`, `vs`, `comparison`, `alternatives` |
| **Resource Page** | Many outbound links, aggregator structure |
| **Other** | Cannot classify — flag for manual review |

If user pre-filled `Article Type`, skip detection and use their value.

---

### Step 3 — Find Contact Information

Search in this priority order. Stop when a high-confidence email is found.

| Priority | Channel | Method |
|----------|---------|--------|
| 1 | Author byline | Extract name from article, search `name + domain` on Hunter.io or similar |
| 2 | Website contact page | Check `/contact`, `/about`, `/advertise` |
| 3 | Author personal website | If linked from byline, check About/Contact page |
| 4 | Twitter/X bio | If author Twitter linked, check bio |
| 5 | LinkedIn | If author LinkedIn linked, check profile (manual lookup required) |
| 6 | Not found | Mark for manual follow-up |

> ⚠️ **Realistic constraint:** LinkedIn does not allow automated scraping. If LinkedIn is the only source, flag the entry as `confidence: Low` and recommend manual DM outreach instead.

**Output per entry:**
```
email: john@example.com
source: Hunter.io / contact page / Twitter / not found
confidence: High / Medium / Low / Not Found
```

**Confidence levels:**
- **High** — Verified via Hunter.io or direct contact page
- **Medium** — Inferred from domain pattern or social bio
- **Low** — Guessed or single unverified source
- **Not Found** — Try LinkedIn/Twitter DM manually

---

### Step 4 — Analyze Target Article

Fetch and read the full article content. Extract:

- **Key sections** — Main headings and what they cover
- **Specific claims** — Quotes or data points to reference naturally
- **Missing gaps** — Topics/tools the article doesn't cover (opportunity for your product)
- **Author angle** — Data-driven? Opinionated? Beginner-focused?
- **Relevance match** — How does your product connect to this article's theme?

Example output:
```
article_summary: "Reviews top 10 SEO tools, Ahrefs ranked #1 for backlink data"
key_reference: "Section on backlink analysis — author emphasizes data accuracy"
gap_identified: "No mention of AI Overview gap detection"
author_angle: "Practical, data-driven, values accuracy"
relevance: "Your product fills the AI Overview gap not covered in article"
```

---

### Step 5 — Generate Personalized Email

Using setup data + Step 4 analysis, generate one email per opportunity.

**Email rules:**
- Under 150 words (concise = higher reply rate)
- Reference ONE specific section or data point from the article
- One clear ask (mention / trial / update)
- No generic openers ("Hope this finds you well")
- Match tone to author angle (casual vs. formal)

**Template by article type:**

**Best/Top List:**
> Hi [Name], read your [article title] — the section on [specific point] stood out. Noticed you didn't include [Your Product]. We [key differentiator]. Happy to set up a free trial if you'd consider it for your next update. — [Your Name], [Title]

**How-to / Tutorial:**
> Hi [Name], your guide on [topic] is genuinely useful — especially [specific section]. One addition your readers might find helpful: [Your Product] lets them [relevant use case]. Worth a mention if it fits. — [Your Name], [Title]

**Review / Comparison:**
> Hi [Name], your [comparison title] covers the major players well. One tool worth considering for a future update: [Your Product]. Key difference from [competitor they mentioned]: [differentiator]. Happy to share more. — [Your Name], [Title]

**Resource Page:**
> Hi [Name], I found your [page title] while researching [topic]. [Your Product] might be a useful addition — [one-line description]. Let me know if you'd like to take a look. — [Your Name], [Title]

---

### Step 6 — Output Structured Report

Output all processed entries in a table, saved to Google Doc or shown in conversation:

| # | Target URL | Article Type | Email | Source | Confidence | Subject | Body | Status |
|---|-----------|--------------|-------|--------|------------|---------|------|--------|
| 1 | [url] | Best/Top | john@x.com | Hunter.io | High | [subject] | [body] | processed |

**Confidence-based actions:**

| Confidence | Recommended Action |
|------------|-------------------|
| High | Send immediately |
| Medium | Quick review before sending |
| Low | Verify email first (Hunter.io, manual check) |
| Not Found | Try LinkedIn DM or Twitter DM |

---

## Commands (Natural Language)

| Say This | Action |
|----------|--------|
| "Setup outreach" | Start one-time setup wizard |
| "Run outreach" | Process all pending rows |
| "Run outreach on [URL]" | Process one opportunity only |
| "Regenerate email for [URL]" | Rewrite that entry's email |
| "Update product info" | Re-fetch and re-analyze product page |
| "Show pending" | List unprocessed opportunities |

---

## Sending Volume Guidelines

| Phase | Volume | Goal |
|-------|--------|------|
| Week 1 | 5–10/day | Test & calibrate |
| Week 2 | 10–20/day | Scale what works |
| Ongoing | Based on reply rate | Optimize |

> Always manually review emails before sending. Claude generates drafts — you approve and send.

---

## Compliance

Ensure all outreach complies with:
- **CAN-SPAM** (US) — Include physical address, opt-out option
- **GDPR** (EU) — Only contact business emails, include opt-out
- **No bulk automated sending** — Review and send manually or via your own ESP

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Can't find contact email | System tries 5 channels; flag for manual LinkedIn/Twitter DM |
| Email feels generic | Re-run Step 4 analysis with a more detailed article fetch |
| Wrong article type detected | Pre-fill `Article Type` column — system respects manual values |
| Low confidence on all emails | Use Hunter.io directly to verify before sending |
| Google Sheets not connecting | Paste the spreadsheet data directly into the conversation |
| Product info outdated | Say "Update product info" to re-fetch |

---

## References

- See `references/email-templates.md` for extended template library by niche
- See `references/compliance-checklist.md` for CAN-SPAM / GDPR details
