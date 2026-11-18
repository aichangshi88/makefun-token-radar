# makefun-token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/aichangshi88/makefun-token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/aichangshi88/makefun-token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar (model radar-2): a dashboard that ranks pre-token protocols by implied value and shows
how to farm each one. It reads a sealed DefiLlama protocol snapshot, scores every rankable pre-token
protocol, and renders a sortable, filterable board with a detail view per protocol.

## How the model works

Implied value is calibrated from the snapshot itself. Each category's multiple is the median
market-cap-per-TVL of the protocols in that category whose token already trades (three or more of
them; the global median otherwise), clamped to [0.05, 1.5]. Then:

```
implied = round( tvl_usd x category_multiple
  x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
  x (0.7 + min(age_days, 1095) / 1095 x 0.3)
  x (1 + 0.02 x min(chains - 1, 10)) )
```

The board covers the snapshot's 40-protocol watchlist plus every other rankable pre-token protocol
below the floor (289 rows in all). Live-token protocols are set aside with their market caps shown:
they calibrate the model, they are never ranked. No protocol name is special-cased. Implied value
is a model, not a price, and a protocol that records no token may still have one its DefiLlama
record does not carry.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input snapshot.json
```

Node.js 22.18 or later, standard library only, no install. `snapshot.json` is the sealed input from
the competition kit. The program writes `out/board.json`, `out/data.js`, copies `dashboard/` and
`screenshots/` to `out/`, and writes `out/dossier.json` with the vendored make.fun library.

---

<sub>Entered on [make.fun](https://make.fun/?ref=v72g1v8t&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
