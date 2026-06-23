[Google News Scraper](https://apify.com/knotless_cadence/google-news-scraper?fpr=data)

# Google News Scraper

Scrape Google News articles by keyword — extract headlines, URLs, publication dates, source names, and descriptions. Monitor news coverage for any topic, brand, or industry in real-time.

## Features

- **Real-time news data** — scrape the latest articles from Google News RSS feeds
- **Multiple search queries** — process several topics in a single run
- **Time range filters** — filter articles by hour, day, week, month, or year
- **15 languages × 15 country regions** — exact set defined in input_schema.json (en/es/fr/de/it/pt/ru/ja/ko/zh-CN/ar/hi/tr/pl/nl × US/GB/CA/AU/DE/FR/ES/IT/BR/IN/JP/KR/RU/MX/TR)
- **Source attribution** — extract publisher name and URL for each article
- **HTML fallback** — automatically switches to HTML parsing if RSS is unavailable
- **Clean output** — HTML tags stripped from titles and descriptions

## Output Example

```
{
 "title": "OpenAI Announces GPT-5 with Advanced Reasoning Capabilities",
 "url": "https://www.reuters.com/technology/openai-announces.",
 "publishedAt": "2026-03-17T14:30:00.000Z",
 "description": "OpenAI unveiled its latest language model on Monday, featuring significant improvements in mathematical reasoning and code generation.",
 "source": "Reuters",
 "sourceUrl": "https://www.reuters.com",
 "query": "artificial intelligence",
 "scrapedAt": "2026-03-18T12:00:00.000Z"
}
```

**Field reference (8 fields per article via RSS branch):** `title`, `url`, `publishedAt`, `description`, `source`, `sourceUrl`, `query`, `scrapedAt`. The HTML fallback branch returns 6 fields (no `description`, no `sourceUrl`) — used only when Google News RSS is empty for the query.

## Use Cases

- **Media monitoring** — track news coverage of your brand, product, or industry across hundreds of sources
- **Competitive intelligence** — monitor competitor announcements, press releases, and media mentions
- **Trend tracking** — spot emerging stories in real-time, before they hit Twitter
- **PR measurement** — count brand mentions vs competitors (article-count signal, not weighted share-of-voice)
- **Content sourcing** — feed news headlines into LLM pipelines, briefing tools, or RSS replacements

## Input Parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `searchQueries` | Array | `["artificial intelligence"]` | Keywords or phrases to search for in Google News |
| `language` | String | `"en"` | Language code (en, es, fr, de, ru, etc.) |
| `country` | String | `"US"` | Country code for regional news (US, GB, DE, FR, etc.) |
| `maxArticlesPerQuery` | Number | `50` | Maximum articles to extract per search query |
| `timeRange` | String | `"week"` | Time filter: hour, day, week, month, year |

## Pricing

Standard Apify per-run compute pricing — no per-article fee. Compute cost depends on Google News response latency (typical: a few CU per query). Use `maxArticlesPerQuery` to cap output size.

## Step-by-Step Tutorial

### 1. Open Google News Scraper on Apify

Go to [Google News Scraper](https://apify.com/knotless_cadence/google-news-scraper) and click "Try for free."

### 2. Configure Your Search

Enter your search queries. Examples:

- Brand monitoring: `["Tesla", "Tesla stock", "Elon Musk"]`
- Industry tracking: `["electric vehicles", "EV charging", "battery technology"]`
- Competitor intelligence: `["competitor-name"]`

### 3. Set Filters

Choose time range (hour/day/week/month/year) and language/country for localized results.

### 4. Run and Download

Click "Start." Results are available as JSON, CSV, or Excel in under 30 seconds.

## Technical Details

This scraper uses **Google News RSS feeds** — the most reliable method for news data extraction. RSS feeds:

- Never break on website redesigns
- Return structured XML with consistent format
- Don't require JavaScript rendering
- Work without proxy or authentication

The fallback HTML parser activates only if RSS is unavailable for a specific query.

**Method:** `https://news.google.com/rss/search?q={query}+when:{first-letter-of-timerange}&hl={language}&gl={country}&ceid={country}:{language}`

**Honest behavioral notes:**

- `timeRange` is encoded as `+when:h|d|w|m|y` appended to the query (one letter), NOT as a separate URL param. There is also an unused `tbs=qdr:*` map in code that is not appended to the request URL — only the `+when:` form is sent.
- `description` field is truncated to 500 characters via `.substring(0, 500)`. Long article summaries are cut off mid-sentence.
- The HTML fallback branch (`article, [data-n-tid], .NiLAwe` selectors) only activates if RSS returns 0 items for that query. Google News rarely serves HTML to the RSS endpoint, so in practice this branch is exercised only on edge cases.
- No proxy / no retry. A transient 5xx from Google News leaves that query with 0 articles emitted.
- HTML tags are stripped from `title` and `description` via `.replace(/<[^>]*>/g, '')` — fast, but not a full HTML decoder (entities like `&amp;` are NOT decoded).

## Integration Examples

### With n8n (no-code automation)

1. Add HTTP Request node → call Apify API
2. Parse JSON response
3. Send to Slack/Email/Google Sheets

### With Python

```
import requests
response = requests.get(
 "https://api.apify.com/v2/acts/knotless_cadence~google-news-scraper/runs/last/dataset/items",
 params={"token": "YOUR_TOKEN"}
)
articles = response.json()
```

### With JavaScript

```
const response = await fetch(
 `https://api.apify.com/v2/acts/knotless_cadence~google-news-scraper/runs/last/dataset/items?token=YOUR_TOKEN`
);
const articles = await response.json();
```

## FAQ

**Q: How fresh are the news articles?**
A: Articles are scraped from Google News in real-time. Use `timeRange: "hour"` to get only the most recent stories. Publication dates come directly from Google News.

**Q: Can I scrape news in languages other than English?**
A: Yes. Set the `language` and `country` parameters to get localized results. For example, `language: "de"` and `country: "DE"` for German news.

**Q: Does it follow links to the original articles?**
A: The scraper extracts the original article URL as provided by Google News. It does not visit or scrape the full text from source websites — for that, combine this scraper with a web content extractor.

**Q: How often does Google News update?**
A: Articles appear within minutes of publication. Use `timeRange: "hour"` for the freshest results.

**Q: What languages are supported?**
A: 15 languages × 15 country regions (full list in input_schema.json). Examples: `de/DE` for German news, `ja/JP` for Japanese, `ru/RU` for Russian. Combinations not in the enum will be rejected by the input validator.

**Q: Does it follow links to full article text?**
A: No — it extracts metadata (title, source, date, description, URL). For full text, combine with a web content extractor.

**Q: How reliable is it?**
A: Uses Google News RSS feeds — the most stable data format. RSS hasn't changed in years. No JavaScript rendering needed.

**Q: Can I monitor multiple topics?**
A: Yes. Pass an array of search queries and process them all in one run.

---

*Part of 78 data tools (31 published) by knotless_cadence on Apify. Related tools:*

- [Walmart Reviews Scraper](https://apify.com/knotless_cadence/walmart-reviews-scraper) — Product reviews to CSV/JSON/Excel, 17 fields per review, bypasses Walmart's 100-review UI cap
- [Trustpilot Review Scraper](https://apify.com/knotless_cadence/trustpilot-review-scraper) — 951 lifetime production runs, full review schema export
- [Hacker News Scraper](https://apify.com/knotless_cadence/hacker-news-scraper) — Scrape top stories, comments, and user data from Hacker News
- [MCP Trend Detector](https://apify.com/knotless_cadence/mcp-trend-detector) — AI-powered trend detection across news and social media sources
- [Bluesky Scraper](https://apify.com/knotless_cadence/bluesky-scraper) — Scrape posts and profile data from Bluesky social network

---

**Proof of delivery**: This Google News scraper has **45 lifetime production runs** as of May 2026. Author maintains 31 published actors (78 total) and shipped a paid 3-article series in March 2026 ($150, proxy industry). Pilot pricing locked through **May 2026**.

**Sample request?** Reply `sample` to [spinov001@gmail.com](mailto:spinov001@gmail.com) and we'll send 2 published case-study articles within 24 hours.

## Custom scraping — pilot tiers

Need data, not infrastructure. We build, you query. Three tiers:

- **Pilot — $97** · 1 actor, basic config, 7-day support. Good entry point for one-off jobs.
- **Standard — $297** · custom actor + Slack/email alerts on results, 30-day support. Most projects fit here.
- **Premium — $797** · custom actor + dashboard + 90-day support + 1 modification round. For ongoing data pipelines.

**Email:** [spinov001@gmail.com](mailto:spinov001@gmail.com) — drop specs, schema, or target URLs and get a quote within 48h.

**Proof of work:** [31 published Apify scrapers](https://apify.com/knotless_cadence) (78 total in portfolio) — Trustpilot 951 runs, Reddit 82, Google News 45, Email Extractor 107. Recently delivered a paid 3-article series for a client in the proxy industry ($150).

**More tips:** [t.me/scraping_ai](https://t.me/scraping_ai) · [blog.spinov.online](https://blog.spinov.online)

---

*Honest disclosure: this scraper relies on Google News public RSS endpoints — no scraping behind login walls, no personal data, robots.txt respected. Not affiliated with Google.*