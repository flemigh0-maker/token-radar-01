# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/flemigh0-maker/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/flemigh0-maker/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar ranks 198 pre-token protocols by implied launch value, with a farm plan for each, on a static dashboard that works offline.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22, standard library only. `main.ts` reads the sealed DefiLlama snapshot, writes
`board.json` (the ranked board) and `data.js` (the dashboard's data), copies `dashboard/` and
`screenshots/`, and writes `dossier.json` with the vendored `vendor/makefun-board.ts`.
Open `out/dashboard/index.html` from disk: the page needs no network.

## The model (radar-2)

A launch candidate records no token and is not a wrapped, staked or bridged product of a
token that already trades, not an exchange, and not a versioned pool — the watchlist's own
rule, applied to the whole snapshot. The board covers all 40 watchlist protocols and 158
more candidates the snapshot holds. Live-token protocols are set aside with the market's own
values, never mixed into the ranking.

Implied value is calibrated on the snapshot's own live market:

```
implied = round(tvl_usd × category multiple × growth × maturity × chains)
```

- **Category multiple**: the median market-cap-to-TVL ratio of live-token protocols in the
  same category (only when at least three report both figures), else the global median
  (0.241×), clamped to 0.05–1.5×.
- **Growth**: `1 + clamp(change_7d, −60, 120) / 100 × 0.5`.
- **Maturity**: `0.6 + min(age_days, 1095) / 1095 × 0.4`; age is days since first listing,
  unknown age counts as 365 days.
- **Chains**: `1 + 0.015 × min(chains − 1, 10)`.

No protocol name is special-cased: one function scores every row, and the dashboard shows
the calibration table it was scored with. The page recomputes each row and checks it against
`board.json`.

## Honesty notes

- "Records no token" is not "has no token": DefiLlama's record is not proof, and a protocol
  on this board may still have a token its record does not carry.
- Implied value is an estimate of what a launch would be worth if the protocol's record
  stayed as it is. It is not a price, a prediction that a token will launch, or advice.
- Every farm step states what it costs and how long it takes. Never pay to claim an
  airdrop; claim only from a link the protocol itself publishes.

---

<sub>Entered on [make.fun](https://make.fun/?ref=w7h9vbej&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
