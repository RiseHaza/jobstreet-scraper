[Jobstreet Scraper](https://apify.com/unfenced-group/jobstreet-scraper?fpr=data)

# JobStreet Scraper

![JobStreet Scraper](https://images.apifyusercontent.com/e5VvzJ-EV7Tn1Ydejud7GZSliB0cRE_ma0yRO00vRzQ/w:1800/cb:1/aHR0cHM6Ly9pLmltZ3VyLmNvbS9FTHR5RmRPLnBuZw.webp)

Extract job listings from **JobStreet** — Southeast Asia's leading job platform — across **Singapore, Malaysia, Philippines, and Indonesia**. Get structured data including job titles, companies, locations, salary ranges, work arrangements, and full descriptions. No API key required.

---

## 🌏 Markets covered

| Country | Code | Active listings |
| --- | --- | --- |
| Singapore | `SG` | ~65,000+ |
| Malaysia | `MY` | ~35,000+ |
| Philippines | `PH` | ~45,000+ |
| Indonesia | `ID` | ~10,000+ |

---

## ✨ Features

- **Keyword + location search** — find jobs by role, skill, or discipline combined with any city or region
- **Work type filter** — Full time, Part time, Contract/Temp, Casual/Vacation
- **Date filter** — restrict results to listings posted within 1, 7, 30, or any number of days
- **Full job descriptions** — optional fetch of complete HTML descriptions (plain text + Markdown also provided)
- **Structured salary data** — parsed min/max amount, currency, and period from salary labels
- **Job categories** — filter by SEEK classification ID for precise industry targeting
- **Deduplication** — 90-day repost cache per country; set `skipReposts: true` to receive only new listings on repeat runs
- **Direct URL mode** — provide specific JobStreet job page URLs to fetch those listings directly
- **Self-healing** — automatic failure detection and health signal written after every run
- **Max results cap** — precise spend control; only successfully retrieved listings are charged

---

## 📥 Input parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `country` | string | Market: `SG`, `MY`, `PH`, `ID` (default: `SG`) |
| `searchQuery` | string | Keywords, job title, or skill |
| `location` | string | City or region (e.g. `Singapore`, `Kuala Lumpur`) |
| `workType` | string | `Full time`, `Part time`, `Contract/Temp`, `Casual/Vacation` |
| `classificationId` | integer | SEEK category ID for industry filtering |
| `daysOld` | integer | Only return listings posted within N days |
| `maxResults` | integer | Maximum listings to retrieve (default: 200) |
| `fetchDetails` | boolean | Fetch full HTML description (default: false) |
| `skipReposts` | boolean | Skip listings seen in previous runs (default: false) |
| `startUrls` | array | Direct job page URLs to fetch instead of searching |

### Example input

```
{
    "country": "SG",
    "searchQuery": "software engineer",
    "location": "Singapore",
    "workType": "Full time",
    "daysOld": 7,
    "maxResults": 500,
    "fetchDetails": true,
    "skipReposts": false
}
```

---

## 📤 Output schema

Each result contains the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Unique listing ID |
| `url` | string | Direct link to the job listing |
| `title` | string | Job title |
| `company` | string | Employer name |
| `companyUrl` | string | Company profile page |
| `location` | string | Location label |
| `country` | string | ISO country code (SG / MY / PH / ID) |
| `workTypes` | array | Employment type(s) |
| `workArrangement` | string | On-site / Hybrid / Remote |
| `salaryLabel` | string | Salary as displayed on the platform |
| `salaryMin` | number | Parsed minimum salary |
| `salaryMax` | number | Parsed maximum salary |
| `salaryCurrency` | string | Currency code (SGD, MYR, PHP, IDR) |
| `salaryPeriod` | string | HOUR / MONTH / YEAR |
| `classifications` | array | Job categories (e.g. `ICT › Developers/Programmers`) |
| `teaser` | string | Short listing summary |
| `bulletPoints` | array | Highlighted selling points (sponsored listings only) |
| `isFeatured` | boolean | Sponsored/featured listing flag |
| `isExpired` | boolean | Whether the listing has expired |
| `publishDateISO` | string | Listing date (ISO 8601) |
| `descriptionHtml` | string | Full HTML description (`fetchDetails: true`) |
| `descriptionText` | string | Plain text description (`fetchDetails: true`) |
| `descriptionMarkdown` | string | Markdown description (`fetchDetails: true`) |
| `isRepost` | boolean | Seen in a previous run |
| `source` | string | Source domain |
| `scrapedAt` | string | Scrape timestamp (ISO 8601) |
| `contentHash` | string | 16-char change-detection fingerprint |

### Example output record

```
{
    "id": "91694036",
    "url": "https://sg.jobstreet.com/job/91694036",
    "title": "Software Engineer",
    "company": "Acme Technologies Pte Ltd",
    "location": "Singapore",
    "country": "SG",
    "workTypes": ["Full time"],
    "workArrangement": "Hybrid",
    "salaryLabel": "$5,500 – $8,000 per month",
    "salaryMin": 5500,
    "salaryMax": 8000,
    "salaryCurrency": "SGD",
    "salaryPeriod": "MONTH",
    "classifications": ["Information & Communication Technology › Developers/Programmers"],
    "teaser": "Join our growing engineering team...",
    "isFeatured": false,
    "isExpired": false,
    "publishDateISO": "2026-04-22T08:14:00Z",
    "descriptionHtml": null,
    "descriptionText": null,
    "descriptionMarkdown": null,
    "isRepost": false,
    "source": "sg.jobstreet.com",
    "scrapedAt": "2026-04-23T10:00:00.000Z",
    "contentHash": "a3f1c8d92e047b51"
}
```

---

## 💰 Pricing

**$0.99 per 1,000 results** — you only pay for successfully retrieved listings.
Failed retries and filtered reposts are never charged.

| Results | Cost |
| --- | --- |
| 100 | ~$0.10 |
| 1,000 | ~$0.99 |
| 10,000 | ~$9.90 |
| 100,000 | ~$99.00 |

> Flat-rate alternatives typically charge $29–$49/month regardless of usage.

Use the **Max results** cap in the input to control your spend exactly.

---

## ⚠️ Known limitations

- **`applyUrl` not available** — JobStreet does not expose direct application URLs; the listing URL is the entry point.
- **Salary is often null** — many listings in PH and ID do not display salary information; `salaryMin` / `salaryMax` will be null for those.
- **`bulletPoints` sparse** — only sponsored/premium listings include highlight bullet points.
- **`workArrangement`** — not available when using `startUrls` mode (list-page only field).
- **HK / TH not supported** — those markets use a different platform architecture and are not covered by this actor.
- **Max 10,000 results per search** — the platform caps paginated results at 100 pages × 100 items. Use multiple targeted keyword+location runs for larger extractions.

---

## Technical details

- **Source:** jobstreet.com — Southeast Asia's leading job platform
- **Memory:** 256 MB
- **Repost storage:** KeyValueStore `jobstreet-{country}-job-dedup`, 90-day TTL per market
- **Retry:** Automatic retry on network errors, exponential backoff, 3 attempts per request

---

## Additional services

Need a custom actor, additional filters, scheduled runs, or integration support?
Send an email to [info@unfencedgroup.nl](mailto:info@unfencedgroup.nl) — we build on request.

---

*Built by [unfenced-group](https://apify.com/unfenced-group) · Issues? Open a ticket or send a message.*