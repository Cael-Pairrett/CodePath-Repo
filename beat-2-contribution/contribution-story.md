# Contribution: Cookbook: --json recipes with jq

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Cael Pairrett
**GitHub Username:** Cael-Pairrett
**Issue:** https://github.com/devdavidejesus/btc-toolkit/issues/63
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my unit 1 issue-select skill into issue-scout and wrote my own search strategy.**

I installed the scout over my unit 1 skill, updated my fit profile, and wrote two `gh search issues` queries (Python data issues and Python docs issues) with ranking rules that put bug and docs labels first and the fewest comments first. My first scan accepted all five issues it graded, but four came from one repo created that day, because I had put my search filters in one quoted string and gh ignored them; I split the filters into separate arguments, capped results at two per repo, and added a rule to my rubric that a repo must be at least 60 days old. The second scan rejected the two too-new repos it found, but my first query still lets brand-new issues through, and when I graded my picks the bounded-scope check rejected tongs#328 for being two parts after the scan had passed it, so my next fixes are splitting that query and making bounded-scope clear about multi-part issues.

**Unit 6: Installed the voice guard with my own Upstream rules and wrote a CLAUDE.local.md for btc-toolkit.**

I folded my three voice-guide copies into one guide, added Upstream rules for claims, questions, and pings, and installed the voice-guard hook; the wiring test was stopped with "no body the guard can read", which is the pass. My claim draft passed the guard on the first check, but repro-check pointed out it said two recipes "both work" with no output behind it, so I added the actual numbers. I also wrote a 32-line CLAUDE.local.md for my btc-toolkit fork from one scoped read of docs/json-schema.md and the README, and kept it out of git with .git/info/exclude.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

This issue came from my own scout scan, not the Issue Selection site, because the site needs a browser sign-in I didn't use. My second scan (after I fixed my queries) surfaced btc-toolkit #63 from my Python data/JSON query, and when I graded it with `issue-scout: grade` after this week's scope patch, it was accepted on all five required checks and both preferred ones, with a difficulty note of straightforward: the issue names the file to add and the JSON keys to use. It fits my profile because it's a small, clear docs change in a Python CLI, and the work is writing jq recipes over JSON output, which is close to the data work I do in my analytics classes. I hope to learn the full claim, PR, and review loop on a real repo with strict CI, and to get better at jq. Nobody had commented on or been assigned to the issue when I read the full thread.

My scout was useful but needed real fixes. My first scan accepted five issues, four from a repo created that day, because my search filters were in one quoted string and gh ignored them. I fixed the queries and added a 60-day repo-age rule. Then my bounded-scope check gave two different answers on tongs#328, so this week I replaced it with scope-feasible, which only rejects umbrella issues and gives a difficulty note instead. One weakness is still open: my first query still lets brand-new issues through, and the scout can't tell when an owner labels every issue "good first issue".

**Claim comment:** claim pending (2026-10-21)

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran two scans and then graded my picks directly with `issue-scout: grade`.
- What it produced: scan 2 surfaced #63 from my Python JSON query, and the Unit 6 re-grade accepted it with every required and preferred check passing and a "straightforward" difficulty note.
- Difficulties: my first queries didn't apply their filters, and bounded-scope rejected tongs#328 for having two parts after the scan had passed it.
- What changed: I split the query filters into separate arguments, added a 60-day repo-age rule to repo-in-use, and replaced bounded-scope with scope-feasible plus a difficulty note.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Debian Linux, Python 3.13.5, jq-1.7, and btc-toolkit 1.8.1 at upstream `main` 55b9e19 (2026-10-19). I forked the repo with `gh repo fork devdavidejesus/btc-toolkit`, cloned my fork, and ran the tool with `python3 -m btc_toolkit` from the clone root, since it has no runtime dependencies. The recipes query mainnet through the tool's default public API. My first try at a venv install failed because the box has no ensurepip, so I ran the module directly instead. One address I tried returned an API 400, so I used a different public address for the utxo recipe.

**Steps to Reproduce**

1. Clone the fork at 55b9e19 and run `ls docs/`, `ls docs/cookbook.md`, and `grep -n cookbook README.md`.
2. Run the issue's fee-rate recipe: `echo a1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d | python3 -m btc_toolkit tx - --json | jq -r '.fee_rate_sat_vb // empty'`.
3. Run the issue's utxo recipe on `17SkEw2md5avVNyYgj6RiXuQKNwkXaxFyQ`, and again on `bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h`.
4. Observed result: there is no `docs/cookbook.md` and the README only mentions the cookbook in its roadmap line, so the gap is real. The recipes return real output (4191.36 and 1935301). When the API returns an error object, the utxo recipe fails in jq.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

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

- **My findings:** The gap the issue describes is real on today's `main`. The example recipes work on mainnet, but `.utxos[]` breaks with "Cannot iterate over null" when the tool returns an error object, so my cookbook should use `.utxos[]?` the way the issue's opreturn example already uses `.outputs[]?`.

**Toolkit**

- Tool: repro-check, with the wild scope installed this week. I graded my claim draft alone, then my claim and repro report together.
- What it produced: an accept on the claim alone, then an accept with all seven required checks passing on the claim and report together.
- Difficulties: the first joint check said my evidence block looked retyped (shortened txids, `btc-toolkit` instead of `python3 -m btc_toolkit`, a comment inside the code block), and it flagged that I called the address "busy" with nothing to show that.
- What changed: I re-ran every command and pasted the raw output, recorded my jq version, and cut "busy". The re-check passed with no fixes listed.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
