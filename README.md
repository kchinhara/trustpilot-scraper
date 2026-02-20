# Trustpilot Review Scraper & Analyzer

Scrape Trustpilot reviews for any company and turn them into PPC-actionable competitive intelligence. Rating distributions, sentiment themes, ad copy angles, objection handling, and standout quotes — all from real customer reviews.

## What You Get

| Output | Description |
|--------|-------------|
| `trustpilot_[domain].json` | Raw review data (reviewer, date, rating, title, text) |
| `trustpilot_[domain].csv` | Same data in spreadsheet format |
| Analysis report | Rating distribution, positive/negative themes, PPC insights, notable quotes |

## Install as Claude Code Skill

```bash
# Copy into your skills directory
cp -r trustpilot-scraper ~/.claude/skills/trustpilot-scraper

# Or for a project
cp -r trustpilot-scraper your-project/.claude/skills/trustpilot-scraper
```

Then tell Claude: `scrape trustpilot reviews for example.org`

## Setup

The scraper and its dependencies are bundled. Just install:

```bash
cd ~/.claude/skills/trustpilot-scraper
npm install
```

This installs Puppeteer with stealth plugin for reliable, undetected scraping.

## Usage

```
/trustpilot example.org
/trustpilot acme-corp example.org
/trustpilot acme-corp example.org --search "bad service" --max 200
```

### Options

| Flag | Description |
|------|-------------|
| First arg | Domain as it appears on Trustpilot (e.g., `example.org`) |
| Client name (optional) | Saves output to `clients/[client]/trustpilot/` |
| `--search [term]` | Filter reviews by keyword |
| `--max [number]` | Maximum reviews to collect |
| `--pages [number]` | Maximum pages to scrape |

## What the Analysis Covers

### Rating Distribution
Breakdown of 1-5 star reviews with percentages and average.

### Sentiment Themes
- **Positive themes** (4-5 star): What customers love, with frequency counts and quotes
- **Negative themes** (1-2 star): Recurring complaints and objections
- **Mixed signals** (3 star): What "almost" works — improvement opportunities

### PPC-Actionable Insights
| Category | What You Get |
|----------|-------------|
| Ad Copy Angles | Top positive themes to use as headlines/descriptions |
| Social Proof Lines | Short, punchy quotes suitable for ad copy |
| Objection Handling | Top complaints to address proactively in ads/landing pages |
| Keyword Signals | Language customers use naturally (potential keywords) |
| Competitor Weaknesses | Mentions of switching from competitors |
| Trust Signals | Specific numbers (response time, years, percentages) |

### Notable Quotes
5-10 standout review quotes that are short enough for ads, specific, and emotionally compelling.

## Requirements

- Node.js 18+
- Dependencies installed via `npm install` (Puppeteer + stealth plugin)

## License

MIT

## Author

Built by [Kuda Chinhara](https://linkedin.com/in/kudachinhara) at [Agentic PPC Ads](https://agenticppcads.com).

We build AI-powered Google Ads tools for agencies and in-house teams.

- Web: [agenticppcads.com](https://agenticppcads.com)
- LinkedIn: [linkedin.com/in/kudachinhara](https://linkedin.com/in/kudachinhara)
- X: [@kudachinhara](https://x.com/kudachinhara)
