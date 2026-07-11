[Jobstreet Scraper](https://apify.com/blackfalcondata/jobstreet-scraper?fpr=data)

## What does JobStreet Scraper do?

JobStreet Scraper extracts structured job data from [jobstreet.com](https://www.jobstreet.com) across Malaysia (`my.jobstreet.com`), Singapore (`sg.jobstreet.com`), Indonesia (`id.jobstreet.com`), and the Philippines (`ph.jobstreet.com`) — including salary data, contact details, company metadata, and full descriptions. It supports keyword search, location filters, and controllable result limits, so you can run the same query consistently over time.

**New to Apify?** [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) and use the included $5 monthly platform credit to test this actor.

## Key features

- **Four-market coverage** — one actor covers all JobStreet markets: MY, SG, ID, and PH.
- **Incremental mode** — recurring runs emit and charge only for listings that are new or whose tracked content changed. First run builds the baseline state; subsequent runs emit only new or changed records.
- **Detail enrichment** — full descriptions, company metadata, and contact information where the source provides them.
- **Compact mode** — AI-agent and MCP-friendly payloads with core fields only.

## What data can you extract from jobstreet.com?

Each result includes Core listing fields (`jobId`, `seekJobId`, `title`, `canonicalUrl`, `advertiserId`, `location`, `locationCountry`, and `locationState`, and more), detail fields when enrichment is enabled (`roleId`, `description`, `descriptionHtml`, `descriptionMarkdown`, and `descriptionLength`), contact and apply information (`phoneNumber`, `applyUrl`, and `extractedEmails`), and company metadata (`company`, `companyUrl`, `companyIndustry`, and `companySize`). In standard mode, all fields are always present — unavailable data points are returned as `null`, never omitted. In compact mode, only core fields are returned.

Enable `includeDetails` in the input to fetch the detail fields.

## Input

The main inputs are a search keyword, an optional location filter, and a result limit. Additional filters and options are available in the input schema.

Key parameters:

- **`query`** — Job search keywords. Use JSON array for multi-query.
- **`country`** — Which JobStreet market to search. (default: `"MY"`)
- **`location`** — City, state, or region. Use JSON array for multi-location.
- **`startUrls`** — Direct search or job detail URLs.
- **`maxResults`** — Maximum total job listings to return (0 = unlimited). (default: `25`)
- **`maxPages`** — Maximum SERP pages to scrape per search source. (default: `5`)
- **`sortMode`** — Sort results by relevance or date.
- **`dateRange`** — Filter jobs posted within a time range (e.g. '1', '3', '7', '14', '31').
- **`workType`** — Filter by work type code (e.g. '242' for Full Time on JobStreet).
- **`workArrangement`** — Filter by work arrangement code (e.g. '2' for Remote).
- **`classification`** — Filter by job classification/category code (e.g. '6281' for IT).
- **`subClassification`** — Filter by job sub-classification code (e.g. '6287' for Developers/Programmers under IT).
- ...and 11 more parameters

### Input examples

**Basic search** — Keyword-driven search with a result cap.

→ Full payload per result — all standard fields populated where the source provides them.

```
{
  "query": "software engineer",
  "maxResults": 50
}
```

**Incremental tracking** — Only emit jobs that changed since the previous run with this `stateKey`.

→ First run builds the baseline state. Subsequent runs emit only records that are new or whose tracked content changed. Set `emitUnchanged: true` to include unchanged records as well.

```
{
  "query": "software engineer",
  "maxResults": 200,
  "incrementalMode": true,
  "stateKey": "software-engineer-tracker"
}
```

**Compact output for AI agents** — Return only core fields for AI-agent and MCP workflows.

→ Small payload with the most important fields — ideal for piping into LLMs without token overhead.

```
{
  "query": "software engineer",
  "maxResults": 50,
  "compact": true
}
```

## Output

Each run produces a dataset of structured job records. Results can be downloaded as JSON, CSV, or Excel from the Dataset tab in Apify Console.

### Example job record

```
{
  "jobId": "485469726b257aa48bceebec68c9213bdf9a52e7ba3595af9158237164ad24c0",
  "seekJobId": "91247023",
  "title": "Associate Software Engineer (0-2 years)",
  "canonicalUrl": "https://my.jobstreet.com/job/91247023",
  "company": "Software International Corporation (M) Sdn Bhd",
  "companyUrl": "https://my.jobstreet.com/companies/software-international-corporation-168553406415897",
  "advertiserId": "60627682",
  "location": "Kuala Lumpur, Malaysia",
  "locationCountry": "MY",
  "locationState": "Kuala Lumpur",
  "locationSuburb": null,
  "locationPostcode": null,
  "salaryText": null,
  "salaryMin": null,
  "salaryMax": null,
  "salaryCurrency": "MYR",
  "salaryType": null,
  "employmentType": "Full time",
  "workArrangement": "hybrid",
  "category": "Information & Communication Technology",
  "subCategory": "Developers/Programmers",
  "roleId": "software-engineer",
  "teaser": "Gain proficiency in tools, programming, work closely with seniors. Prep docs, analyze problems, gather data, resolve issues in dev collaboration.",
  "bulletPoints": [
    "Hybrid Working Mode",
    "Generous Annual Leave",
    "Annual Performance Bonus, Increment, Individual Expenses Benefits"
  ],
  "description": "Responsibilities:\n\nParticipate in enterprise application development and maintenance for large corporations both within Malaysia and Worldwide.\n\n·       Develop knowledge and expertise in the applicat...",
  "descriptionHtml": "<p><strong>Responsibilities:</strong></p><p>Participate in enterprise application development and maintenance for large corporations both within Malaysia and Worldwide.</p><p>·&nbsp;&nbsp;&nbsp;&nbsp;...",
  "descriptionMarkdown": "**Responsibilities:**\n\nParticipate in enterprise application development and maintenance for large corporations both within Malaysia and Worldwide.\n\n· Develop knowledge and expertise in the applicatio...",
  "descriptionLength": 2044,
  "contentQuality": "full",
  "companyIndustry": "Computer Software & Networking",
  "companySize": "101-1,000 employees",
  "companyWebsite": "http://www.sicmsb.com",
  "phoneNumber": null,
  "screeningQuestions": [
    "How would you rate your English language skills?",
    "Which of the following types of qualifications do you have?",
    "How many years' experience do you have as a software engineer?",
    "Which of the following statements best describes your right to work in Malaysia?",
    "Which of the following front end development libraries and frameworks are you proficient in?",
    "What's your expected monthly basic salary?",
    "Which of the following programming languages are you experienced in?",
    "Which of the following languages are you fluent in?"
  ],
  "applyUrl": "https://www.seek.com.au/job/91247023?tracking=SHR-WEB-SharedJob-anz-1",
  "postedDate": "2026-03-31T01:42:31.188Z",
  "validThrough": "2026-04-30T13:59:59.999Z",
  "contentHash": "dd34696968c2eb237697635c81982afa76418d985a7862da891ca6466965bb96",
  "isSponsored": false,
  "sourceUrl": "https://my.jobstreet.com/job/91247023",
  "sourceCountry": "MY",
  "sourceDomain": "my.jobstreet.com",
  "searchQuery": "software engineer",
  "searchUrl": "https://www.seek.com.au/jobs?keywords=software+engineer&where=Malaysia",
  "scrapedAt": "2026-04-18T07:47:12.494Z",
  "fetchedAt": "2026-04-18T07:47:12.494Z",
  "detailFetched": true,
  "extractedEmails": [],
  "changeType": "NEW",
  "trackedHash": "dd34696968c2eb237697635c81982afa76418d985a7862da891ca6466965bb96",
  "firstSeenAt": "2026-04-18T07:47:12.494Z",
  "lastSeenAt": "2026-04-18T07:47:12.494Z",
  "previousSeenAt": null,
  "expiredAt": null,
  "stateKey": "live-verify-1776498430631",
  "isRepost": false,
  "repostOfId": null,
  "repostDetectedAt": null
}
```

### Incremental fields

When `incremental: true`, each record also carries:

- `changeType` — one of `NEW`, `UPDATED`, `UNCHANGED`, `REAPPEARED`, `EXPIRED`. Default output covers `NEW` / `UPDATED` / `REAPPEARED`; set `emitUnchanged: true` or `emitExpired: true` to opt into the others.
- `firstSeenAt`, `lastSeenAt` — ISO-8601 timestamps tracking the listing across runs.
- `isRepost`, `repostOfId`, `repostDetectedAt` — populated when a new listing matches the tracked content of a previously expired one. Set `skipReposts: true` to drop detected reposts from the output.

## How to scrape jobstreet.com

1. Go to [JobStreet Scraper](https://apify.com/blackfalcondata/jobstreet-scraper?fpr=1h3gvi) in Apify Console.
2. Enter a search keyword and optional location filter.
3. Set `maxResults` to control how many results you need.
4. Enable `includeDetails` if you need full descriptions, contact info, or company data.
5. Click **Start** and wait for the run to finish.
6. Export the dataset as JSON, CSV, or Excel.

## Use cases

- Extract job data from jobstreet.com for market research and competitive analysis.
- Track salary trends across regions and categories over time.
- Monitor new and changed listings on scheduled runs without processing the full dataset every time.
- Build outreach lists using contact details and apply URLs from listings.
- Research company hiring patterns, employer profiles, and industry distribution.
- Feed structured data into AI agents, MCP tools, and automated pipelines using compact mode.
- Export clean, structured data to dashboards, spreadsheets, or data warehouses.

## How much does it cost to scrape jobstreet.com?

JobStreet Scraper uses [pay-per-event](https://apify.com/pricing?fpr=1h3gvi) pricing. You pay a small fee when the run starts and then for each result that is actually produced.

- **Run start:** $0.01 per run
- **Per result:** $0.002 per job record

Example costs:

- 10 results: **$0.03**
- 100 results: **$0.21**
- 500 results: **$1.01**

### Example: recurring monitoring savings

These examples compare full re-scrapes with incremental runs at different churn rates. Churn is the share of listings that are new or whose tracked content changed since the previous run. Actual churn depends on your query breadth, source activity, and polling frequency — the scenarios below are examples, not predictions.

Example setup: 100 results per run, daily polling (30 runs/month). Event-pricing examples scale linearly with result count.

| Churn rate | Full re-scrape run cost | Incremental run cost | Savings vs full re-scrape | Monthly cost after baseline |
| --- | --- | --- | --- | --- |
| 5% — stable niche query | $0.21 | $0.02 | $0.19 (90%) | $0.60 |
| 15% — moderate broad query | $0.21 | $0.04 | $0.17 (81%) | $1.20 |
| 30% — high-volume aggregator | $0.21 | $0.07 | $0.14 (67%) | $2.10 |

Full re-scrape monthly cost at daily polling: $6.30. First month with incremental costs $0.79 / $1.37 / $2.24 for the 5% / 15% / 30% scenarios because the first run builds baseline state at full cost before incremental savings apply.

 

## FAQ

### How many results can I get from jobstreet.com?

The number of results depends on the search query and available listings on jobstreet.com. Use the `maxResults` parameter to control how many results are returned per run.

### Does JobStreet Scraper support recurring monitoring?

Yes. Enable incremental mode to only receive new or changed listings on subsequent runs. This is ideal for scheduled monitoring where you want to track changes over time without re-processing the full dataset.

### Can I integrate JobStreet Scraper with other apps?

Yes. JobStreet Scraper works with Apify's [integrations](https://apify.com/integrations?fpr=1h3gvi) to connect with tools like Zapier, Make, Google Sheets, Slack, and more. You can also use webhooks to trigger actions when a run completes.

### Can I use JobStreet Scraper with the Apify API?

Yes. You can start runs, manage inputs, and retrieve results programmatically through the Apify API. Client libraries are available for JavaScript, Python, and other languages.

### Can I use JobStreet Scraper through an MCP Server?

Yes. Apify provides an [MCP Server](https://apify.com/apify/actors-mcp-server?fpr=1h3gvi) that lets AI assistants and agents call this actor directly. Use compact mode and `descriptionMaxLength` to keep payloads manageable for LLM context windows.

### Is it legal to scrape jobstreet.com?

This actor extracts publicly available data from jobstreet.com. Web scraping of public information is generally considered legal, but you should always review the target site's terms of service and ensure your use case complies with applicable laws and regulations, including GDPR where relevant.

### Your feedback

If you have questions, need a feature, or found a bug, please [open an issue](https://apify.com/blackfalcondata/jobstreet-scraper/issues?fpr=1h3gvi) on the actor's page in Apify Console. Your feedback helps us improve.

## You might also like

- [Adzuna Job Scraper](https://apify.com/blackfalcondata/adzuna-scraper?fpr=1h3gvi) — Scrape adzuna.com - the global job board with 20+ country markets. Structured salary.
- [APEC.fr Scraper - French Executive Jobs](https://apify.com/blackfalcondata/apec-scraper?fpr=1h3gvi) — Scrape apec.fr - French executive job listings with salary ranges, company, location, skills,.
- [Arbeitsagentur Scraper - German Jobs](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Scrape arbeitsagentur.de - Germany’s official employment portal with 1M+ listings. Contact data,.
- [AutoScout24 Scraper](https://apify.com/blackfalcondata/autoscout24-scraper?fpr=1h3gvi) — Scrape autoscout24.com - Europe's largest used car marketplace with 770K+ listings. Structured.
- [Bayt.com Scraper - Jobs from the Middle East](https://apify.com/blackfalcondata/bayt-scraper?fpr=1h3gvi) — Scrape bayt.com - the leading Middle East job board. Salary data, experience requirements.
- [Bilbasen Scraper - Denmark’s Car Marketplace](https://apify.com/blackfalcondata/bilbasen-scraper?fpr=1h3gvi) — Scrape bilbasen.dk - Denmark’s largest car marketplace. Full vehicle specifications, seller.
- [Bumeran Scraper](https://apify.com/blackfalcondata/bumeran-scraper?fpr=1h3gvi) — Scrape bumeran.com.ar - the largest job board across 8 LATAM countries. Work modality, contract.
- [Cadremploi Job Scraper](https://apify.com/blackfalcondata/cadremploi-scraper?fpr=1h3gvi) — Scrape cadremploi.fr - French management and executive jobs. Salary ranges, apply links.

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open this actor and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).