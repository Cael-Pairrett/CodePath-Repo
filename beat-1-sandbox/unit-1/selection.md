# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73

**Verdict output**

Live-mode run of the installed `issue-select` skill (`~/.claude/skills/issue-select/`)
on three candidate issues from `codepath/pathreview-ai301-fa26-s1`, per `scope.md`:

````
Accepted candidates, in fit order:

1. #73 — README and .env.example disagree about which LLM API key to set
   Fit: my profile says to start with something config/docs-shaped rather than a fix
   buried in a subsystem I haven't touched yet; this is exactly that — no retrieval,
   security, or async-test internals, just aligning two files' documented API key
   names.

2. #72 — verify_password raises UnknownHashError instead of returning False
   Fit: touches core/security.py logic (one function, one xfail test to un-mark), a
   step up from #73 in that it's real backend code, but still small and self-contained.

3. #68 — KeywordSearcher.index() raises ZeroDivisionError on an empty corpus
   Fit: accepted, but lowest fit for me right now — it's in the RAG/retrieval stack,
   which my profile flags as the internals I have the least familiarity with, and it
   carries the largest estimated effort (2-4h vs 1-2h) of the three.

Rejected candidates: none — all three passed every required check.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Aburke225 (repo maintainer) committed to main on 2026-09-16, 4 days before this run"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push to main on 2026-09-16"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single stated goal (align README and .env.example on the LLM API key name), estimated effort 1-2 hours, no umbrella/design-debate/abandoned-PR signals"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; comments: none"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI-contribution policy; silence passes"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "labeled 'good first issue' and 'tier-1'"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by Aburke225, the repo's maintainer"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "same repo, last maintainer commit 2026-09-16"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 2026-09-16"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single function fix in core/security.py plus un-xfailing one named test, estimated 1-2 hours"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; comments: none"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "no stated AI policy in CONTRIBUTING.md"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "labeled 'good first issue' and 'tier-1'"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by Aburke225, the repo's maintainer"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "same repo, last maintainer commit 2026-09-16"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 2026-09-16"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single method fix (KeywordSearcher.index()) across two named files, estimated 2-4 hours"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; comments: none"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "no stated AI policy in CONTRIBUTING.md"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "labeled 'good first issue' and 'tier-1'"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by Aburke225, the repo's maintainer"}
    ],
    "verdict": "accept"
  }
]
```
````

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--limit 3` smoke run (first draft rubric): 2/3 scored items.
2. `--limit 20` full smoke run (first draft rubric): 17/20 scored items — missed
   `issue-01` (bounded-scope), `issue-09` (unclaimed), `issue-15` and `issue-20`
   (both bounded-scope).
3. After revising `bounded-scope` and `unclaimed` (see Check rationale/Trade-offs
   below) and confirming the three disagreements individually with `--only
   issue-09,issue-15,issue-20`: `--limit 20` full smoke run: 19/20 scored items
   (missed `issue-19`).
4. Final confirming full run, `--save-run eval-run.txt`: **20/20 scored items** —
   matches the `agreement: 20/20 scored items  (bar: 18/20: PASS)` line in
   `eval-run.txt`.

**Issue analysis**

`issue-15` (gold: `reject`, my rubric: `reject`, agreement: yes). The bundle looks
bounded on its surface — "separate `command` and `text` field for slack-compatible
outgoing webhook" — but the 97-comment thread shows a technical disagreement between
the reporter and a maintainer over the exact payload format and transformation
behavior that was never resolved, plus two closed-unmerged linked PRs from different
contributors who each abandoned the attempt after being auto-unassigned for
inactivity. My rubric's `bounded-scope` check fails an issue when its thread shows
"an unresolved technical/design debate... that no maintainer ever settled" or "two or
more closed-unmerged linked PRs from different contributors who each abandoned the
attempt," which is exactly this issue's shape. My first-draft rubric didn't check for
either signal and graded this `accept`; adding them brought it to `reject`, matching
the gold label the maintainer's own repeated re-assignment cycles were pointing at.

**Check rationale**

From `tools/issue-select/rubric.md`, the `bounded-scope` check's pass condition:

> The issue asks for one identifiable change working toward a single stated goal,
> even if it touches several files or sections to get there (e.g. one new docs page
> plus updating its cross-references is still one change). Fails if: the issue
> explicitly frames itself as a tracking/umbrella issue whose listed sub-items are
> meant to become separate issues or PRs; the thread shows an unresolved
> technical/design debate (disagreement about exact behavior or format) that no
> maintainer ever settled; a maintainer states the fix needs deep/core internal
> changes; the thread shows two or more closed-unmerged linked PRs from different
> contributors who each abandoned the attempt (a sign the fix is harder than the
> description suggests); or it is a feature request with a product decision embedded
> in it (what the feature should be, whether it belongs in core) that no maintainer
> has triaged or endorsed at all.

I wrote it this way because my first draft ("one identifiable, describable change")
was too strict in one direction and too loose in another. `issue-01` is a docs
initiative that touches five different pages toward one goal (documenting a single
new workflow), and the first draft's wording read that as an umbrella issue and
rejected it; I added the "even if it touches several files... toward a single stated
goal" clause so multi-file work in service of one goal still passes. In the other
direction, the same first draft missed that a request can look like one bounded ask
in its own words while the thread (repeated abandoned PRs, an unsettled format
argument) or the issue itself (an untriaged feature idea with a hidden product
decision) shows it is not actually scoped yet — so I added the three thread/PR/
triage-based failure conditions to catch that.

**Trade-offs**

Loosening the "single stated goal" wording to admit multi-file work cost me
precision on `issue-01`-shaped cases: I re-ran that exact change as a canary with
`--only issue-01,issue-05,issue-10` after the edit. `issue-01` flipped from `reject`
to `accept` (correct — gold is `accept`), while `issue-05` (duplicate, gold
`reject`) and `issue-10` (megaissue, gold `reject`) stayed `reject`, so the looser
wording didn't drag in the two scope-family rejects sitting right next to it. What it
still accepts that a stricter, single-file-only version would have caught for free:
a multi-file change where each file's slice is individually fine but the *set* of
files is actually two unrelated goals wearing one issue number — my check only
looks for an explicit "these should be separate issues" framing or thread signals,
not an implicit one, so a well-worded but genuinely two-goal issue could still slip
through as accepted.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

<!-- DRAFTED FOR REVIEW: written from a generic fit profile since I hadn't given
specific notes. Read it, and edit anything that doesn't sound like me before
submitting. -->

**Selection rationale**

1. I'm new to contributing to a codebase I didn't write, so a first issue that's
   about fixing a factual inconsistency (README vs. `.env.example`) rather than
   changing behavior fits where I actually am right now, and it fits comfortably
   inside an hour or two alongside everything else due this week.
2. The verdict's `unclaimed` and `maintainer-alive` checks confirmed something I
   couldn't have judged just by reading the issue body — that nobody else had
   already commented or opened a PR against it, and that the repo had a real commit
   four days ago rather than sitting idle. What I weighed on top of that (the rubric
   can rank preferred checks but can't feel it) was that #72 and #68 were both also
   accepted and technically more interesting, but as a genuine first PR I'd rather
   bank a small, easy win in this repo's actual review process before taking on
   security- or retrieval-code logic I haven't touched yet.
3. Low difficulty expected: no assignee or existing PR to race, the fix is a
   two-file text change with no ambiguity about what "correct" looks like (the
   README and `.env.example` just need to agree, and `core/config.py` already
   supports both key names), and the Path Review house rule means even if a
   classmate also comments on it, claiming it costs nothing.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
