# Usage plan: reaching 1,000 logged Nansen API calls

The buildathon requires **1,000 logged Nansen API calls** by September 27, 2026.
A connector only burns calls when someone uses it, so the plan has two engines:

1. **Interactive use** — real questions through Muse (unpredictable volume, but it's the point of the thing).
2. **Scheduled "smart money brief"** — an automated run through the connector on a fixed cadence. Each brief run costs **~12 tool calls**: netflow 24h, pnl leaderboard, top dex trades, token balances, 2–3 token flow summaries, perp trades, screener pass, and a couple of follow-up wallet lookups.

## The math

| Cadence | Runs/day | Calls/day | Days to 1,000 (automation alone) |
|---|---|---|---|
| Every 6h | 4 | ~48 | ~21 |
| Every 3h | 8 | ~96 | ~10.4 |
| Every 2h | 12 | ~144 | ~7 |

Starting September 18, there are ~9 days to the deadline. Interactive use (demos,
testing, the X demo video run-through, daily check-ins) realistically adds 20–40
calls/day on top.

## Recommendation

**Every 3 hours** as the default. Rationale:

- Automation alone lands at ~10.4 days — just past the deadline — but interactive use closes the gap comfortably (~9 days with even modest daily usage).
- Every 2h guarantees the number on automation alone (~7 days), but burns 50% more quota for insurance you likely don't need.
- Every 6h doesn't get there without heroic interactive volume. Don't.

**Owner's call.** The tradeoff is API quota burn vs. timeline certainty. If the Nansen
key's quota is tight, run 3h and lean on interactive use. If quota is plentiful and
you want zero doubt, run 2h. Either way, keep an eye on the Nansen dashboard's logged
call count — that's the number that matters, not this spreadsheet.

## Notes

- The brief content should actually be useful (morning-brief format from `examples.md`), not empty pings — junk calls risk looking like quota gaming if anyone looks closely.
- Each brief run should vary slightly (rotate chains, timeframes, a featured token) so the call pattern looks like real usage. Because it is.
- Stop or slow the schedule the moment the 1,000-call mark is confirmed — no reason to burn quota past the requirement.
