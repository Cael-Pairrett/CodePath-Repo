I graded both issues. Only **btc-toolkit#63** is accepted; **tongs#328** is rejected.

**Accepted**

1. **devdavidejesus/btc-toolkit#63 — "Cookbook: --json recipes with jq"** (direct). This fits your profile closely. It's a docs task in a Python command-line tool: add one new `docs/cookbook.md` page and link it from the README. The work uses JSON output and `jq`, which is close to your data background, and it's small enough to finish in two weeks. It passes both preferred checks: it has the `good first issue` label, and the owner opened it themself and asks people to "Comment below to take this issue." One thing to expect: the issue says every recipe must be "run against mainnet or signet, with one real sample output", so you'd be running real Bitcoin queries, not just writing text.
   - maintainer-alive: pass. The owner merged PR #69 from an outside contributor on 2026-10-19.
   - repo-in-use: pass. Not archived, created 2026-04-07, last push 2026-10-19, latest release v1.8.1 on 2026-10-15.
   - bounded-scope: pass. It asks for one new page plus a README link.
   - unclaimed: pass. No assignees, no comments, no linked or open PRs. It was opened 4 days ago.
   - ai-contribution-allowed: pass. CONTRIBUTING.md and the PR template say nothing about AI.

**Rejected**

2. **andre-motta/tongs#328 — "Remove the unused MRFilter and untangle the draft store's import cycle"** (direct). It fails **bounded-scope**. The issue says it "comes in two independent parts, and each one can be its own small PR", and each part has its own goal and its own "Done when". The rubric asks for one change working toward a single goal. Part 2 also moves code to break an import cycle, in an area labelled `area: core`, which is the kind of internals work your profile says to avoid. All the other required checks pass:
   - Commits from 2026-10-21 and v1.1.1 was released today.
   - No assignees, comments or PRs. The only cross-reference is the parent audit issue #330.
   - CONTRIBUTING.md has no AI ban; it only asks for a `Co-Authored-By:` line naming the AI tool.

**Rubric gap worth logging in `scan-notes.md`:** under the rubric, #328 fails only because it bundles two parts. Part 1 alone is a small, clearly defined first task (delete one unused class and fix two sentences in the guides), and the maintainer openly invites questions. The rubric can't currently accept just one part of a multi-part issue. If you want it to, patch bounded-scope to allow that, for example: "passes if the issue names one independently completable part that meets this condition on its own". Under the current rubric the verdict stays reject.

```json
[
  {
    "item": "https://github.com/devdavidejesus/btc-toolkit/issues/63",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Owner devdavidejesus committed/merged on 2026-10-19 (Merge PR #69 from montionugera, Merge PR #70), 2 days before today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; created 2026-04-07; pushed_at 2026-10-19; latest release v1.8.1 on 2026-10-15"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "\"Add docs/cookbook.md with 5–8 recipes ... and link the new page from the README\": one new docs page plus a cross-reference"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; 0 comments; no linked or open PRs; timeline shows only the two label events"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md and pull_request_template.md contain no AI policy; silence passes"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "Labels: documentation, good first issue"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Opened by repo owner devdavidejesus: \"Comment below to take this issue.\""}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/andre-motta/tongs/issues/328",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Owner andre-motta merged PR #465 from JessN777 and PR #467 on 2026-10-21"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; created 2026-07-19 (94 days ago); pushed_at 2026-10-21; latest release v1.1.1 on 2026-10-21"},
      {"name": "bounded-scope", "grade": "fail", "evidence": "\"It comes in two independent parts, and each one can be its own small PR\": two separate goals (dead-code/doc cleanup; breaking the draft-store/services import cycle), each with its own Done-when"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; 0 comments; no linked PRs; only cross-reference is open parent issue #330 (audit tracker); no PR mentions MRFilter"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md: \"If an AI assistant wrote part of a commit, credit it with a Co-Authored-By: trailer\": a disclosure condition, not a ban"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "Labels: documentation, good first issue, tech-debt, area: core"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Opened by repo owner andre-motta and linked from his audit tracker #330"}
    ],
    "verdict": "reject"
  }
]
```
