# Technical Write-up: Adding a `--json` Cookbook to btc-toolkit (PR #84)

## Overview

I contributed documentation and offline tests to [btc-toolkit](https://github.com/devdavidejesus/btc-toolkit) that closed [issue #63](https://github.com/devdavidejesus/btc-toolkit/issues/63) via [PR #84](https://github.com/devdavidejesus/btc-toolkit/pull/84). The PR added `docs/cookbook.md` (eight `--json` recipes with real mainnet samples) and `tests/test_cookbook.py`, was reviewed, revised in response to feedback, and merged.

## What happened

### Work Performed

- **Oct 17**: Maintainer opened issue #63 (“Cookbook: --json recipes with jq”).
- **Oct 21**: Claimed the issue and drafted the approach. A repro-check flagged that the claim said two recipes “both work” with no output, so I replaced the assertion with concrete numbers and sample runs.
- **Nov 9, ~5:06–6:01 PM CT**: Three commits — cookbook (`3a1bc7c`), tests that check its commands, flags, and `--json` keys (`77959ba`), and follow-ups from the issue thread (`65c8970`).
- **Nov 9, 6:45 PM CT**: Opened PR #84.
- **Nov 9, 6:57 PM CT**: Maintainer asked for two fixes.
- **Nov 10, 1:38 PM CT**: Both fixes landed in one commit (`186ee49`); I replied on the PR. My automated PR monitor failed with “Not logged in,” so I followed the playbook by hand.
- **Nov 10, 7:50 PM CT**: Maintainer approved with “Thanks for the quick follow-up, both changes look good. Merging.” The PR merged and the issue closed.

### Changes Made in Response to Review

The maintainer’s review comment:

> Thanks, this is great work. Two small things: `test_recipes_are_well_formed` caps the cookbook at 8 recipes; please drop the upper bound, so adding a recipe later doesn't fail the test. Anyone can still send coins to an old address, so the address outputs can change; please say they "should rarely change" instead of "stay the same" (that holds for the transactions).

I addressed both in `186ee49` and replied:

> Thanks, both done in 186ee49. I removed the 8-recipe cap from `test_recipes_are_well_formed` and changed the wording so transaction outputs stay the same and address outputs "should rarely change."

**Recipe-cap change in `tests/test_cookbook.py`:**

Before (`77959ba`):

```python
def test_recipes_are_well_formed(self) -> None:
    recipes = _recipes()
    self.assertGreaterEqual(len(recipes), 5)
    self.assertLessEqual(len(recipes), 8)
    for recipe in recipes:
        ...
After (186ee49):

Python
def test_recipes_are_well_formed(self) -> None:
    recipes = _recipes()
    self.assertGreaterEqual(len(recipes), 5)
    for recipe in recipes:
        ...
Wording change in docs/cookbook.md:

Before: "...an address with no activity since 2011 or 2012, so the outputs stay the same."

After: "...an address with no activity since 2011 or 2012. Transaction outputs stay the same. Address outputs should rarely change, but anyone can still send coins to an old address."

The maintainer then approved and merged the PR.

Limitations and what's next
The tests in tests/test_cookbook.py check that each recipe is formatted correctly, uses a real command and flags, and only references keys documented in docs/json-schema.md. They parse the markdown offline and never actually run the recipes against live mainnet data. Sample outputs were captured once on 2026-11-09 and treated as stable. My automated PR monitor also failed with “Not logged in” right when the review arrived, so I had to reply by hand.

Next I would add a small optional integration check that can re-run the recipes against the public API when a network is available, and I would fix the monitor login so the next review is caught automatically.

Links
Issue: [github.com/devdavidejesus/btc-toolkit/issues/63](https://github.com/devdavidejesus/btc-toolkit/issues/63)
PR: [github.com/devdavidejesus/btc-toolkit/pull/84](https://github.com/devdavidejesus/btc-toolkit/pull/84)
Cookbook: [github.com/devdavidejesus/btc-toolkit/blob/main/docs/cookbook.md](https://github.com/devdavidejesus/btc-toolkit/blob/main/docs/cookbook.md)
Tests: [github.com/devdavidejesus/btc-toolkit/blob/main/tests/test_cookbook.py](https://github.com/devdavidejesus/btc-toolkit/blob/main/tests/test_cookbook.py)
Fix commit: [github.com/devdavidejesus/btc-toolkit/commit/186ee4939ceaae118bb461ddcc89e7eccec2888b](https://github.com/devdavidejesus/btc-toolkit/commit/186ee4939ceaae118bb461ddcc89e7eccec2888b)
