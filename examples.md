> **Note:** These are scripted demo conversations. Muse's replies below show
> **illustrative** tool outputs — plausible in shape, invented in numbers — to demonstrate
> what the connector interaction feels like. Real outputs depend on live Nansen data.

---

## Demo A — Smart money morning brief

**user:** Give me the smart money morning brief.

**Muse:** On it — pulling overnight flows, top performers, and notable trades. *(calls `smart_traders_and_funds_netflow` with `orderBy: net_flow_24h_usd`, then `smart_traders_and_funds_pnl_leaderboard` with `timeframe: 7`, then `smart_traders_and_funds_dex_trades` with `orderBy: trade_value_usd`, min $250k)*

Here's your brief for the last 24 hours:

**Biggest net inflows (smart money, all chains):**
1. **HYPE** — +$4.2M net (38 smart wallets buying)
2. **NEAR** — +$1.8M net (21 wallets)
3. **VIRTUAL** — +$940K net (14 wallets)

**Biggest net outflows:**
1. **WIF** — −$2.6M net (distribution, 31 sellers)
2. **JUP** — −$1.1M net

**Top PnL wallets this week:** `0x7f…a3c1` (+$1.9M total, 68% win rate, 41 trades) leads the board. Two fund-labeled wallets round out the top three.

**Notable single trades:** a 90D Smart Trader opened a $1.4M HYPE spot position on HyperEVM ~6h ago; a fund wallet added $880K NEAR on… Ethereum mainnet, interesting — cross-chain accumulation.

Want me to dig into any of these — say, what the top wallet's been buying?

---

## Demo B — Which smart money wallets are accumulating NEAR?

**user:** Which smart money wallets are accumulating NEAR?

**Muse:** Checking aggregate flows first, then the individual buyers. *(calls `smart_traders_and_funds_netflow` filtered to NEAR, then `smart_traders_and_funds_dex_trades` with `tokenBoughtSymbol: "NEAR"` sorted by trade value, then `token_who_bought_sold` with `buy_or_sell: "BUY"` and smart-trader labels)*

**Aggregate:** Smart money is net **+$1.8M on NEAR over 24h** (+$3.4M over 7d) — consistent accumulation, not a one-off spike.

**Biggest individual buyers (24h):**
- `0x7f…a3c1` (90D Smart Trader) — bought $880K
- `0x2b…e9d4` (Fund) — bought $510K
- `0x91…44af` (30D Smart Trader) — bought $290K

**Context:** 21 distinct smart wallets bought vs. 9 selling. The fund buyer (`0x2b…e9d4`) has been adding daily for five sessions — looks like a programmatic build, not a punt. No whale-labeled sellers of size in the same window.

Want the full trade history on any of these wallets, or their PnL?

---

## Demo C — Find the best wallet this month, then copy its homework

**user:** Find the best performing smart money wallet this month and show me what it's buying.

**Muse:** Pulling the 30-day leaderboard, then that wallet's recent trades. *(calls `smart_traders_and_funds_pnl_leaderboard` with `timeframe: 30, orderBy: total_pnl_usd`, then `smart_traders_and_funds_dex_trades` with `traderAddress` set to the winner)*

**Winner:** `0x7f…a3c1` — **+$6.8M total PnL over 30 days** (62% realized), 71% win rate across 112 trades, avg ROI 18%. Labeled 90D Smart Trader.

**What it's buying right now (last 7 days):**
- HYPE — $1.4M in (3 buys, no sells)
- NEAR — $880K in (2 buys)
- AAVE — $420K in (1 buy)
- It fully exited WIF (−$960K out, 4 sells) two days before the distribution I flagged in your morning brief.

**Read:** This wallet rotates early into L1-adjacent beta and cuts losers fast. Its current book is concentrated: HYPE > NEAR > AAVE, all net long, no perp hedges visible.

Shall I set a watch on this wallet and include its new buys in your morning brief?
