# Technical Write-up: Adding a `--json` Cookbook to btc-toolkit (PR #84)

## Overview

I contributed documentation and supporting tests to [btc-toolkit](https://github.com/devdavidejesus/btc-toolkit), a zero-dependency Python CLI for reading and verifying Bitcoin data from the terminal. The work closed [issue #63](https://github.com/devdavidejesus/btc-toolkit/issues/63) (“Cookbook: --json recipes with jq”) via [pull request #84](https://github.com/devdavidejesus/btc-toolkit/pull/84), which was reviewed, revised, and merged.

The contribution adds `docs/cookbook.md` (eight copy-paste `btc-toolkit … --json | jq` recipes with real mainnet sample outputs) and `tests/test_cookbook.py` (an offline unittest that validates the recipes’ structure, commands, flags, and JSON keys against the project’s schema). The page is linked from the README and `docs/commands.md`, and a CHANGELOG entry was added.

## Problem and Claim

btc-toolkit already supported stable `--json` output and documented the keys in `docs/json-schema.md`, but users still had to invent their own `jq` pipelines. Issue #63 asked for a cookbook of ready-to-use recipes.

My claim was that a short set of recipes, each backed by a real mainnet sample and constrained to only documented keys, would make the JSON interface immediately usable while remaining maintainable. Early drafting of the claim (Oct 21) said two recipes “both work” without showing output; a repro-check flagged the missing evidence, so I replaced the assertion with concrete numbers and sample runs before continuing.

## Work Performed

- **Oct 17**: Maintainer opened issue #63.
- **Oct 21**: Claimed the issue, drafted the approach, and strengthened the claim with real outputs after the repro-check.
- **Nov 9, ~5:06–6:01 PM CT**: Three commits:
  - `3a1bc7c` — cookbook with eight recipes and mainnet samples.
  - `77959ba` — `tests/test_cookbook.py` checking commands, flags, `--json` keys, and links.
  - `65c8970` — follow-ups from the issue thread (link from `docs/commands.md`, error-filtering recipe, stable old addresses/txids).
- **Nov 9, 6:45 PM CT**: Opened PR #84.
- **Nov 9, 6:57 PM CT**: Maintainer review requested two changes.
- **Nov 10, 1:38 PM CT**: Both changes landed in `186ee49`; I replied on the PR. (My automated PR monitor failed with “Not logged in” at the moment it mattered, so I followed the playbook manually.)
- **Nov 10, 7:50 PM CT**: Maintainer approved and merged. Issue #63 closed.

The tests parse the markdown, confirm each recipe has one command block and one sample, use a real subcommand and `--json`, and only reference keys present in `docs/json-schema.md` (plus the documented `"error"` key). They run fully offline.

## Changes Made in Response to Review

The maintainer’s review comment (quoted):

> Thanks, this is great work. Two small things: `test_recipes_are_well_formed` caps the cookbook at 8 recipes; please drop the upper bound, so adding a recipe later doesn't fail the test. Anyone can still send coins to an old address, so the address outputs can change; please say they "should rarely change" instead of "stay the same" (that holds for the transactions).

I addressed both in one commit (`186ee49`) and replied:

> Thanks, both done in 186ee49. I removed the 8-recipe cap from `test_recipes_are_well_formed` and changed the wording so transaction outputs stay the same and address outputs "should rarely change."

**Before (from `77959ba`):**
```python
def test_recipes_are_well_formed(self) -> None:
    recipes = _recipes()
    self.assertGreaterEqual(len(recipes), 5)
    self.assertLessEqual(len(recipes), 8)   # ← removed
    for recipe in recipes:
        ...

**After**
def test_recipes_are_well_formed(self) -> None:
    recipes = _recipes()
    self.assertGreaterEqual(len(recipes), 5)
    for recipe in recipes:
        ...

