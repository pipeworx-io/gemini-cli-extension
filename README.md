# Pipeworx Gemini CLI Extension

Connect Gemini CLI to live data from **2,325 tools across 555 packs** — SEC filings, USPTO patents, FRED economic data, FDA drug data, Census, EPA, ATTOM real estate, weather, and 547+ more.

Backed by the [Pipeworx](https://pipeworx.io) MCP gateway at `gateway.pipeworx.io`.

## Install

```bash
gemini extensions install https://github.com/pipeworx-io/gemini-cli-extension
```

## How it works

Gemini CLI loads **17 meta-tools** from the Pipeworx gateway — not all 2,325 underlying tools. That's deliberate: dumping every tool definition into the context window burns tokens you'll never use.

Instead, Gemini reaches for `ask_pipeworx` or `discover_tools` and the gateway routes the request to the right pack at session time. You get the full catalog without paying for it up front.

The loaded meta-tools:

- **`ask_pipeworx`** — natural-language router. *"What's Apple's latest 10-K?"* hits SEC EDGAR. *"Side effects of Ozempic?"* hits FDA.
- **`discover_tools`** — top-20 most relevant tools for a task, with full schemas.
- **`entity_profile`**, **`recent_changes`**, **`compare_entities`**, **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL. Returns a verdict + citation.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`**, **`search_packs`**, **`get_pack_tools`**, **`get_connection_config`**, **`get_platform_status`**, **`search_mcp_directory`** — browse the catalog.

The bundled skill teaches Gemini when to reach for each.

## Verify after install

In Gemini CLI, list extensions:

```bash
gemini extensions list
```

You should see `pipeworx` enabled. Then try a real query:

> What was the unemployment rate last month?

Gemini should call `ask_pipeworx` (which routes to `fred_get_series`) and return a real number.

## Need direct pack access?

If you want a specific pack's tools loaded directly (e.g., to call `attom_property_search` without going through `ask_pipeworx`), edit `gemini-extension.json` (or your global Gemini settings) to point at a scoped MCP entry:

```json
{
  "mcpServers": {
    "pipeworx-attom": {
      "httpUrl": "https://gateway.pipeworx.io/attom/mcp"
    }
  }
}
```

Or a vertical bundle (e.g., `?vertical=housing` for the housing data stack).

## Higher rate limits

The extension runs on the anonymous tier (50 calls/day per IP). For higher limits (500/day BYO, 2,000/day OAuth, or unlimited paid), [sign up at pipeworx.io](https://pipeworx.io) and add an `X-API-Key` header to the `pipeworx` server block in `gemini-extension.json`:

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
- Stack guide: https://pipeworx.io/stack
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT
