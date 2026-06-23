[Google News Scraper](https://apify.com/taroyamada/google-news-scraper?fpr=data)

# 📰 Google News Scraper

Discover fresh article URLs from Google News RSS for topic monitoring, PR workflows, and content-intelligence pipelines. This actor is a **discovery surface** in the Content Intelligence Pack: use it to find news URLs, then hand those URLs to **Article Content Extractor** for full-body cleanup.

## Store Quickstart

- Start with **Quickstart (company news)** for a reliable first run.
- Use **Brand Monitoring** to track multiple companies or themes.
- Use **Google News → Article Cleanup** when your next step is article extraction.

## Where this actor fits

| Surface | Best for |
| --- | --- |
| **Google News Scraper** | Discover current article URLs by query |
| **Article Content Extractor** | Clean the discovered article/news/blog pages |
| **Website Content Extractor** | Clean discovered non-article pages |
| **RSS Feed Aggregator** | Discover fresh URLs from known publishers and blogs |

## Key Features

- 🔎 **Query-based discovery** — Pull article URLs from Google News RSS without a paid API
- 🌍 **Localized results** — Tune by `language` and `country`
- 🔄 **Deduplication** — Remove duplicate URLs across multiple queries
- 📰 **Publisher context** — Keep headline, source, description, and publish date
- ⚡ **Fast feeder step** — Lightweight discovery before deeper extraction

## Use Cases

| Who | Why |
| --- | --- |
| PR teams | Find the latest media mentions to hand off for cleanup |
| Competitive intelligence | Build newsroom watchlists from search queries |
| Content ops | Discover trending stories before enrichment |
| AI / RAG teams | Create a steady article URL feed for downstream extraction |

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `queries` | `string[]` | required | Search queries (max 50) |
| `language` | `string` | `en` | Google News language code |
| `country` | `string` | `US` | Google News country code |
| `maxItems` | `integer` | `25` | Max articles per query |
| `deduplicate` | `boolean` | `true` | Remove duplicate links across queries |
| `timeoutMs` | `integer` | `15000` | Request timeout |
| `delivery` | `string` | `dataset` | `dataset` or `webhook` |
| `webhookUrl` | `string` | — | Webhook target when `delivery=webhook` |
| `dryRun` | `boolean` | `false` | Run without saving |

### Input Example

```
{
  "queries": ["OpenAI", "Google AI"],
  "language": "en",
  "country": "US",
  "maxItems": 20,
  "deduplicate": true
}
```

## Output

| Field | Type | Description |
| --- | --- | --- |
| `title` | string | Article headline |
| `link` | string | Direct article URL for downstream cleanup |
| `source` | string | Publisher name |
| `pubDate` | string | Original RSS publish date |
| `pubDateISO` | string | ISO timestamp version of `pubDate` |
| `description` | string | Short Google News snippet |
| `query` | string | Search query that surfaced the row |

### Output Example

```
{
  "title": "Codex for (almost) everything",
  "link": "https://openai.com/index/codex-for-almost-everything",
  "source": "OpenAI",
  "pubDate": "Thu, 16 Apr 2026 10:00:00 GMT",
  "pubDateISO": "2026-04-16T10:00:00.000Z",
  "description": "The updated Codex app for macOS and Windows adds computer use...",
  "query": "OpenAI"
}
```

## First-run buyer experience

1. Run **Quickstart (company news)**.
2. Confirm the dataset shows real article URLs, not generic homepages.
3. Pick the top URLs and send them to **Article Content Extractor**.
4. If a discovered URL is actually a docs/product/policy page, clean it with **Website Content Extractor** instead.

## Tips & Limitations

- Use broad queries for the first run; refine later.
- RSS is a discovery layer only — it does not return full article bodies.
- Combine multiple narrower queries instead of one overloaded boolean query when relevance matters.

## FAQ

**Can I get full article text here?**

No. This actor discovers URLs and returns metadata only. Use Article Content Extractor for full content.

**Why use this instead of scraping the Google News UI?**

The RSS surface is lighter, more stable, and better suited for recurring discovery runs.

**Can I schedule recurring news monitoring?**

Yes — run it on a schedule, then pass the discovered URLs into an article-cleanup step.

## Related Actors

Content Intelligence Pack handoffs:

- [📰 Article Content Extractor](https://apify.com/taroyamada/article-content-extractor) — clean newsroom and blog article pages
- [📄 Website Content Extractor](https://apify.com/taroyamada/website-content-extractor) — clean discovered non-article pages
- [📡 RSS Feed Aggregator](https://apify.com/taroyamada/rss-feed-aggregator) — discover fresh URLs from known publisher feeds

## Cost

**Pay Per Event**:

- `actor-start`: $0.01
- `dataset-item`: $0.003 per output item

 

## ⭐ Was this helpful?

If this actor saved you time, please [leave a ★ rating](https://apify.com/taroyamada/google-news-scraper/reviews) on Apify Store.