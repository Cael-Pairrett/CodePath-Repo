# Repro report: btc-toolkit #63 (Cookbook: --json recipes with jq)

## Environment
- btc-toolkit 1.8.1 (`python3 -m btc_toolkit --version`), run from a clone of my fork at upstream `main` 55b9e19 (2026-10-19)
- Python 3.13.5, jq-1.7, Debian Linux
- Network: mainnet (default public API), run 2026-10-21

## Steps
1. `gh repo fork devdavidejesus/btc-toolkit` and clone; `git log -1` shows 55b9e19.
2. `ls docs/`, `ls docs/cookbook.md`, and `grep -n cookbook README.md`.
3. Run the issue's example recipes on mainnet from the clone root:
   - `echo a1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d | python3 -m btc_toolkit tx - --json | jq -r '.fee_rate_sat_vb // empty'`
   - `python3 -m btc_toolkit utxo 17SkEw2md5avVNyYgj6RiXuQKNwkXaxFyQ --json | jq '[.utxos[] | select(.value_sats > 10000) | .value_sats] | add'`
   - `python3 -m btc_toolkit utxo bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h --json | jq '[.utxos[] | select(.value_sats > 10000) | .value_sats] | add'`

## Evidence
Raw terminal output, run from the clone root (the README line is cut at 160 characters with `cut`; the full line is the roadmap paragraph that links to #63):

```
$ ls docs/
json-schema.md
python-api.md
releases.md
$ ls docs/cookbook.md
ls: cannot access 'docs/cookbook.md': No such file or directory
$ grep -n cookbook README.md | cut -c1-160
452:The original roadmap shipped in v1.0.0; every release since is in the [CHANGELOG](https://github.com/devdavidejesus/btc-toolkit/blob/main/CHANGELOG.md). Wha
$ echo a1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d | python3 -m btc_toolkit tx - --json | jq -r '.fee_rate_sat_vb // empty'
4191.36
$ python3 -m btc_toolkit utxo 17SkEw2md5avVNyYgj6RiXuQKNwkXaxFyQ --json | jq '[.utxos[] | select(.value_sats > 10000) | .value_sats] | add'
1935301
$ python3 -m btc_toolkit utxo bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h --json
{
  "error": "API error 400: Bad Request",
  "address": "bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h"
}
$ python3 -m btc_toolkit utxo bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h --json | jq '[.utxos[] | select(.value_sats > 10000) | .value_sats] | add'
jq: error (at <stdin>:4): Cannot iterate over null (null)
```

## Outcome
Reproduced: the gap the issue describes is real on today's `main`. There is no `docs/cookbook.md` and the README has no link to one. The example recipes from the issue work against mainnet. One thing I noticed: when the API returns an error object (the `bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h` address got an API 400), the `utxo` recipe as written fails in jq with "Cannot iterate over null", so the cookbook should use `.utxos[]?` there, the same way the opreturn example already uses `.outputs[]?`.

