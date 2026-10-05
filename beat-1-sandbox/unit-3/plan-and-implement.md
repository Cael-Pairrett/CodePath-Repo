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

1. Run 1 (full, saved): **19/20**. `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. The one miss was pkg-14 (gold accept, I graded reject on `executable-by-stranger`).
2. Partial re-grade, `--only pkg-14,pkg-10,pkg-17,pkg-18` after loosening `executable-by-stranger`: 4/4 agree. pkg-14 flipped to accept, and the three unbuildable canaries stayed reject. Partial runs don't count toward the bar.
3. Run 2 (full, saved, this is the committed `eval-run.txt`): **20/20**. `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`, `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-14** (`zellij-org/zellij#5174`, OSC color-query responses leaking into the pane on SSH reattach). Gold: **accept**. My rubric said **reject** in run 1 and **accept** in run 2.

In run 1 it failed `executable-by-stranger` and nothing else. The plan's Files line says "the client attach/reattach path in `zellij-server` (session connection handling) ... exact functions to be pinned in the PR after tracing the query issuance with debug logs". My first pass condition required the file *and the function*, and this plan honestly doesn't name a function yet, so the grader failed it. Reading it again, the change itself is already decided: "consuming or draining pending OSC query responses in the client attach path ... before pane input is wired", bounded "to OSC response patterns rather than a time window". The diagnosis explains every control (fresh attach clean, 0.44.1 clean, the cache-clear one-off). The test plan re-runs the repro loop ("5 consecutive SSH reattach cycles with no rgb strings in any pane"), and the Windows variant is deferred with a reason. A stranger knows what to change and where. All that's left is the line number. The unbuildable packages are different because they don't know the change: pkg-17 "investigate the input stack ... experiment with the mouse protocols", pkg-10 "optimize whatever shows up hot". So my check was punishing honesty about an exact line, not a plan nobody could build. I loosened it to accept a named code path plus a concrete change when the plan says how it will pin the function, and run 2 graded pkg-14 accept.

**Check rationale**

Quoted exactly from `tools/plan-check/rubric.md`:

> | executable-by-stranger | The plan's Files, Approach, and Change lines | Pass if a stranger could start the work without asking the author: it names where the change goes (a file, or a specific code path such as "the reattach handshake in the client attach path") and says concretely what the change is (what will be different in the code afterward). An exact function still to be pinned is fine when the plan names the code path and how it will pin it, and a named site the plan honestly flags as possibly one layer off still passes. Fail if the work is described as activities instead of changes ("investigate", "explore", "look at", "optimize whatever shows up hot", "track down what changed", "sort out the handling"), or names no code location at all. | required |

Why it reads this way: the first version said a plan passes only if "it names the file(s) and the function, handler, or code site to change". That read executability as "has a function name", which is about format, and it failed pkg-14, a good plan that names the code path and the exact change but is honest that the function will be pinned with debug logs. What actually separates buildable from unbuildable plans in the set is whether the *change* is decided. So the pass side now asks for a location (a file or a specific code path) plus "what will be different in the code afterward", and it explicitly lets a "still to be pinned" function or a "possibly one layer off" site through. The fail side lists the activity verbs ("investigate", "explore", "optimize whatever shows up hot", "track down what changed", "sort out the handling"), because those are what the unbuildable packages actually say. I quoted them so two graders draw the line in the same place.

**Trade-offs**

Loosening the check gives up some strictness. A plan can now pass while naming only a code path ("the client attach path") and promising to pin the function later. If that promise is empty, the gap only shows up at build time, not at plan time. I accept that this check will miss a plan that names a plausible path and a plausible change but has the path wrong. Catching that is `diagnosis-grounded`'s job, not this check's. To make sure the loosening didn't let vague plans through, I re-ran pkg-14 with all three unbuildable packages as canaries (`--only pkg-14,pkg-10,pkg-17,pkg-18`). pkg-10, pkg-17, and pkg-18 stayed reject, and pkg-14 flipped to accept. The confirming full run then held 20/20, with unbuildable still 3/3 and no other package moving. I know nothing changed elsewhere because the table matches run 1 row for row except pkg-14.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
