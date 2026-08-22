# Pipeworx for Gemini CLI

Give Gemini one MCP that reaches **5,581+ live-data tools across 1,463+ sources** — SEC filings, USPTO patents, FRED, Census, FDA, EPA, USAspending, Polymarket, Zillow, weather, and 1,455+ more — without loading 5,581+ tool schemas into your context window.

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

The extension exposes **~31 meta-tools**, not all 5,581+ — `ask_pipeworx({question})` and friends route at runtime so you get the full catalog without paying the context tax for tools you'll never call this session.

## Free tier + signup

**Signing in is free and takes one GitHub click** — it moves you from 50 calls a day to 200, on a stable account that does not rotate with your IP. Point the server at `https://gateway.pipeworx.io/oauth/mcp` and complete the sign-in when prompted, or [sign up first](https://pipeworx.io/signup?via=gemini_plugin).

No account at all still works: `https://gateway.pipeworx.io/pipeworx-catalog/mcp`, anonymous, 50 calls a day per IP.

## Verify after install

```bash
gemini extensions list
```

You should see `pipeworx` enabled. Then ask in chat:

> What was the unemployment rate last month?

## What's loaded

- **`ask_pipeworx`** — natural-language router across all 1,463+ sources.
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

**No key? Sign in instead — it is free and gets you the same 200 calls/day.**
Use `https://gateway.pipeworx.io/oauth/mcp` as the `httpUrl` with no `headers`
block, and complete the GitHub sign-in when prompted. Keep the anonymous URL
(`.../pipeworx-catalog/mcp`, no headers) if you want no account at all — that is
50 calls/day. The two are alternatives: an `X-API-Key` sent to the OAuth URL is
rejected, because that endpoint authenticates with a bearer token.

## Links

- Gateway: https://gateway.pipeworx.io
- Status: https://pipeworx.io/status
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT

---

⭐ Star if you'd use this — helps other Gemini CLI users discover it.
