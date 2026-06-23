[Google News Scraper](https://apify.com/scrape.badger/google-news-scraper?fpr=data)

## What does Google News Scraper do?

Scrape [Google News](https://news.google.com) at scale — keyword search, topic feeds (world, business, tech, sports, …), and country-level Top Stories trending feeds.

## Why use Google News Scraper?

- **Three modes.** Search (keyword), Topic Feed (7 predefined), Trending (country Top Stories).
- **Country + language targeting.** `gl` + `hl` for localised news editions.
- **Each article as its own record.** Stream-friendly — one article per dataset row with source, publish date, link, snippet.
- **No duplicates.** Pagination dedupes across pages automatically.
- **Fast.** Lightweight JSON responses, ≈ 1s per call.

## What data can Google News Scraper extract?

| Field | Type | Description |
| --- | --- | --- |
| title | string | Article headline |
| source | string | Publisher name |
| published_at | string | ISO 8601 timestamp |
| link | string | Canonical article URL |
| snippet | string | Lead paragraph preview |
| thumbnail | string | Article image (when present) |
| topic | string | Google topic ID (in Topic Feed mode) |

## How to scrape Google News

1. Click **Try for free**.
2. Pick a `mode`: Search, Topic Feed, or Trending.
3. For Search: enter `q`. For Topic Feed: pick a topic (WORLD / BUSINESS / TECHNOLOGY / …). For Trending: just set country.
4. Set `gl` and `hl` for the local edition.
5. Click **Start** — articles stream into the dataset.

## How much will it cost?

**$0.002 per page (≈ $2 per 1,000 pages).** One call per page; each page returns ≈ 50-100 articles depending on mode. A typical 3-page Search run pushes ~200 articles for $0.006.

### Competitor benchmark

| Actor | Author | Price | Notes |
| --- | --- | --- | --- |
| lhotanova/google-news-scraper | lhotanova | ~$4 / 1k articles | Search-only |
| easyapi/google-news-api-scraper | easyapi | ~$3 / 1k articles | Search-only |
| apify/google-search-scraper | Apify | ~$3.50 / 1k pages | News as SERP block |
| **scrape-badger/google-news-scraper** | **ScrapeBadger** | **$2 / 1k pages** | **Search + Topics + Trending in one** |

## Input

Configure the run in the **Input** tab above, or pass a JSON object matching the fields below when calling the Actor via the Apify API.

| Field | Required | Description |
| --- | --- | --- |
| mode | ✅ | `Search` / `Topic Feed` / `Trending`. |
| q | Search only | Search query. |
| topic | Topic Feed only | WORLD / BUSINESS / TECHNOLOGY / ENTERTAINMENT / SPORTS / SCIENCE / HEALTH. |
| gl / hl | — | Country + language. |
| max_results | — | Result cap across the full paginated run. |

## Output

Every successful run streams records into the run's dataset. Download as JSON, CSV, XML, Excel, or HTML from the **Dataset** tab; consume programmatically via the Apify API or webhooks.

Example record:

```
{
  "title": "Major breakthrough in quantum computing",
  "source": "TechCrunch",
  "published_at": "2026-04-22T09:30:00Z",
  "link": "https://techcrunch.com/\u2026",
  "snippet": "Researchers at MIT announced today\u2026",
  "thumbnail": "https://news.google.com/\u2026"
}
```

## Tips / Advanced options

- **Use Topic Feed for broad monitoring.** Cron a daily Topic Feed run for Technology / Business — covers the full industry automatically.
- **Use Trending for viral stories.** Top Stories per country, refreshed every hour by Google.
- **Pair with entity-extraction.** Pipe the `snippet` field through an LLM or NER service to tag companies, people, and topics.
- **Deduplicate by `link`.** The canonical URL is stable across runs — use it as the primary key in your downstream DB.

## FAQ, Disclaimers, Support

### Can I filter by date range?

Search mode accepts Google's `tbs=qdr:d` (24h) / `qdr:w` (week) via the SDK. Topic Feed and Trending are always current.

### What's the article body?

Google News only returns title, snippet, and link. Fetch the full body from `link` via a separate HTTP call (or use `google-scraper` for the full SERP).

### How many articles per page?

≈ 50-100 depending on mode — `Topic Feed` typically returns more than `Search`.

### Can I bypass paywalls?

No — `link` resolves to the publisher's canonical URL. Paywalls apply to the actual article, not to Google News metadata.

### Disclaimer

This Actor scrapes public Google data only. You're responsible for compliance with Google's Terms of Service and any applicable data-protection laws (GDPR, CCPA, etc.) in your jurisdiction. ScrapeBadger does not store the scraped results — they are delivered directly to your Apify dataset.

### Support

Something not working? Open a ticket in the **Issues** tab above — we triage within one business day. Full API reference: [docs.scrapebadger.com](https://docs.scrapebadger.com).

### Related Actors

- [`google-search-scraper`](https://apify.com/scrape-badger/google-search-scraper) — SERP with inline News block
- [`google-trends-scraper`](https://apify.com/scrape-badger/google-trends-scraper) — Search-interest trend data

### Powered by

[ScrapeBadger](https://scrapebadger.com) — Google-optimised residential proxy pool + browser-farm fallback, 99.7% uptime, unmetered bandwidth. No CAPTCHAs reach you.