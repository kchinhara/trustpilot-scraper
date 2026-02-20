---
name: trustpilot-scraper
description: "Scrape and analyze Trustpilot reviews for any company. Extracts ratings, themes, and sentiment to generate PPC-actionable competitive intelligence. Outputs JSON, CSV, and a structured review analysis report."
license: MIT
metadata:
  version: 1.1.0
  author: Kuda Chinhara
  url: https://agenticppcads.com
  domains: [scraping, reviews, competitive-intelligence, reputation-monitoring]
  type: generator
  inputs: [domain, client-name, search-term, max-reviews]
  outputs: [JSON, CSV, review-analysis-report]
---

# Trustpilot Review Scraper & Analyzer

Scrape Trustpilot reviews for any company and generate competitive intelligence summaries with PPC-actionable insights.

Built by [Kuda Chinhara](https://linkedin.com/in/kudachinhara) at [Agentic PPC Ads](https://agenticppcads.com).

---

## Quick Start

```
/trustpilot elder.org
/trustpilot care-verify elder.org
/trustpilot care-verify elder.org --search "bad service" --max 200
```

---

## Triggers

- `/trustpilot [domain]` - Scrape reviews for a domain
- `/trustpilot [client] [domain]` - Scrape and save to client directory
- `scrape trustpilot reviews for [domain]`
- `trustpilot analysis for [domain]`
- `get reviews for [domain] from trustpilot`

| Input | Output |
|-------|--------|
| Domain only | JSON + CSV + console analysis |
| Client + Domain | JSON + CSV + analysis saved to `clients/[client]/trustpilot/` |
| Domain + search term | Filtered reviews matching search term |

---

## Setup (One-Time)

The scraper requires a local copy of `tpscraper.js` with Puppeteer installed.

```bash
# Clone or create a trustpilot-scraper directory somewhere on your machine
mkdir trustpilot-scraper && cd trustpilot-scraper
npm install puppeteer
# Place tpscraper.js in this directory
```

### Configuration

Set `SCRAPER_PATH` to wherever your `tpscraper.js` lives. Outputs are saved relative to your current working directory.

```
SCRAPER_PATH: <path-to-your-trustpilot-scraper-directory>
SCRAPER_SCRIPT: tpscraper.js
```

---

## Workflow

### Phase 1: Parse & Validate

1. **Parse arguments from $ARGUMENTS:**
   - First arg: If it matches a known client folder in `clients/`, treat as client name and next arg as domain. Otherwise treat as domain.
   - Domain (required): Company domain as it appears on Trustpilot (e.g., `elder.org`, `example.co.uk`)
   - Client name (optional): Maps to `clients/[client]/trustpilot/` output directory
   - `--search [term]`: Filter reviews by keyword
   - `--max [number]`: Maximum reviews to collect (default: no limit)
   - `--pages [number]`: Maximum pages to scrape (default: no limit)

2. **Verify scraper is ready:**
   ```bash
   ls "{SCRAPER_PATH}/tpscraper.js"
   ls "{SCRAPER_PATH}/node_modules/puppeteer"
   ```
   - If `node_modules` missing, run: `cd "{SCRAPER_PATH}" && npm install`

3. **Create output directory** (if client specified):
   ```bash
   mkdir -p "clients/[client]/trustpilot"
   ```

### Phase 2: Scrape

1. **Run the scraper** from the scraper directory (output files are created in CWD):
   ```bash
   cd "{SCRAPER_PATH}" && node tpscraper.js --domain "[domain]" [--searchTerm "[term]"] [--maxreviews [N]] [--maxpages [N]]
   ```

2. **Verify output files exist:**
   - Look for `trustpilot_[sanitized_domain].json` and `.csv` in the scraper directory
   - The scraper sanitizes domain names: `elder.org` → `trustpilot_elder.json`, `example.co.uk` → `trustpilot_example.json`

3. **Move output to client directory** (if client specified):
   ```bash
   cp trustpilot_[name].json "clients/[client]/trustpilot/"
   cp trustpilot_[name].csv "clients/[client]/trustpilot/"
   ```

### Phase 3: Analyze

Read the JSON output file and generate a comprehensive analysis. This is the core intelligence layer.

**Calculate these metrics from the review data:**

#### 3A: Rating Distribution
```
5-star: [count] ([%])
4-star: [count] ([%])
3-star: [count] ([%])
2-star: [count] ([%])
1-star: [count] ([%])
Average: [X.X] / 5.0
Total reviews analyzed: [N]
```

#### 3B: Sentiment Theme Extraction

**From 4-5 star reviews (Positive Themes):**
- Read ALL positive reviews
- Identify the top 5-8 recurring themes/topics customers praise
- For each theme: name, frequency count, representative quote

**From 1-2 star reviews (Negative Themes / Objections):**
- Read ALL negative reviews
- Identify the top 5-8 recurring complaints/objections
- For each theme: name, frequency count, representative quote

**From 3-star reviews (Mixed Signals):**
- Identify what "almost" works - these reveal improvement opportunities

#### 3C: PPC-Actionable Insights

Generate specific recommendations for Google Ads campaigns:

| Category | What to Extract |
|----------|----------------|
| **Ad Copy Angles** | Top 3-5 positive themes to use as headlines/descriptions |
| **Social Proof Lines** | Direct quotes suitable for ad copy (short, punchy) |
| **Objection Handling** | Top complaints to address proactively in ads/landing pages |
| **Keyword Signals** | Language customers use naturally (potential keywords) |
| **Competitor Weaknesses** | If negative reviews mention switching from competitors |
| **Trust Signals** | Specific numbers (response time, years, percentages) mentioned in reviews |

#### 3D: Notable Quotes

Extract 5-10 standout review quotes that are:
- Short enough for ad copy or landing page testimonials
- Specific (mention results, numbers, experiences)
- Emotionally compelling

### Phase 4: Output

1. **Present the analysis** in a structured format to the user in the console

2. **Save the analysis** as markdown (if client specified):
   - Save to: `clients/[client]/trustpilot/trustpilot_[domain]_analysis.md`
   - Include all sections from Phase 3
   - Add metadata: date analyzed, total reviews, source domain

3. **Summary banner:**
   ```
   ✓ Trustpilot Scrape Complete
   Domain: [domain]
   Reviews: [N] collected
   Average Rating: [X.X]/5.0
   Output: [file paths]
   ```

---

## Output Schema

### JSON (from scraper)
```json
[
  {
    "reviewerName": "John Doe",
    "dateExperience": "February 10, 2026",
    "rating": "5",
    "title": "Excellent service",
    "reviewText": "Great experience with this company..."
  }
]
```

### Analysis Report (generated by skill)
```markdown
# Trustpilot Review Analysis: [domain]
**Date:** [YYYY-MM-DD]
**Total Reviews:** [N]
**Average Rating:** [X.X]/5.0

## Rating Distribution
[table]

## Positive Themes
[themed analysis with quotes]

## Negative Themes / Objections
[themed analysis with quotes]

## PPC-Actionable Insights
[ad copy angles, social proof, objection handling]

## Notable Quotes
[curated testimonial quotes]
```

---

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Scraping without analysis | Raw data isn't actionable | Always run Phase 3 |
| Ignoring negative reviews | Missing objection-handling gold | Analyze all ratings |
| Generic theme names | "Good service" tells you nothing | Be specific: "Fast response time" |
| Copying full reviews as quotes | Too long for ads | Extract the punchiest 1-2 sentences |
| Running without --max on huge profiles | Slow, excessive | Suggest --max 500 for large profiles |

---

## Integration with Other Skills

| Skill | How Trustpilot Data Helps |
|-------|--------------------------|
| `/competitive_analysis` | Add review sentiment to competitor profiles |
| `/ad_recon` | Compare ad claims vs actual customer experience |
| `/business_clarity` | Enrich pain point analysis with real customer language |
| `/thank_you_page` | Use positive quotes as social proof on TY pages |
| `/landing_page_audit` | Check if LP addresses top objections from reviews |

---

## Verification Checklist

After execution:
- [ ] JSON file exists with review data
- [ ] CSV file exists with review data
- [ ] Rating distribution calculated correctly (counts sum to total)
- [ ] At least 3 positive themes identified (if sufficient positive reviews)
- [ ] At least 3 negative themes identified (if sufficient negative reviews)
- [ ] PPC insights are specific and actionable (not generic)
- [ ] Notable quotes are short enough for ad copy (<150 chars each)
- [ ] Files saved to correct client directory (if client specified)

---

*Built by [Kuda Chinhara](https://linkedin.com/in/kudachinhara) | [Agentic PPC Ads](https://agenticppcads.com) | [X: @AIPPCKuda](https://x.com/AIPPCKuda)*
