# Plan: #64 — relevance scorer "partial overlap" fixture has full overlap

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64
My repro: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5858903544

## Diagnosis

The bug is in the test fixture, not in `RelevanceScorer`. My Unit 2 repro
(PathReview `f89c06f`, Python 3.13.5, pytest 9.1.1, Debian 13) ran:

```
.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail
```

and got:

```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
2026-09-27 14:06:15 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
```

Without `--runxfail` the file reports `18 passed, 1 xfailed`, because the
test is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`.

Why the score is 1.0: `RelevanceScorer.score()` in
`rag/evaluator/relevance_scorer.py` lowercases and whitespace-splits both
strings (`_tokenize`), then returns `len(query_tokens & chunk_tokens) /
len(query_tokens)` averaged over chunks. The query `Python Django web
framework` gives 4 tokens (`python`, `django`, `web`, `framework`), and
the chunk `Django is a Python web framework for rapid development`
contains all 4, so overlap is 4/4 = 1.0. That is the correct answer for
full coverage (`avg_score=1.0` in the log), so the scorer is doing what
it says. The fixture is labeled "partial overlap" but isn't one, which
is exactly what the issue says.

## Scope

In scope:
- The chunk text in `test_query_with_partial_overlap` so it matches only
  some of the query tokens.
- Removing that test's `@pytest.mark.xfail(strict=True, ...)` marker,
  which CONTRIBUTING.md ("Working on a seeded bug: remove its xfail
  marker") says is part of fixing a seeded bug.

Out of scope:
- `rag/evaluator/relevance_scorer.py` (the scorer is correct; no
  tokenizer, stemming, or punctuation changes).
- The `0.3 < score < 0.9` band and every other test in the file.
- `pyproject.toml` suppressions (there are none for #64; I checked).
- Lint/format cleanup anywhere else.

## Files to touch

- `tests/unit/test_relevance_scorer.py`, `TestRelevanceScorer.test_query_with_partial_overlap` only.

## Approach

1. Create `fix/64-partial-overlap-fixture` from `main` on my fork.
2. In `test_query_with_partial_overlap`, keep the query `Python Django
   web framework` and change the chunk to `Django is a Python library
   for rapid development`. Its tokens hit `django` and `python` but not
   `web` or `framework`, so the expected score is 2/4 = 0.5, right in
   the middle of the `0.3 < score < 0.9` band. I picked 2 of 4 instead
   of 3 of 4 (0.75) so the case sits clear of both edges of the band.
   I kept every word space-separated with no punctuation next to a
   query word, because `_tokenize` only splits on whitespace (`python,`
   would not match `python`).
3. Delete the `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`
   decorator from that test. Leaving it would turn the passing test into
   `XPASS(strict)` and fail CI.
4. Commit as `test(rag): make partial-overlap fixture actually partial`
   with `Fixes #64`, with plan.md and comment.md left out (checked with
   `git status` before committing).

## Test plan

Re-run my Unit 2 repro commands against the branch:

1. `.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail`
   - Before: `FAILED`, `E       assert 1.0 < 0.9`, log `avg_score=1.0`.
   - Expected after: `PASSED`, log `avg_score=0.5 chunks_count=1 query_len=4`, exit 0.
2. `.venv/bin/pytest tests/unit/test_relevance_scorer.py -q` (no `--runxfail`)
   - Before: `18 passed, 1 xfailed`.
   - Expected after: `19 passed`, with no xfailed and no XPASS(strict).
3. Repo checks for the touched file: `ruff check` and `black --check`
   on `tests/unit/test_relevance_scorer.py`, then `make test-unit`, all
   passing.

## Risks and unknowns

- `_tokenize` splits on whitespace only, so punctuation next to a query
  word in the new chunk would quietly change the score. I'm keeping the
  new chunk free of punctuation, and the after-run's `avg_score=0.5`
  confirms it.
- Other students have plans on this same issue (for example a 3-of-4
  chunk). That doesn't block this one, but the exact fixture text will
  differ between branches.
- I am not checking whether other tests in the repo depend on the
  scorer's whitespace tokenization. That's outside this issue.

## Deviations

The code change is exactly what's planned above. On
`fix/64-partial-overlap-fixture` (commit `a544d35`), the only file
touched is `tests/unit/test_relevance_scorer.py`. The chunk is now
`Django is a Python library for rapid development` and the
`xfail(strict=True)` marker is gone. plan.md and comment.md stayed out
of the commit. The after-results matched what I expected: `PASSED` with
`avg_score=0.5`, then `19 passed` for the file.

One small change to how I ran the test plan. `make` isn't installed on
the machine I built on, so for step 3 I ran the command the Makefile's
`test-unit` target uses, `.venv/bin/pytest tests/unit -v -m unit`
(with `-q` instead of `-v`), and got `376 passed, 52 xfailed`. I ran
ruff and black directly on the touched file. That doesn't change the
fix or what the plan comment says. CI on the PR will run the real
`make` targets.
