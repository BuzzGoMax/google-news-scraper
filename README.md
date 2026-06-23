[Google News Scraper](https://apify.com/nexgendata/google-news-scraper?fpr=data)

# Google News Scraper

Scrapes Google News for the latest articles, headlines, and news coverage on any topic or region.

## Features

- Fast, reliable data extraction
- Structured JSON output
- Handles pagination automatically
- Built-in retry and error handling
- Pay only for results delivered

## Use Cases

| Use Case | Description |
| --- | --- |
| News monitoring | Track news coverage of your brand |
| PR tracking | Monitor media mentions |
| Market intelligence | Follow industry news |
| Content research | Find trending news topics |

## Input Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `searchQuery` | string | News search query |
| `maxItems` | integer | Max articles to return |
| `language` | string | Language code (en, es, etc.) |

## Output Example

Results are returned as structured JSON with fields including: `title`, `source`, `publishDate`, `snippet`, `url`, `imageUrl`.

## Pricing

This actor uses pay-per-event pricing:

- **Actor start**: $0.00005 per run
- **Results**: $3 per 1,000 results ($0.00300 per result)

You only pay for the data you receive. No monthly fees, no commitments.

## Tips for Best Results

1. Start with a small test run to verify the output matches your needs
2. Use specific search queries to get more relevant results
3. Set appropriate `maxItems` limits to control costs
4. Results are delivered in JSON format, ready for processing

## Support

Found a bug or need help? Open an issue on the actor's GitHub page or contact us through Apify.

---

 

## 💻 Code Example — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("nexgendata/google-news-scraper").call(run_input={
    # Fill in the input shape from the actor's input_schema
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

## 🌐 Code Example — cURL

```
curl -X POST "https://api.apify.com/v2/acts/nexgendata~google-news-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ /* input schema */ }'
```

## ❓ FAQ

**Q: How do I get started?**
Sign up at [apify.com](https://www.apify.com/?fpr=2ayu9b), grab your API token from Settings → Integrations, and run the actor via the Apify console, API, Python SDK, or any integration (Zapier, Make.com, n8n).

**Q: What's the typical cost per run?**
See the pricing section below. Most runs finish under $0.10 for typical batches.

**Q: Is this actor maintained?**
Yes. NexGenData maintains 165+ Apify actors and ships updates regularly. Bug reports via the Apify console issues tab get responses within 24 hours.

**Q: Can I use the output commercially?**
Yes — you own the output data. Check the target site's Terms of Service for any usage restrictions on the scraped content itself.

**Q: How do I handle rate limits?**
Apify manages concurrency and retries automatically. For very large batches (10K+ items), run multiple smaller jobs in parallel instead of one mega-job for better reliability.

## 💰 Pricing

Pay-per-event pricing — you only pay for what you actually extract.

- **Actor Start:** $0.0001
- **result:** $0.0030

## 🔗 Related NexGenData Actors

- [RAG Web Browser](https://apify.com/nexgendata/rag-web-browser?fpr=2ayu9b)
- [AI Web Scraper](https://apify.com/nexgendata/ai-web-scraper?fpr=2ayu9b)
- [Hacker News Scraper](https://apify.com/nexgendata/hacker-news-scraper?fpr=2ayu9b)

## 🚀 Apify Affiliate Program

New to Apify? Sign up with our [referral link](https://www.apify.com/?fpr=2ayu9b) — you get free platform credits on signup, and you help fund the maintenance of this actor fleet.

## 📚 More From NexGenData

Explore the full catalog, tutorials, Gumroad data packs, and newsletter at **[thenextgennexus.com](https://thenextgennexus.com)** — the brand home for everything we ship.

- 📖 Tutorials & how-to guides
- 🗂️ Full actor catalog with usage examples
- 📦 Gumroad data packs (one-time purchases)
- 📬 Newsletter — monthly drops of new actors and revenue experiments

---

*Built and maintained by [NexGenData](https://apify.com/nexgendata?fpr=2ayu9b) — 165+ actors covering scraping, enrichment, MCP servers, and automation.*
🏠 Home: [thenextgennexus.com](https://thenextgennexus.com)