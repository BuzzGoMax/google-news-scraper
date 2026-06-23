[Google News Scraper](https://apify.com/epicscrapers/google-news-scraper?fpr=data)

# Google News Scraper

Gets featured articles from Google News with title, link, source, publication date and image.

## What does Google News Scraper do?

**Google News Scraper** is a powerful tool that queries the Google News RSS API to collect news article metadata—headlines, article URLs, publisher/source, publication timestamps, and preview image URLs. It can optionally fetch full article pages to decode final links and extract images.

This Actor is perfect for:

- **News aggregation** - Collect articles from multiple sources on specific topics
- **Media monitoring** - Track mentions of your brand, competitors, or industry keywords
- **Market research** - Analyze news trends and sentiment over time
- **Content curation** - Gather articles for newsletters or content platforms
- **Academic research** - Collect news data for analysis and studies

Unlike manually browsing Google News (which limits you to ~100 results per search), this Actor can retrieve significantly more results by automatically iterating through date ranges day by day.

## Why use Google News Scraper?

- **Bypass result limits** - Get more than the standard 100 results per search
- **Advanced search operators** - Use `intitle`, `inurl`, `site`, exact match, exclusions, and boolean operators
- **Date range filtering** - Search specific time windows or open-ended ranges (last hour, day, week, year)
- **Topic-based search** - Scrape predefined topics or use hashed topic IDs for specific sections
- **Multilingual support** - Search in 50+ languages and regions
- **Cost-effective** - Uses RSS API instead of browser automation for faster, cheaper runs
- **Flexible output** - Get simple RSS links or enriched data with decoded URLs and images

## How to use Google News Scraper

### 1. Query-based search

Enter a search query as you would in Google News. You can use advanced operators:

| Operator | Example | Description |
| --- | --- | --- |
| `intitle:` | `intitle:"AI"` | Find articles with keyword in title |
| `site:` | `site:bbc.com` | Search within specific site |
| `""` | `"climate change"` | Exact phrase match |
| `-` | `apple -fruit` | Exclude term |
| `AND` / `OR` | `AI AND (ethics OR regulation)` | Boolean operators |

**Example queries:**

- `intitle:"AI" AND site:forbes.com` - AI articles from Forbes
- `site:reuters.com "stock market" -crypto` - Stock market news excluding crypto
- `"Samsung Galaxy S25" AND (review OR comparison)` - Reviews or comparisons

### 2. Topic-based search

Select predefined topics or use hashed topic IDs:

**Predefined topics:**

- WORLD 🌎, NATION 🚩, BUSINESS 🪙, TECHNOLOGY 💻, ENTERTAINMENT 🎸, SPORTS 🏒, SCIENCE 🧪, HEALTH 🧑‍⚕️

**Hashed topics:**
Copy topic IDs from Google News URLs for custom topics. Example:

```
CAAqJggKIiBDQkFTRWdvSUwyMHZNRGRqTVhZU0FtVnVHZ0pWVXlnQVAB/sections/CAQiQ0NCQVNMQW9JTDIwdk1EZGpNWFlTQW1WdUdnSlZVeUlOQ0FRYUNRb0hMMjB2TUcxcmVpb0pFZ2N2YlM4d2JXdDZLQUEqKggAKiYICiIgQ0JBU0Vnb0lMMjB2TURkak1YWVNBbVZ1R2dKVlV5Z0FQAVAB
```

This targets "Technology > Artificial Intelligence" section.

### 3. Configure date range

**Option A: Fixed dates**

- `Date from`: 2024-01-01
- `Date to`: 2024-01-31

**Option B: Open-ended range**

- `1h` - Last hour
- `24h` - Last 24 hours
- `7d` - Last week
- `30d` - Last month
- `1y` - Last year

### 4. Set language and region

Choose from 50+ options like:

- `US:en` - United States (English)
- `GB:en` - United Kingdom (English)
- `DE:de` - Germany (German)
- `FR:fr` - France (French)
- `JP:ja` - Japan (Japanese)

### 5. Choose output detail level

**Fetch article details: ON**

- Decodes RSS links to actual article URLs
- Extracts preview images from article pages
- Slower but more informative

**Fetch article details: OFF**

- Returns RSS feed links only
- Much faster and cheaper
- No images, encoded URLs

## Input

| Field | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `query` | string | No | - | Search query with optional advanced operators |
| `topics` | array | No | [] | Predefined topics (WORLD, NATION, BUSINESS, etc.) |
| `topicsHashed` | array | No | [] | Hashed topic IDs from Google News URLs |
| `language` | string | Yes | US:en | Language and region pair |
| `maxItems` | integer | No | - | Maximum number of items to scrape |
| `fetchArticleDetails` | boolean | No | true | Decode RSS links and fetch images |
| `dateFrom` | string | No | - | Start date (YYYY-MM-DD) |
| `dateTo` | string | No | - | End date (YYYY-MM-DD) |
| `openEndedDateRange` | string | No | - | Open-ended range (e.g., 1h, 7d, 1y) |
| `proxyConfiguration` | object | No | `{useApifyProxy: true}` | Proxy settings |

### Example Input (JSON)

```
{
  "query": "intitle:\"artificial intelligence\" AND site:techcrunch.com",
  "language": "US:en",
  "maxItems": 50,
  "fetchArticleDetails": true,
  "openEndedDateRange": "7d",
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

## Output

The Actor stores results in a dataset. You can download data in JSON, HTML, CSV, or Excel formats.

### Output Schema

```
{
  "title": "Article headline",
  "link": "Direct article URL (decoded if fetchArticleDetails=true)",
  "guid": "Unique article identifier",
  "source": "Publisher name (e.g., 'BBC News')",
  "sourceUrl": "Publisher website URL",
  "publishedAt": "2024-01-15T14:30:00.000Z",
  "loadedUrl": "Final URL after redirects",
  "rssLink": "Original RSS feed link",
  "image": "Preview image URL (if available)"
}
```

### Example Output

```
[
  {
    "title": "Web Scraping Optimization: Tips for Faster, Smarter Scrapers",
    "link": "https://hackernoon.com/web-scraping-optimization-tips-for-faster-smarter-scrapers",
    "guid": "CBMiiAFBVV95cUxQUXh5WVZ2RkNpNG9ndjF6V3hMRHBRTGRSVnNkelpwZDY2TWJzejBSMGZrRC1rSm5DZ1BxanpoeFFGdDRjWGpZR0tOUG9FY0kyeWFXOE9MSzBobTg1ajRiZzVhSWhtbm5nSVNJVWExSDBSaEFjUUJkT1JRRDJHSDBrMU9jU2ZZN3RN",
    "source": "hackernoon.com",
    "sourceUrl": "https://hackernoon.com",
    "publishedAt": "2024-11-15T08:00:00.000Z",
    "loadedUrl": "https://hackernoon.com/web-scraping-optimization-tips-for-faster-smarter-scrapers",
    "rssLink": "https://news.google.com/rss/articles/CBMiiAFBVV95cUxQUXh5WVZ2RkNpNG9ndjF6V3hMRHBRTGRSVnNkelpwZDY2TWJzejBSMGZrRC1rSm5DZ1BxanpoeFFGdDRjWGpZR0tOUG9FY0kyeWFXOE9MSzBobTg1ajRiZzVhSWhtbm5nSVNJVWExSDBSaEFjUUJkT1JRRDJHSDBrMU9jU2ZZN3RN?oc=5",
    "image": "https://hackernoon.imgix.net/images/0FC9YtxD4fbD3T7mPipOt4HSxY42-7y034nb.png"
  }
]
```

## Data table

| Field | Type | Description |
| --- | --- | --- |
| `title` | string | Article headline |
| `link` | string | Direct article URL |
| `guid` | string | Unique article identifier from RSS |
| `source` | string | News publisher name |
| `sourceUrl` | string | Publisher's website URL |
| `publishedAt` | string (ISO 8601) | Publication timestamp |
| `loadedUrl` | string | Final URL after following redirects |
| `rssLink` | string | Original Google News RSS link |
| `image` | string | Preview image URL (if fetched) |

## Pricing

This Actor is priced at **$20/month** with a **7-day free trial** (10,080 minutes).

**Cost estimation:**

| Scenario | Articles | With Article Details | Est. Cost |
| --- | --- | --- | --- |
| Quick search | 100 | No | ~$0.01 |
| Medium search | 500 | Yes | ~$0.05-0.10 |
| Large search | 2000 | Yes | ~$0.20-0.40 |

*Note: Costs include Apify platform usage (compute units, proxy). Actual costs vary based on proxy type and run duration.*

**Tips to reduce costs:**

1. Set `fetchArticleDetails` to `false` if you only need RSS links (~10x faster)
2. Use `maxItems` to limit results
3. Use shorter date ranges for focused searches
4. Use the FREE plan's monthly credits for small runs

## Advanced Tips

### Combining search methods

You can combine `query`, `topics`, and `topicsHashed` in a single run. The Actor will search each independently and merge results.

### Finding hashed topic IDs

1. Go to [Google News](https://news.google.com)
2. Navigate to a topic or section
3. Copy the ID from the URL after `/topics/`
4. For sections, include the `/sections/` part too

### Handling large result sets

If you need thousands of articles:

1. Set `maxItems` to your desired count
2. The Actor automatically iterates day-by-day when maxItems > 100
3. Use broader date ranges for historical data
4. Consider running multiple Actors with different queries in parallel

### Language/region tricks

Get different perspectives on the same topic:

- Search US news in German: `language: DE:de` + US topic hash
- Search international news about Japan in English: `language: US:en` + Japan topic hash

## FAQ and Support

**Is scraping Google News legal?**
This Actor uses the public Google News RSS API, which is designed for syndication. However, you should:

- Respect robots.txt and terms of service
- Not overwhelm the service with excessive requests
- Use extracted data in compliance with copyright laws
- Consider data privacy regulations (GDPR, CCPA) when storing/processing data

**Why are some images missing?**
Images are extracted from article pages using Open Graph and Twitter Card metadata. Not all websites provide this data. The image URL comes directly from the publisher's site, not Google News.

**Can I get full article text?**
This Actor extracts metadata only. For full article text, use the extracted `link` field with a dedicated article scraper like [Smart Article Extractor](https://apify.com/lukaskrivka/article-extractor-smart).

**What if I get blocked?**
The Actor uses Apify Proxy by default. If you experience issues:

1. Check your proxy settings
2. Reduce concurrency (built-in default is conservative)
3. Use shorter date ranges
4. Enable `fetchArticleDetails` only when needed

**Need help?**

- Report issues via the [Issues tab](https://console.apify.com/actors/YOUR-ACTOR-ID/issues)
- Join [Apify Discord](https://discord.com/invite/jyEM2PRvMU) for community support
- Contact [Apify Support](https://support.apify.com) for platform issues

## Resources

- [Apify SDK for JavaScript documentation](https://docs.apify.com/sdk/js)
- [Crawlee documentation](https://crawlee.dev/)
- [Google Search Operators Guide](https://www.googleguide.com/advanced_operators_reference.html)
- [Apify Proxy documentation](https://docs.apify.com/platform/proxy)