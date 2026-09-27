# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Cael-Pairrett

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5858883519

Looking at #64 — the relevance scorer “partial overlap” fixture (query “Python Django web framework” vs a chunk that appears to contain all four query terms). Next I will run `pytest tests/unit/test_relevance_scorer.py -q` on Linux and report whether the score lands at 1.0 and breaks the `0.3 < score < 0.9` band the way the issue describes.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5858903544

## Reproduction — #64 partial-overlap fixture

**Environment:** PathReview `f89c06f` (main), Python 3.13.5, pytest 9.1.1, Debian 13 / Linux x86_64.

**Steps:**

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"
.venv/bin/pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v --runxfail
```

(`--runxfail` needed because the case is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`; default `pytest tests/unit/test_relevance_scorer.py -q` reports `18 passed, 1 xfailed`.)

**Observed:**

```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
2026-09-27 14:06:15 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
```

Query tokens `python django web framework` all appear in the chunk text, so keyword overlap is 4/4 and the scorer returns `1.0` — full coverage, not partial.

**Result:** Reproduced. Matches the issue (`assert 1.0 < 0.9`). Next step: adjust the fixture so overlap is genuinely partial, then drop the xfail marker.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Final (only) full harness run: agreement **19/20** scored items (bar: 18/20: PASS). Matches `eval-run.txt` line: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Sole disagreement: pkg-07 (gold accept, graded reject, note `failed: env-faithful`). Model: sonnet (pinned). Written 2026-09-27T19:08:30Z.

**Package analysis**

`pkg-07` (processing/p5.js#7168, FES reserved-word message under non-English browser language). Gold label: **accept**. My rubric verdict: **reject** (disagreement). The harness note says `failed: env-faithful`. The package records p5.js **1.11.7** on Chrome/macOS with Japanese-first language order, and it does call out that the issue was filed against 1.9.4/1.10.0 and still reproduces on 1.11.7. Repo facts list latest release **v2.3.2**. My `env-faithful` check requires a match to the issue's target **or** current/latest **or** every material deviation called out in plain language. The grader treated the 1.11.7 vs 2.3.2 (and vs filed 1.9.4/1.10.0) gap as a silent mismatch even though a delta sentence is present — so the check fired stricter than the gold accept intends. That is the only scored miss in the run.

**Check rationale**

Quoted from uploaded `tools/repro-check/rubric.md` (`env-faithful` row):

> The environment record compared to the issue's stated target (version, platform, release channel), plus any explicit delta callout in the report or claim. | The tested version/platform matches what the issue targets, or matches current/latest when the thread is about current behavior, OR every material deviation is called out in plain language (for example "filed on 2.x / main; I tested 1.5.3"). Silent testing on a mismatched older version, wrong channel, or wrong platform that cannot speak to the reported bug fails even if the error text looks similar. | required

Why it reads this way: I kept `env-faithful` as a required gate so packages that silently test the wrong version/channel cannot pass on a lookalike error. I rejected a softer "any version is fine if the symptom matches" wording because that would accept adjacent-version false confidence. The pkg-07 miss shows the remaining tension: when a clear delta callout exists but latest is a major bump away, the check can still false-reject if the model under-weights the callout — tightening the evidence guide's "what good looks like" for delta sentences is the next lever, not dropping the check.

**Trade-offs**

`env-faithful` is the check that changes pkg-07's result relative to gold (accept → reject). I accept that packages which test a nearby older line with an explicit callout but skip naming the latest major may still fail under a strict reading; that is the canary this check buys — catching silent wrong-channel/wrong-era runs. Nothing else in the 19/20 agreement set flipped: clear-accept stayed 7/8 only because of this one env miss.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
