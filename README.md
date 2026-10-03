# How to Scrape e-marchespublics.com: French Public Procurement Tenders

> **Run it now, no code: [e-marchespublics-tenders-scraper on the Apify Store](https://apify.com/bibian/e-marchespublics-tenders-scraper)** &nbsp;|&nbsp; $2.00 per 1,000 tenders &nbsp;|&nbsp; no browser, no proxy

e-marchespublics.com is a major French public procurement portal: thousands of contracting authorities (town halls, regional authorities, public agencies) publish their tenders (appels d'offres) there. It has no public export for third parties. This guide shows how to get these tenders as clean structured data: contracting authority, location, procedure type, submission deadline and a link to the full notice, with a free Apify Actor, from the console or from code.

Searches this page answers: e-marchespublics scraper, french public procurement data, appel d'offres scraper, marchés publics API France.

## What you get from the e-marchespublics scraper

- Every current tender matching your keywords and category, newest first.
- The contracting authority (`organism`) and its location.
- The procedure type (works, services or supplies) and the submission deadline.
- A direct link to the full notice on e-marchespublics.com.

Export the dataset as JSON, CSV or Excel, or read it through the Apify API. A real sample (five results) is in [`sample-output.json`](sample-output.json).

## Output fields

| Field | What it is |
|---|---|
| `id`, `url` | Tender id and public link to the full notice |
| `title` | Tender title |
| `organism` | Contracting authority (town hall, regional authority, public agency) |
| `location` | Postal code and place of execution |
| `procedureType` | Category and procedure (for example `Travaux - Procédure Adaptée`) |
| `deadline` | Submission deadline, date and time |

## Use cases

- **Monitor a sector for new tenders.** Run with `what` set to your trade (for example "nettoyage" or "voirie") on a daily schedule and get only new tenders.
- **Track a region's public spending.** Filter by `category` (travaux, service, fourniture) to see what a given type of contracting authority is buying.
- **Feed a business development pipeline.** Export `organism`, `location` and `deadline` into a CRM or spreadsheet to prioritize which tenders to bid on.
- **Build a public procurement newsletter or dashboard.** Combine several keyword runs into one feed, deduplicated by `id`.

## Input

| Field | What it does |
|---|---|
| `what` | Words to search for, exactly as typed in the site's own search box, for example `nettoyage` or `voirie`. Empty means every current tender. |
| `category` | `tous` (all), `travaux` (works), `service` (services) or `fourniture` (supplies). |
| `maxItems` | The run stops once this many tenders are saved (default 50). |

Example input:

```json
{
  "what": "nettoyage",
  "category": "tous",
  "maxItems": 50
}
```

## Three ways to use it

### 1. Apify console, no code

1. Open [the Actor page](https://apify.com/bibian/e-marchespublics-tenders-scraper) and click **Try for free** (a free Apify account is enough).
2. Fill in keywords, a category and the maximum number of results, then click **Start**.
3. When the run finishes, open the **Output** tab, check the table, then **Export** as CSV, Excel or JSON.
4. For a daily tender alert, open **Schedules**, create a daily schedule for the Actor and, optionally, add an integration that sends new results to Slack, e-mail or Google Sheets.

### 2. Apify API with JavaScript

```bash
npm install apify-client
export APIFY_TOKEN="your token from Apify console, Settings, Integrations"
node examples/run.js
```

```js
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });

const run = await client.actor('bibian/e-marchespublics-tenders-scraper').call({
    "what": "nettoyage",
    "category": "tous",
    "maxItems": 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
for (const item of items) {
    console.log(`${item.title} | ${item.organism} | ${item.location} | deadline ${item.deadline}`);
}
```

The complete file is [`examples/run.js`](examples/run.js) (ES module, Node 18 or later).

### 3. Apify API with Python

```bash
pip install apify-client
export APIFY_TOKEN="your token from Apify console, Settings, Integrations"
python examples/run.py
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])

run = client.actor("bibian/e-marchespublics-tenders-scraper").call(run_input={
    "what": "nettoyage",
    "category": "tous",
    "maxItems": 50,
})

for item in client.dataset(run.default_dataset_id).iterate_items():
    print(f"{item['title']} | {item.get('organism')} | deadline {item.get('deadline')}")
```

The complete file is [`examples/run.py`](examples/run.py). Never put your token in the code: read it from the `APIFY_TOKEN` environment variable as above.

## Pricing

Pay per result: **$2.00 per 1,000 tenders** (that is $0.002 per tender), plus a tiny start fee of $0.00005 per run. You pay only for the results saved to the dataset. Apify's free plan includes monthly credits, enough to try it on a few hundred results. Prices are those shown on the [Store page](https://apify.com/bibian/e-marchespublics-tenders-scraper) when this guide was written; the Store page is the reference.

## Limits and honest notes

- Only what e-marchespublics.com publishes is collected; the detailed tender notice (full specifications, documents) is accessed through the `url` link, not re-hosted.
- The dataset lists public contracting authorities only. No personal data of individual civil servants or bidders is ever collected.
- This is an unofficial tool, not affiliated with e-marchespublics.com. Check the site terms that apply to your use.

## FAQ

**Does it need a browser or a proxy?**  
No. The Actor submits the same search the site's own form uses and reads the HTML listing directly, so runs are fast and cheap.

**How do I only get new tenders every day?**  
Schedule the Actor once a day with the same `what`/`category` input; tenders are listed newest first, and your own deduplication by `id` keeps only the new ones.

**Can I filter by procedure type?**  
Yes, set `category` to `travaux`, `service` or `fourniture`.

**Can I get the data in Excel or Google Sheets?**  
Yes. Export the dataset as CSV, Excel or JSON from the Apify console, or connect the Apify Google Sheets integration.

## Try it

**[Open e-marchespublics-tenders-scraper on the Apify Store](https://apify.com/bibian/e-marchespublics-tenders-scraper)**

The code in `examples/` is released under the MIT license (see [LICENSE](LICENSE)). The data belongs to its original publishers.
