# SEC EDGAR Filings Search MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-sec-edgar-filings-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-sec-edgar-filings-search) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-sec-edgar-filings-search)](https://github.com/mambalabsdev/mcp-sec-edgar-filings-search/blob/main/LICENSE)

An MCP server for the Mamba Labs SEC EDGAR Filings Search actor on Apify. SEC filings as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes tickers, CIK numbers, or company names and returns their SEC EDGAR filings, newest first, or takes phrases and returns the filing documents whose text matches. Filters by form type and filing date.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-sec-edgar-filings-search": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-sec-edgar-filings-search"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_sec_filings` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List Apple's 8-K filings filed since 2026-01-01 with their item numbers."
- "Find 10-K filings from the last 60 days that mention "going concern"."

## Tool

`search_sec_filings`. One tool. Example arguments:

```json
{
  "companies": [
    "AAPL"
  ],
  "forms": [
    "8-K"
  ],
  "filed_after": "2026-01-01",
  "max_filings_per_source": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `companies` | array | no | Tickers (AAPL), SEC CIK numbers (320193), or company names (Apple Inc.). Each company returns its filings, newest first. A name must match an EDGAR company name exactly, legal form words such as Inc ignored. In Clay, put the column inside a one item list. |
| `search_phrases` | array | no | Phrases to search in the text of EDGAR filings and exhibits filed since 2001. Put a phrase in double quotes for an exact match, for example "going concern". Each phrase returns one row per matching document, most relevant first. |
| `forms` | array | no | Form types to keep, for example 10-K, 10-Q, 8-K, S-1, DEF 14A, 4, 13F-HR. Empty keeps every form type. |
| `include_amendments` | boolean | no | true (default) also keeps amended filings of the chosen form types, such as 10-K/A. false drops them. |
| `filed_after` | string | no | Optional date, YYYY-MM-DD. Keeps filings filed on or after this date. |
| `filed_before` | string | no | Optional date, YYYY-MM-DD. Keeps filings filed on or before this date. |
| `max_filings_per_source` | number | no | Most filing rows for each company and each phrase, 1 to 10000. Default 100. |
| `dedupe` | boolean | no | true (default) returns each filing document once per run, even when two companies or phrases find it. Repeats are not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `filing-row`, one filing or matched filing document | $0.0015 | $0.001425 | $0.00135 | $0.001275 |
| `apify-actor-start`, per run start | $0.003 | $0.003 | $0.003 | $0.003 |

Repeats, companies not found, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/sec-edgar-filings-search](https://apify.com/mambalabs/sec-edgar-filings-search)

## License

MIT. See [LICENSE](LICENSE).
