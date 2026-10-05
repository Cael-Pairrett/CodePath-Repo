# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Cael-Pairrett

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5986314653

Plan for #64, based on my repro above (`--runxfail` gives `E assert 1.0 < 0.9`, log `avg_score=1.0 ... query_len=4`).

**Cause:** the fixture, not the scorer. `RelevanceScorer.score()` returns matched query tokens / total query tokens. All 4 tokens of `Python Django web framework` appear in `Django is a Python web framework for rapid development`, so 1.0 is the right score for full coverage.

**Change:** only `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py`. I'll keep the query and change the chunk to `Django is a Python library for rapid development` (matches `django` and `python`, so 2/4 = 0.5). @srithimahi's plan above uses a 3-of-4 chunk (0.75). I'm going with 2 of 4 so the score sits in the middle of the `0.3 < score < 0.9` band instead of near the top. Same cause, separate branch on my fork. I'll also remove the test's `xfail(strict=True)` marker, per CONTRIBUTING. Not touching the scorer, its tokenizer, the band, or any other test.

**Test:** re-run the same `--runxfail` command and expect `PASSED` with `avg_score=0.5`. Then the whole file without `--runxfail` should go from `18 passed, 1 xfailed` to `19 passed`, followed by ruff, black, and `make test-unit`.

**Unknown:** `_tokenize` only splits on whitespace, so the new chunk can't have punctuation next to a query word. The `avg_score=0.5` in the after-run is what confirms that.

Next step: build this on `fix/64-partial-overlap-fixture` in my fork and post the before/after output.

---

## Your branch

**Branch**

fix/64-partial-overlap-fixture

https://github.com/Cael-Pairrett/pathreview-ai301-fa26-s1/tree/fix/64-partial-overlap-fixture (commit `a544d35`)

**Evidence**

Before is `main` at `f89c06f`, which is the same code my Unit 2 repro ran on. After is `fix/64-partial-overlap-fixture` at `a544d35`. Same Unit 2 commands both times. Every pytest call also had `-p no:cacheprovider` added so no `.pytest_cache` landed in the clone. It doesn't affect results. The after's single-test run adds `-rP` so the passing test's captured `avg_score` log is printed.

**Before (unfixed `main`)**

```
$ .venv/bin/pytest tests/unit/test_relevance_scorer.py -q
..x................                                                      [100%]
18 passed, 1 xfailed in 0.76s
exit=0

$ .venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail
============================= test session starts ==============================
platform linux -- Python 3.13.5, pytest-9.1.1, pluggy-1.6.0 -- /workspace/pathreview-ai301-fa26-s1/.venv/bin/python3
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /workspace/pathreview-ai301-fa26-s1
configfile: pyproject.toml
plugins: hypothesis-6.168.3, cov-7.1.0, asyncio-1.4.0, benchmark-5.3.0, anyio-4.15.1, platformdirs-4.12.3, pytest_httpserver-1.1.5
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap FAILED [100%]

=================================== FAILURES ===================================
_____________ TestRelevanceScorer.test_query_with_partial_overlap ______________

self = <tests.unit.test_relevance_scorer.TestRelevanceScorer object at 0x7fe09cf5e520>
scorer = <rag.evaluator.relevance_scorer.RelevanceScorer object at 0x7fe09d3c38c0>

    @pytest.mark.xfail(
        strict=True,
        reason="issue #64: relevance scorer 'partial overlap' fixture actually has full overlap",
    )
    def test_query_with_partial_overlap(self, scorer):
        """Test query with partial overlap returns score between 0 and 1."""
        query = "Python Django web framework"
        chunks = [
            {"text": "Django is a Python web framework for rapid development"},
        ]
    
        score = scorer.score(query, chunks)
    
        assert isinstance(score, float)
        assert 0.0 <= score <= 1.0
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
        ^^^^^^^^^^^^^^^^^^^^^^^^
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
----------------------------- Captured stdout call -----------------------------
2026-10-04 19:50:15 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
=========================== short test summary info ============================
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap
============================== 1 failed in 0.19s ===============================
exit=1
```

**After (`fix/64-partial-overlap-fixture`)**

```
$ .venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail -rP
============================= test session starts ==============================
platform linux -- Python 3.13.5, pytest-9.1.1, pluggy-1.6.0 -- /workspace/pathreview-ai301-fa26-s1/.venv/bin/python3
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /workspace/pathreview-ai301-fa26-s1
configfile: pyproject.toml
plugins: hypothesis-6.168.3, cov-7.1.0, asyncio-1.4.0, benchmark-5.3.0, anyio-4.15.1, platformdirs-4.12.3, pytest_httpserver-1.1.5
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap PASSED [100%]

==================================== PASSES ====================================
_____________ TestRelevanceScorer.test_query_with_partial_overlap ______________
----------------------------- Captured stdout call -----------------------------
2026-10-04 19:51:16 [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
============================== 1 passed in 0.19s ===============================
exit=0

$ .venv/bin/pytest tests/unit/test_relevance_scorer.py -q
...................                                                      [100%]
19 passed in 0.17s
exit=0

$ .venv/bin/ruff check tests/unit/test_relevance_scorer.py
All checks passed!
exit=0

$ .venv/bin/black --check tests/unit/test_relevance_scorer.py
All done! ✨ 🍰 ✨
1 file would be left unchanged.
exit=0

$ .venv/bin/pytest tests/unit -q -m unit   # what `make test-unit` runs (make is not installed on this box)

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
376 passed, 52 xfailed, 2 warnings in 9.09s
exit=0
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Every full run, in order:

1. Full run 1: **19/20**. `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. This was my first, longer rubric (six required checks I wrote myself). The miss was pkg-14 on `executable-by-stranger`. A partial `--only pkg-14,pkg-10,pkg-17,pkg-18` re-grade after loosening that check went 4/4.
2. Full run 2: **20/20**. `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. Same rubric with the loosened executability check.
3. Full run 3: **18/20**. `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 1/2  unbuildable 3/3  wrong-cause 4/4`. I rebuilt the rubric to start from my group's "Our rubric": just `diagnosis`, `scope`, and `test`, in the group's words. It missed pkg-14 (failed `scope` and `test`) and pkg-20 (graded accept). Partial re-grades after adding the `comms` check: `--only pkg-20,pkg-04,pkg-03,pkg-05,pkg-08,pkg-09` went 5/6, because the canary pkg-09 flipped to reject on `comms`. After narrowing the disclosure wording, `--only pkg-09,pkg-20,pkg-03,pkg-04` went 4/4.
4. Full run 4: **19/20**. `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`, `agreement: 19/20 scored items  (bar: 18/20: PASS)`. This is the committed `eval-run.txt`: the group's three checks plus the one `comms` addition.

**Package analysis**

**pkg-20** (`ghostty-org/ghostty#11261`, stale `prev` pointer after page growth in `Terminal.print`). Gold: **reject** (thread-convention). Run 3, with only the group checks, said **accept**. Run 4, with `comms`, said **reject**.

The plan itself is strong. The diagnosis matches the repro ("the stale `prev` pointer survives the capacity change", isolated by the no-hyperlink control). The scope names `Terminal.print`'s grapheme paths and the page, and says what's out ("recomputing `prev` unconditionally (the approach the thread already rejected as too expensive)"). The test plan adds "both of the issue's test cases as regression tests". So `diagnosis`, `scope`, and `test` all passed in both runs, and with only those three checks the package graded accept. What's wrong is the comment. The repo facts say "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance ... AI-assisted issues and comments must be reviewed and edited by a human", and the comment ("My plan follows the direction proposed here: a page generation counter...") never mentions AI at all. None of my group's checks reads the plan comment, so the group rubric literally couldn't see this. The `comms` addition reads the comment against the policy line, fails it for the missing disclosure, and the verdict becomes reject, matching gold.

**Check rationale**

Quoted exactly from `tools/plan-check/rubric.md`:

> | test | The plan's Test plan | Passes if the plan adds an automated test. | required |

Why it reads this way: this is my group's "Our rubric" test check, word for word: "passes if the plan adds an automated test." Earlier I had replaced it with a longer check of my own ("re-runs the repro and states the observable after-result"). When I rebuilt the rubric from the group's worksheet I put the group's sentence back unchanged rather than paraphrasing it. The Evidence column only says where to look (the plan's Test plan). The group's version is about the outcome, not the formatting: it doesn't care how the test plan is written, only whether a test that runs in the suite comes out of it. In run 3 it did real work. It failed pkg-04 ("follow only the new documentation ... and confirm", no automated test), pkg-07, pkg-16 ("the parser test suite passes", nothing new), and the vague unbuildable plans. In the evidence guide and procedure I spelled out that a *fixed* existing test that now runs in the suite counts as "adds", because that's what my own #64 plan does.

**Trade-offs**

The group's `test` check gives up plans whose right test is a manual repro. pkg-14 (zellij, gold accept) is the case it misses. Its test plan is "the repro loop: 5 consecutive SSH reattach cycles with no rgb strings in any pane", which is a solid but manual check of terminal output over SSH that's hard to automate. Under "adds an automated test" it fails `test` in both runs 3 and 4. The group's `scope` check ("names each file it will change") also fails it, because the plan names a code path in `zellij-server` and leaves the exact function to be pinned with debug logs. I accept that miss. Loosening the group's wording to let manual tests through would also let pkg-04, pkg-07, and pkg-10 off one of their failing checks, and the brief asked me to start from the group's rubric, not rewrite it. The `comms` addition gives up something too: its first wording flipped the canary pkg-09 to reject, because fd's policy asks for AI disclosure only in the PR, not on issue comments. I narrowed it so a PR-only disclosure rule doesn't apply to a plan comment, re-ran pkg-09, pkg-20, pkg-03, and pkg-04 with `--only` (4/4), and the confirming full run 4 changed nothing else. Every other row matches run 3 except pkg-20.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
