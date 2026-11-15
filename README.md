# Token Radar

A token launch radar: the pre-token DeFi protocols ranked by the implied value of
the token they may one day issue, with a concrete farm plan for each one.

## The model (radar-2)

`implied_value = TVL x comp_ratio(category) x growth_factor x age_factor`

- **comp_ratio(category)** — the median market-cap / TVL of token-bearing
  protocols in that category (at least 3 live comps, else the overall median
  0.2227x), clamped to [0.02, 2.0]. Computed from the snapshot at run time;
  no protocol name is special-cased.
- **growth_factor** — the protocol's 7-day TVL change, capped to [-50%, +100%]
  and half-weighted.
- **age_factor** — days since first listing, 0.7 to 1.0 over 3 years; a
  protocol that has run longer with no token is a more credible launch.

The board covers the snapshot's whole 40-protocol watchlist (TVL >= $50M, no
token on record) plus 158 pre-token protocols below the floor that the house
rule would keep — 198 rows, every one pre-token. 24 live-token protocols are
set aside on their own tab, never ranked.

## The dashboard

`dashboard/` is a static page (no network): a sortable, filterable board, a
detail view per protocol with the full implied-value breakdown, and the farm
plan. The page recomputes each row with the same `impliedValue` function the
board uses, so the screenshots and the code agree.

## Run it

```sh
mkdir -p out
MAKEFUN_OUTPUT_DIR=out node main.ts --input input/snapshot.json
```

Reads `/input/snapshot.json` (or `--input`), writes `board.json`, `data.js`,
`dashboard/`, `screenshots/` and `dossier.json` into the output dir.
Deterministic: two runs write byte-identical files.

## Entry

Built for the make.fun bounty `token-radar-01` (Build the best token launch
radar). Snapshot: DefiLlama protocols, fetched 2026-11-14.

## License

MIT — see `LICENSE`.
