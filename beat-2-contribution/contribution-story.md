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
**Pull Request:** https://github.com/devdavidejesus/btc-toolkit/pull/84

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

On Nov 9 the first ship-gate run (5:11 PM CST) passed all five repo checks. It still blocked because my pr-precheck couldn't finish: claude exited 1: Not logged in · Please run /login. The gate removes ANTHROPIC_API_KEY so it can use a subscription login, so for that run I passed the key in through a wrapper. Run 2 (5:16 PM CST) then passed all 9 pr-precheck checks, including a re-run of the utxo repro. Afterward I cut my manifest from 5 must-pass rows to 4 (setup, tests under coverage, ruff, mypy) to fit the 2–4 rule. The Atheris fuzzer and the CodeQL/OS matrix stayed in the excluded table, each with a dated reason.

**Unit 8: [one line on the tool work]**

My PR monitor reads the PR on GitHub using only read-only gh calls: CI checks, reviews, comments, days of quiet, mergeability, and whether it's merged or closed. It writes a draft of my next move to a file and never posts anything. When the maintainer's review came in on PR #84, the full monitor run failed with "Not logged in," so I followed my playbook's rules myself. Those rules say to push every requested change as one update before replying, and never to argue a maintainer's style or scope call. My reply should list what I changed in the order the points were raised, and I can't say "fixed" without a commit SHA. I made both changes in one commit and replied "Thanks, both done in 186ee49," followed by a list of each change.

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

Issue #63 asked for a docs/cookbook.md page of 5–8 copy-paste --json + jq recipes, but btc-toolkit had no such page. Nothing showed how to script its JSON output, and nothing checked that a recipe used real commands or documented keys.

**Expected Behavior**

Each recipe is a command plus one line on the question it answers. It uses only keys documented in docs/json-schema.md, runs against mainnet, and shows real sample output. The README links to the page.

**Current Behavior**

When I ran the issue's own utxo example on an address the API refuses, btc-toolkit returned an error object with no utxos field. Then jq '[.utxos[] | ...]' crashed with jq: error (at <stdin>:4): Cannot iterate over null (null). I got the same result on Oct 21 and again on Nov 9.

**Affected Components**

- `docs/cookbook.md` (new file)
- `README.md`, Scripting section (the link next to the `docs/json-schema.md` link)
- `CHANGELOG.md`, `[Unreleased]`
- `docs/json-schema.md` (read only: the source of every key the recipes use)

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

The root cause was that the recipe iterated .utxos[], a field that's null when the command returns an error object. Plain [] fails on null, while []? skips it, which is what the issue's opreturn example already did.

**Proposed Solution**

I wrote a cookbook doc because that's what the issue asked for. I added a test because docs drift quietly: a renamed flag or a misspelled key would break a recipe and nobody would notice. tests/test_cookbook.py checks every recipe against the real CLI and docs/json-schema.md, so the cookbook stays correct as the project changes, even though CONTRIBUTING only requires tests for code changes.

**Implementation Plan**

1. Add `docs/cookbook.md` with 5-8 recipes, each a command, one line on the question it answers, and one real mainnet sample output, using only keys from `docs/json-schema.md`.
2. Link the page from the README next to the `docs/json-schema.md` link, and add a line under `[Unreleased]` in `CHANGELOG.md`.
3. No test changes: CONTRIBUTING.md asks for tests on code changes, and this change has no code.

Deviations:

- 2026-11-09: rebuilt on upstream `main` f7d48e4 instead of 55b9e19. Upstream rewrote the README in #83, so the link went into the Scripting section instead of Automation.
- 2026-11-09: eight recipes, the top of the 5-8 range.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

I re-ran the failing repro. The old filter still gave Cannot iterate over null, and the []? version printed null and exited with code 0. Then I added tests/test_cookbook.py, an offline unittest with 4 tests:

- each recipe has one command block and one sample output
- each uses a real btc-toolkit command and real flags with --json
- each reads only documented keys
- the README and docs/commands.md both link to the page

It also requires at least 5 recipes. The test fails on upstream main (docs/cookbook.md is missing) and fails if I change a key to .fee_rate. It passes on my branch. The full suite ran 310 tests with no failures at 96% coverage, ruff and mypy --strict were clean, and every CI check on the final PR passed.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

**2026-10-21**

Forked devdavidejesus/btc-toolkit, cloned the fork, and reproduced the gap at 55b9e19: no `docs/cookbook.md`, and `.utxos[]` fails in jq with "Cannot iterate over null" on an address the API returns an error for.

**2026-11-09**

Installed the ship-gate and wrote `ship-manifest.md` (five checks from CONTRIBUTING.md and `.github/workflows/tests.yml`; fuzzer and CodeQL excluded with a dated reason). Branched `docs/63-jq-cookbook` from upstream `main` f7d48e4 and committed `docs/cookbook.md`, the README link and the CHANGELOG line as 3a1bc7c. Ran all eight recipes on mainnet, the test suite (306 tests, OK), coverage (96%), ruff and mypy --strict; all passed. The ship-gate passed on the branch.

**2026-11-09**

The push of `docs/63-jq-cookbook` to my fork was rejected because the upstream commits it carries change `.github/workflows/` files and my GitHub token has no `workflow` scope. The fork's `main` needs a sync before the branch can go up.

**Nov 9**

I committed the cookbook as 3a1bc7c at 5:06 PM CST. The push to my fork was rejected because GitHub won't let an OAuth app update .github/workflows/codeql.yml without the workflow scope. I synced my fork with upstream and pushed again without force. After the maintainer assigned me the issue and left four notes, I added the test (77959ba) and the follow-ups (65c8970). I opened PR #84 at 6:45 PM CST.

**Nov 10**

The maintainer (@devdavidejesus) asked for two changes in a comment from 6:57 PM CST the night before, and he merged main into my branch that morning. In 186ee49 (1:38 PM CST) I removed the 8-recipe cap from test_recipes_are_well_formed and kept the 5-recipe minimum. I also reworded the "old address" line: transaction outputs never change, but address outputs "should rarely change," since anyone can still send coins to an old address. Then I replied on the PR with the SHA.

**Nov 10, about 7:50 PM CST**

He approved with "Thanks for the quick follow-up, both changes look good. Merging." He merged PR #84 into main and closed #63 as completed.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** https://github.com/devdavidejesus/btc-toolkit/pull/84

**PR Description:** From the PR's "What and why": "Adds `docs/cookbook.md`, a page of eight `--json` recipes with `jq`, as asked in #63. Each recipe is a command and one line on the question it answers, uses only keys documented in `docs/json-schema.md`, and has a real mainnet sample output under it (run 2026-11-09 with v1.8.2 and jq 1.7). The page is linked from the README's Scripting section and from the index at the top of `docs/commands.md`, and `CHANGELOG.md` has an entry under `## [Unreleased]`." Full description: https://github.com/devdavidejesus/btc-toolkit/pull/84

**Maintainer Feedback:**

- 2026-11-09: on the issue thread, the maintainer accepted my claim ("Thanks, it's yours, and Sunday works") and asked for four things in the PR: a link in the `docs/commands.md` index, one recipe that filters errors with `select(has("error"))` and mentions exit codes 1 and 2, old confirmed txids and addresses so the outputs stay the same, and a line under `## [Unreleased]` in `CHANGELOG.md`.
- 2026-11-09: commit 65c8970 added the `docs/commands.md` link, changed the failed-items recipe to `select(has("error"))` with exit codes 1 and 2 (its input now includes `not-a-txid`, so the batch exits 2), moved the balance and UTXO recipes to addresses unused since 2011 and 2012, and replaced the `fees` recipe with `tx --outputs` on the pizza transaction. The CHANGELOG line was already in 3a1bc7c.

**Status:** Merged into `main` on 2026-11-10 (about 7:50 PM CST); #63 closed as completed.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

In jq, .field[] fails on null but .field[]? skips it. That matters most with a CLI's error objects. I also learned to build on top of a maintainer's merge commit on my branch rather than rewriting history or force-pushing over his work.

**Challenges Overcome**

My local main was stale, still at Oct 21's 55b9e19. So the git diff main...HEAD the gate collected for run 2 included about 30 upstream files that weren't mine. It still graded my one real commit, but I had to reset local main to upstream. The lesson is that the gate trusts whatever base ref I have locally. The workflow-scope push rejection was also confusing until I figured out it was because my fork was out of date.

**What I'd Do Differently Next Time**

My tools caught real problems before a maintainer did. The gate blocked on a placeholder "TODO" line, plan-check pushed me to add a test, and the voice guard flagged a claim with no check-in date. Because I'd written the playbook ahead of time, I knew how to answer the review as soon as it came in, even though the monitor itself couldn't run. Next time I'll check my logins and my local main before the first gate run.

---

## Resources Used

- [btc-toolkit issue #63](https://github.com/devdavidejesus/btc-toolkit/issues/63): the recipe examples and the "Done when" line
- [btc-toolkit CONTRIBUTING.md](https://github.com/devdavidejesus/btc-toolkit/blob/main/CONTRIBUTING.md): branch from `main`, the CHANGELOG rule, and "Claims are verifiable"
- [docs/json-schema.md](https://github.com/devdavidejesus/btc-toolkit/blob/main/docs/json-schema.md): every key the recipes use, the error object shape, and exit codes
- [btc-toolkit tests workflow](https://github.com/devdavidejesus/btc-toolkit/blob/main/.github/workflows/tests.yml): the exact test, coverage, ruff and mypy commands
- [jq manual](https://jqlang.org/manual/): `[]?`, `// empty` and `@tsv`
