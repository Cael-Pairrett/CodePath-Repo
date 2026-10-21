I graded both issues against the rubric and both are accepted. All five required checks pass for each. I'd rank btc-toolkit #63 first.

**1. devdavidejesus/btc-toolkit#63: "Cookbook: --json recipes with jq"** (direct): **accept**
- maintainer-alive: pass. The owner devdavidejesus merged PR #70 on 2026-10-19, and on the same day merged #69, a PR from an outside contributor.
- repo-in-use: pass. It isn't archived, it was created 2026-04-07, the last push was 2026-10-19, and v1.8.1 came out 2026-10-15.
- scope-feasible: pass. It asks for one new file, `docs/cookbook.md` with "5–8 recipes", plus a link from the README.
- unclaimed: pass. It has no assignees, no linked PRs and no comments. It was opened 4 days ago.
- ai-contribution-allowed: pass. Neither CONTRIBUTING.md nor the PR template says anything about AI, and silence passes.
- Preferred checks: both pass. It has the `good first issue` label, and the owner opened it.
- **Difficulty: straightforward.** The issue names the file and the JSON keys to use, and the change is bounded. The one real task is that every recipe has to be run against mainnet or signet with a real output pasted under it. That means installing the tool and `jq`.
- **Why it ranks first:** it's a documentation fix in a small Python CLI, which is exactly what your profile asks for. JSON and `jq` work also builds on your data background. One thing to know is that the project is about Bitcoin rather than data work.

**2. andre-motta/tongs#328: "Remove the unused MRFilter and untangle the draft store's import cycle"** (direct): **accept**
- maintainer-alive: pass. The owner andre-motta merged #467 today (2026-10-21) and also merged #465 from an outside contributor, JessN777.
- repo-in-use: pass. It isn't archived, it was created 2026-07-19 (94 days ago), and v1.1.1 came out today.
- scope-feasible: pass. It has "two independent parts, and each one can be its own small PR". Each part names exact files and has its own "Done when" list, so it isn't a container issue.
- unclaimed: pass. It has no assignees and no comments. Its only cross-reference is #330, which is the open audit issue that groups it with other fixes, not a pull request.
- ai-contribution-allowed: pass. CONTRIBUTING.md only requires that "If an AI assistant wrote part of a commit, credit it with a `Co-Authored-By:` trailer". That's a condition you can meet, not a ban, and the repo also has an `AGENTS.md`.
- Preferred checks: both pass. It has the `good first issue` label, and the owner opened it.
- **Difficulty: Part 1 is straightforward; Part 2 is ambitious.** Part 1 deletes one class and fixes two sentences in the guides. Part 2 moves the repo and review identity types (`RepositoryRef`, `ReviewRef`, `ReviewRevision`) into a new module to break an import cycle. The open design question there is where that new module should live. The issue only says "for example `src/tongs/identity.py`", so settle that with the maintainer before you start.
- **Why it ranks second:** the repo is very active and its maintainer writes unusually detailed issues and invites questions. But the issue is labelled `area: core`, and Part 2 is the kind of internals work your profile says to avoid. Part 1 on its own would be a good fit.

**Notes:**
- The rubric passes #328 on scope, but that sits uneasily with your profile, since half the issue is internals work. A preferred check like "a sub-part fits the newcomer profile", or a rule for issues that bundle several parts, might be worth adding. I haven't written to `scan-notes.md`; tell me if you want this logged there.
- Following `scope.md`, comment on whichever issue you pick as soon as you commit to it. #63 is only 4 days old and the repo is getting outside PRs. If you take #328, say in your comment which part you're taking.

```json
[
  {
    "item": "https://github.com/devdavidejesus/btc-toolkit/issues/63",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Owner devdavidejesus authored/merged PR #70 on 2026-10-19 (2 days before today)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; created 2026-04-07; last push 2026-10-19; latest release v1.8.1 on 2026-10-15"},
      {"name": "scope-feasible", "grade": "pass", "evidence": "\"Add docs/cookbook.md with 5–8 recipes ... and link the new page from the README\""},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs, zero comments; timeline shows only label events"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md and PR template contain no AI policy; no stated policy passes"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "Labels: documentation, good first issue"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Opened by repo owner devdavidejesus"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/andre-motta/tongs/issues/328",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Owner andre-motta merged PR #467 on 2026-10-21 (today)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; created 2026-07-19 (94 days ago); last push 2026-10-21; latest release v1.1.1 on 2026-10-21"},
      {"name": "scope-feasible", "grade": "pass", "evidence": "\"It comes in two independent parts, and each one can be its own small PR\" — each with named files and Done-when criteria"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, zero comments; only cross-reference is open parent tracking issue #330, not a PR"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md: \"If an AI assistant wrote part of a commit, credit it with a Co-Authored-By: trailer\" — a condition, not a ban"},
      {"name": "good-first-issue-label", "grade": "pass", "evidence": "Labels: documentation, good first issue, tech-debt, area: core"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Opened by repo owner andre-motta"}
    ],
    "verdict": "accept"
  }
]
```
