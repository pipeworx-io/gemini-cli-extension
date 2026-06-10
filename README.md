# Pipeworx for Gemini CLI

Give Gemini one MCP that reaches **3,300+ live-data tools across 750+ sources** — SEC filings, USPTO patents, FRED, Census, FDA, EPA, USAspending, Polymarket, Zillow, weather, and 740+ more — without loading 3,300+ tool schemas into your context window.

## Install

```bash
gemini extensions install https://github.com/pipeworx-io/gemini-cli-extension
```

## Try it

After install, ask Gemini things like:

| Ask | What it triggers |
|---|---|
| *"What just happened to Apple?"* | `sec_8k_recent` → SEC 8-K events classified by severity |
| *"Spread between Polymarket and Kalshi on the next Fed decision?"* | `polymarket_kalshi_spread` → live cross-venue mispricing |
| *"Overdue Phase 3 readouts at Moderna?"* | `pharma_pipeline_catalysts` → biotech catalyst calendar |
| *"DoD cybersecurity contracts this week?"* | `usa_award_search` → sub-second USAspending mirror |
| *"Median home value and renter share in Lubbock, TX?"* | `housing_market_snapshot` + `housing_metro_demand` |
| *"Unemployment rate last month?"* | `fred_get_series` → official FRED data |

Gemini picks the right tool via `ask_pipeworx` — no pack-name memorization required.

## How it loads light

The extension exposes **~26 meta-tools**, not all 3,300+ — `ask_pipeworx({question})` and friends route at runtime so you get the full catalog without paying the context tax for tools you'll never call this session.

## Free tier + signup

100 calls/day anonymous, IP-bound. [Sign up free in 10s via GitHub](https://pipeworx.io/signup?via=gemini_plugin) for 200/day + a stable account.

## Verify after install

```bash
gemini extensions list
```

You should see `pipeworx` enabled. Then ask in chat:

> What was the unemployment rate last month?

## What's loaded

- **`ask_pipeworx`** — natural-language router across all 750+ sources.
- **`discover_tools`** — top-20 relevant tools for a task, with full schemas.
- **`entity_profile`** / **`compare_entities`** / **`recent_changes`** / **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`** / **`search_packs`** / **`get_pack_tools`** / **`get_connection_config`** / **`get_platform_status`** / **`search_mcp_directory`** — browse the catalog.

The bundled skill teaches Gemini when to reach for each.

## Direct pack access

For a specific pack's tools loaded directly (e.g., `attom_property_search` without going through `ask_pipeworx`), edit `gemini-extension.json` (or your global Gemini settings) to point at a scoped MCP entry:

```json
{
  "mcpServers": {
    "pipeworx-attom": {
      "httpUrl": "https://gateway.pipeworx.io/attom/mcp"
    }
  }
}
```

Or a vertical bundle (e.g., `?vertical=housing` for the housing-data stack).

## Bring your own key

For BYO-tier limits (200/day) or your own per-tool API keys, add an `X-API-Key` header to the `pipeworx` server block:

```json
{
  "mcpServers": {
    "pipeworx": {
      "httpUrl": "https://gateway.pipeworx.io/pipeworx-catalog/mcp",
      "headers": { "X-API-Key": "$PIPEWORX_API_KEY" }
    }
  }
}
```

Set `PIPEWORX_API_KEY` in your shell environment.

## Links

- Gateway: https://gateway.pipeworx.io
- Status: https://pipeworx.io/status
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT

---

⭐ Star if you'd use this — helps other Gemini CLI users discover it.
