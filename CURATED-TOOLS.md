# Nansen Smart Money — Curated Tool Reference

12 tools, hand-picked from Nansen's 47-tool MCP surface (`https://mcp.nansen.ai/ra/mcp`).
Every tool is read-only. Auth: your own Nansen API key, passed in the `NANSEN-API-KEY` request header.

The smart-money cohort across these tools means **Smart Traders + Funds** — Nansen's
labels for consistently profitable on-chain traders and crypto funds. Whales, large
holders, and influencers are explicitly excluded.

---

## Smart Money Flows

### `smart_traders_and_funds_netflow`
**What it answers:** What is smart money buying or selling right now, in aggregate?

Returns per-token net USD flow (inflows minus outflows) from smart traders and funds,
aggregated — not per wallet. Think of it as the cohort-level tape.

**Ask Muse:**
- "What is smart money buying the most today?"
- "Is smart money accumulating or dumping SOL this week?"

**Key parameters:**
- `chains` — default `['all']`; e.g. `["base"]`, `["solana"]`
- `orderBy` — `net_flow_1h_usd` / `net_flow_24h_usd` (default) / `net_flow_7d_usd` / `net_flow_30d_usd` / `trader_count` / `token_age_days` / `market_cap_usd`
- `tokenAddress` — restrict to a single token
- `includeSmartMoneyLabels` — e.g. `["Any Smart Money"]`, `["Fund"]`, `["30D Smart Trader"]`
- `includeStablecoin` / `includeNativeTokens` — default `true`
- `marketCapUsd` — `{from, to}` range filter

### `smart_traders_and_funds_dex_trades`
**What it answers:** Exactly which smart wallet bought or sold what, trade by trade.

Individual per-wallet, per-transaction DEX trades. Use this when you want names and
receipts; use `netflow` when you want the aggregate picture.

**Ask Muse:**
- "Show me the biggest smart money buys in the last 24 hours."
- "What has this wallet been buying this week?" (pass its address)

**Key parameters:**
- `tokenBoughtSymbol` / `tokenSoldSymbol` — e.g. `"NEAR"`, `"HYPE"`
- `traderAddress` — filter to one wallet (string or array)
- `tradeValueUsd` — `{from, to}` minimum trade size filter
- `orderBy` — `block_timestamp` (default) / `trade_value_usd` / `token_bought_amount` / `token_sold_amount`
- `chains` — default all

### `smart_traders_and_funds_token_balances`
**What it answers:** What does smart money actually hold right now?

Aggregated token balances across the cohort plus 24h change per token. Flows tell you
what's moving; balances tell you what's conviction.

**Ask Muse:**
- "What are smart money's largest holdings?"
- "Which tokens did smart money add to the most in the last 24 hours?"

**Key parameters:**
- `orderBy` — `value_usd` (default) / `balance_24h_percent_change` / `holders_count`
- `minHolders` — only include tokens held by at least N smart wallets
- `chains`, `includeSmartMoneyLabels` as above

---

## Wallet Intel

### `smart_traders_and_funds_pnl_leaderboard`
**What it answers:** Who are the best-performing smart money wallets?

Ranks smart trader and fund addresses by realized, unrealized, and total USD PnL,
with ROI, win rate, and trade counts. This is the starting point for copy-trading research.

**Ask Muse:**
- "Who made the most money in crypto this month?"
- "Which smart traders have the highest win rate over the last 90 days?"

**Key parameters:**
- `timeframe` — `1` / `7` (default) / `30` / `90` / `180` days
- `orderBy` — `total_pnl_usd` (default) / `realized_pnl_usd` / `unrealized_pnl_usd` / `avg_trade_roi` / `win_rate` / `n_trades` / `n_tokens`
- `chains`, `includeSmartMoneyLabels` as above

### `wallet_pnl_summary`
**What it answers:** How is this specific wallet performing, overall?

Full PnL summary for one address over a date range. Pass `chain: "hyperliquid"` for
perp traders; `"evm"` / `"all"` reports spot and Hyperliquid perp results in separate sections.

**Ask Muse:**
- "How did 0x… perform in August?"
- "Is this wallet actually profitable, or just loud on the timeline?"

**Key parameters:**
- `walletAddress` — required
- `dateRange` — required, `{from: "YYYY-MM-DD", to: "YYYY-MM-DD"}`
- `chain` — default `"evm"`; `"hyperliquid"` for perp-only

### `wallet_pnl_for_token`
**What it answers:** How much did this wallet make (or is it making) on this one token?

Per-wallet, per-token PnL. `showRealized: true` = closed trades; `false` = current
position analysis (unrealized PnL + active holdings). For Hyperliquid perps, `tokenAddress`
is the perp symbol (e.g. `"BTC"`, `"HYPE"`), not a contract address.

**Ask Muse:**
- "How much did this wallet make on HYPE?"
- "Is this wallet still holding its position, or has it exited?"

**Key parameters:**
- `walletAddress`, `tokenAddress`, `dateRange` — all required
- `showRealized` — required, `true` (realized) / `false` (current position)
- `chain` — default `"evm"`; `"hyperliquid"` for perps

### `address_portfolio`
**What it answers:** What does this wallet hold right now, in full?

Comprehensive portfolio: token balances, DeFi positions, and Hyperliquid positions.
Accepts a wallet address or a named entity (e.g. `"Paradigm Fund"`) — exactly one.

**Ask Muse:**
- "What's in this wallet's portfolio?"
- "Does this wallet have any open Hyperliquid positions?"

**Key parameters:**
- `walletAddress` **or** `entity_id` — exactly one required
- `mode` — `"fast-mode-default"` (balances + Hyperliquid) / `"all"` (+ DeFi) / `"wallet_balances"` / `"defi"` / `"hyperliquid"`
- `chain` — default `"all"`

---

## Token Discovery

### `token_discovery_screener`
**What it answers:** Find me tokens smart money is piling into, with my filters.

The broadest discovery tool: screen tokens across up to 5 chains by flows, buyers,
volume, market cap, token age, sector — restricted to the smart-money trader cohort
with `traderType: "sm"`. Supports `near` as a chain.

**Ask Muse:**
- "Which tokens under $50M market cap is smart money buying on Base?"
- "Find tokens less than 30 days old with heavy smart money inflows today."

**Key parameters:**
- `chains` — max 5; default `["ethereum", "solana", "bnb", "base"]`; supports `near`
- `traderType` — `"sm"` for smart money (required for smart-money label filters)
- `timeframe` — `5m` / `1h` / `6h` / `24h` (default) / `7d` / `30d`
- `netflow` / `buyVolume` / `nofBuyers` — `{from, to}` filters
- `marketCapUsd`, `tokenAgeDays`, `sectors` — e.g. `["Memecoins"]`, `["AI Agents"]`
- `orderBy` — default `netflow`; 25 results per page

### `token_who_bought_sold`
**What it answers:** Who exactly bought (or sold) this token?

Per-wallet buyers or sellers of a single token over a date range, filterable by
label — so you can ask specifically about smart traders vs. whales vs. funds.

**Ask Muse:**
- "Which smart traders bought NEAR this week?"
- "Who's been dumping this token?"

**Key parameters:**
- `chain`, `tokenAddress`, `buy_or_sell` (`"BUY"` / `"SELL"`) — all required
- `time_range` — `{from, to}`
- `include_labels` — e.g. `["30D Smart Trader", "90D Smart Trader", "Fund"]`
- `min_trade_volume_usd` — default `10`

### `token_recent_flows_summary`
**What it answers:** Give me the quick flow tape for this token.

Inflows, outflows, buyer/seller counts over a short lookback — the fastest way to
check whether flows are accelerating or fading. Supports `near` as a chain and a
`perps` mode for Hyperliquid symbols.

**Ask Muse:**
- "Are flows into HYPE accelerating or fading?"
- "Summarize NEAR flows over the last 7 days."

**Key parameters:**
- `tokenAddress` — required
- `chain` — default `"ethereum"`; supports `near`
- `lookbackPeriod` — `5m` / `1h` / `6h` / `12h` / `1d` (default) / `7d`
- `mode` — `"onchain_tokens"` (default) / `"perps"`

### `token_quant_scores`
**What it answers:** What do Nansen's quant models say about this token?

Nansen Score Indicators for a token — the model-driven complement to raw flow data.
On-chain tokens only (no Hyperliquid perps).

**Ask Muse:**
- "What's the quant outlook on this token?"
- "Do Nansen's scores back up the smart money flows I'm seeing?"

**Key parameters:**
- `tokenAddress` — required
- `chain` — default `"ethereum"`

---

## Derivatives

### `smart_traders_and_funds_perp_trades`
**What it answers:** What are smart money perps traders doing on Hyperliquid right now?

Recent perp trades from smart money addresses: side (Long/Short), action
(Open/Add/Reduce/Close), size, and price. Hyperliquid-only, recent trades only —
no date filtering available.

**Ask Muse:**
- "Are smart money traders opening longs or shorts right now?"
- "Show me the biggest smart money perp closes today."

**Key parameters:**
- `side` — `"Long"` / `"Short"`
- `action` — `"Open"` / `"Add"` / `"Reduce"` / `"Close"`
- `valueUsd` — `{from, to}` minimum trade size
- `traderAddress` — filter to one wallet
- `orderBy` — `timestamp` / `amount` (default) / `price_usd`

---

## Notes

- Parameter details above come from the live MCP tool schemas (September 2026). If a
  call fails, re-check the schema — Nansen iterates on these endpoints.
- All 12 tools are read-only. The connector cannot move funds, sign, or trade.
- "Smart money" here = Smart Traders + Funds labels. Whales, large holders, and
  influencers are excluded by these endpoints by design.
