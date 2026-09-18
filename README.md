# Nansen Smart Money for Muse

**Nansen's smart-money API, as a Muse custom connector. Ask Muse about smart money in plain English.**

Instead of a fourth dashboard, this puts on-chain intelligence where you already talk
to AI. "Which smart money wallets are accumulating NEAR?" gets you an answer, not a
tab to open.

## Why this exists

Meta just opened the connector program for Muse — the ability to plug any API into
your personal agent. Every crypto buildathon entry is a dashboard. Distribution beats
another dashboard: this meets users inside the agent they already use, in the language
they already speak.

Built for the **Nansen Meridian Buildathon** (entry #4).

## What it does

12 curated tools from Nansen's MCP surface (`https://mcp.nansen.ai/ra/mcp`), in 4 categories:

- **Smart Money Flows** — what the cohort is buying/selling (netflow), trade-by-trade receipts (dex trades), what it holds (balances)
- **Wallet Intel** — PnL leaderboard, per-wallet PnL summaries, per-token PnL, full portfolio snapshots
- **Token Discovery** — smart-money screener, who bought/sold a token, flow summaries, quant scores
- **Derivatives** — smart money perp trades on Hyperliquid (long/short, open/close)

Full reference: [CURATED-TOOLS.md](CURATED-TOOLS.md). Scripted demos: [examples.md](examples.md).

Everything is read-only. The connector can look, never touch — no trading, no signing, no fund movement.

## Setup

1. **In the Muse app, ask Muse to create a custom connector.** ("Build me a custom connector for this MCP server…")
2. **Point it at this repo's `mcp-config.json`** — or directly at the server URL:
   `https://mcp.nansen.ai/ra/mcp` (transport: `streamable-http`).
3. **Connect your Nansen API key when prompted.** It's sent as the `NANSEN-API-KEY` request header and stored in Muse's Secure Credentials Store. Bring your own key with sufficient API quota.
4. **Try the example prompts:**
   - "Give me the smart money morning brief."
   - "Which smart money wallets are accumulating NEAR?"
   - "Find the best performing smart money wallet this month and show me what it's buying."

See [examples.md](examples.md) for scripted versions of these conversations.

## Files

| File | What |
|---|---|
| `mcp-config.json` | Client-agnostic MCP config (server URL, transport, auth header) |
| `CURATED-TOOLS.md` | The 12 curated tools: what each answers, example questions, key params |
| `examples.md` | 3 scripted demo conversations (illustrative outputs) |
| `USAGE-PLAN.md` | How the 1,000-API-call buildathon requirement gets met |

## Buildathon note

Nansen Meridian Buildathon entry — valid entries need 1,000 logged Nansen API calls
plus a 30–60s demo posted on X tagging `@nansen_ai` with the repo link. The call-volume
plan (scheduled smart-money briefs through the connector + interactive use) is in
[USAGE-PLAN.md](USAGE-PLAN.md). Requires a Nansen API key with enough quota to cover
the cadence you choose.

## Caveats

- "Smart money" = Nansen's Smart Trader + Fund labels. Whales, large holders, and influencers are excluded by these endpoints.
- Tool schemas were captured September 2026; Nansen iterates — re-check a schema if a call fails.
- Past wallet performance doesn't predict anything. This is intel, not advice.
