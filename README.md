# Houzz Pros Scraper [Just 💰$0.9]

Extract structured data from [houzz.com](https://houzz.com) — houzz.com professionals by category and city, or paste a search URL. Every record carries the phone number, street address and star rating the directory publishes. Filter by distance from a point. Structured rows, ready for a CRM or a spreadsheet.

**[Houzz Pros Scraper [Just 💰$0.9] on Apify →](https://apify.com/blackfalcondata/houzz-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch contact information, structured metadata for each professional.

**Incremental mode** — Only get new or changed professionals since your last run. Content hash per professional — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how professionals evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N professionals (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every professional returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured professionals from houzz.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor professionals, track trends, and analyze market dynamics with structured, deduplicated data from houzz.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per professional is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "general-contractors",
  "location": "Austin, TX",
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | A houzz.com professionals URL, a category slug (e.g. general-contractors, architects, interior-designers-and-decorators), or free text. Required if no startUrls provided. |
| `location` | string | — | City and state, e.g. "Austin, TX". Works with a category slug or a profession keyword (e.g. general contractor, which houzz.com maps to its category); a location the site does not recognize stops the run with an error instead of searching the whole country. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `includeDetails` | boolean | `true` | Open each professional's profile for its website, areas served, services, awards, license number and review-aspect scores. Turn off for search-card data only (faster). |
| `includeReviews` | boolean | `false` | Add the professional's reviews (text, rating, reviewer, project date and price, likes, the pro's reply) to each record. Adds one request per 50 reviews, so runs take longer. |
| `maxReviewsPerPro` | integer | `20` | How many reviews to read per professional when reviews are on, in the order houzz.com shows them. |
| `enrichEmails` | boolean | `false` | Read the professional's own website (home page, plus one contact or about page if needed) and add the email addresses it shows. houzz.com itself does not publish emails. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `lat` | number | — | Latitude of the centre point for the search radius. Pair with lon and radiusKm. |
| `lon` | number | — | Longitude of the centre point for the search radius. Pair with lat and radiusKm. |
| `radiusKm` | integer | — | Keep only professionals whose business address lies within this many kilometres of lat/lon. Professionals without a mapped address are left out. Combine with a location near the point. |

---

## Output fields

Every professional returns the same 71-field schema. Missing values are `null` — never omitted.

- `listingId`
- `id`
- `title`
- `entityName`
- `location`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`
- `portalUrl`
- `search_url`
- `page_url`
- `page_number`
- `page_offset`
- `primary_topic_id`
- `sort_value`
- `total_results`
- `has_more_results`
- `scraped_at`
- `professional_id`
- `user_id`
- `pro_id`
- `user_name`
- `name`
- `first_name`
- `last_name`
- `professional_type`
- `pro_type_id`
- `pro_type_labels`
- `pro_sku_id`
- `profile_url`
- `image_url`
- `phone`
- `about_me`
- `social_links`
- `formatted_address`
- `address`
- `street_address`
- `city`
- `state`
- `zip`
- `country`
- `latitude`
- `longitude`
- `serves`
- `distance_miles`
- `rating`
- `review_count`
- `most_recent_review_text`
- `most_recent_reviewer`
- `featured_review_text`
- `badges`
- `badge_option_ids`
- `is_best_of_houzz`
- `local_project_count`
- `cost_estimate`
- `budget_levels`
- `is_paid_pro`
- `is_pro_verified`
- `is_pro_verification_enabled`
- `has_verified_license`
- `has_verified_kyc`
- `is_video_consultation_enabled`
- `is_sponsored_result`
- `is_inactive_pro`
- `offers_figure_financing`
- `offers_greensky_financing`
- `show_highlight_video`

---

## Sample output

One object per professional. Here is a real example from a production run:

```json
{
  "listingId": "9def0a45170f2dc24f5b42d71b690dea21d8a1640d2d5394b78be2627c580f5e",
  "id": "210077",
  "title": "Catalyst Construction, Inc",
  "entityName": "Catalyst Construction, Inc",
  "location": "Austin, TX",
  "searchQuery": "general-contractors",
  "contentQuality": "serp_only",
  "detailFetched": false,
  "scrapedAt": "2026-10-22T15:07:42.381Z",
  "source": "houzz.com",
  "contentHash": null,
  "changeType": null
}
```

*Truncated — full records contain 71 fields. See Output fields for the complete schema.*

**[Try Houzz Pros Scraper [Just 💰$0.9] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/houzz-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00045 |
| Result | $0.0009 |

See the [actor on Apify](https://apify.com/blackfalcondata/houzz-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape houzz.com?**
Use this actor on Apify to extract structured data from houzz.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get houzz.com data as JSON, CSV, or Excel?**
The actor writes each professional to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape houzz.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check houzz.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each professional gets a content hash. On subsequent runs, only new or changed professionals are emitted — saving time, compute, and storage. Expired professionals can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Houzz Pros Scraper [Just 💰$0.9]](https://apify.com/blackfalcondata/houzz-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
