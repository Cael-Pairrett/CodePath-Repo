# Scan 1 (2026-10-21)

The scan ran end to end, and all 5 graded candidates came back **accept**. But the queries didn't search the way `search-strategy.md` meant them to, so read the results with that in mind.

## The main finding: most of the query filters were never applied

Both queries pass their search terms as one quoted string after `--`, e.g. `"-org:codepath comments:<10 created:<2026-10-01 -linked:pr"`. I tested query 2 with the same flags both ways:

- **As written (one string):** 100 of 100 results were created on or after 2026-10-01. The `created:<2026-10-01` filter was ignored.
- **Each term as its own argument** (`-- -org:codepath "comments:<10" "created:<2026-10-01" -linked:pr`): 0 of 100 were.

So the quoted terms were not treated as filters. That covers `created:<`, `comments:<10`, `-linked:pr` and `-org:codepath`, plus `csv OR json OR pandas` and `-label:question` in query 1. Only the flags worked: label, language, `--no-assignee` and state. Your `created:<2026-10-01` filter would have dropped all 5 candidates below. They were all opened today, which is exactly what that filter was meant to keep out.

## The funnel

- **Search:** 100 + 100 results, 199 after removing duplicates.
- **Ranking:**
  - Filter 2 (stale) dropped 32, filter 4 (Feat titles) dropped 5, filter 5 (codepath) dropped 1. Filters 1 and 3 dropped none. 161 survived.
  - Sorted by filter 6: bug/docs label first, then fewest comments, then most recently updated.
  - The top 5 all have the docs label, 0 comments, and were updated on 2026-10-21. Most-recent update decided the order.

## Ranked read-out (all accepted, in fit order)

All five pass every required check: the maintainer committed today, the repo was pushed today and isn't archived, the scope is bounded, nobody has claimed it, and the repo doesn't ban AI. Both preferred checks pass too (good-first-issue label, and opened by the owner), so fit decides the order.

1. **git-into-gsoc #162**: replace real SymPy issue numbers with placeholders. Smallest and least ambiguous, and a `grep` tells you when you're done. It's a Python tooling project with an MCP server and an Agent Skill, which fits your goal of using AI in your workflow.
2. **git-into-gsoc #164**: add four one-sentence definitions to guide 5. Tiny, but you write the sentences yourself.
3. **git-into-gsoc #163**: fix two out-of-date sentences in guide 8. You need to install and run the CLI to check them.
4. **git-into-gsoc #166**: restructure the token-setup docs. The largest of the four, and it covers several platforms.
5. **brasildetodos #19**: translate the README into English. It fits the data space (Brazilian public-data platform), but you'd need to read Portuguese and test the SQLite/PostgreSQL quickstart. Ranked last on fit.

Every candidate came from query 2 (docs + good first issue), and every one was placed by the ranking signals above.

## Things to fix in the strategy and rubric

1. **The query bug above.** Split each term into its own argument in both queries, then re-scan.
2. **Too much from one repo:** 4 of the 5 are from git-into-gsoc. A per-repo cap (say, at most 2) would fix it.
3. **The ordering favours brand-new issues.** "0 comments, newest first" puts issues filed minutes ago at the top. Fixing the `created:` filter partly addresses this.
4. **The rubric's "in use" check measures activity, not adoption.** git-into-gsoc was created today and has 1 star; brasildetodos has 0 stars. Both still pass "maintainer alive" and "in use" on a single day of commits. The evidence guide lists star count as an adoption signal, but no check uses it. The rubric passed them, and per the skill's rules that verdict stands; the fix belongs in the rubric.
5. **brasildetodos and AI tools:** its `AGENTS.md` says automated agents must work only on `main` with no branches or PRs. That isn't a ban on AI-assisted work, so the check passes. git-into-gsoc welcomes AI use if you disclose it, but its CONTRIBUTING also warns against "generic AI-written proposals".

I couldn't save `scan-notes.md`: file writes in this session need your approval. I can write it once you allow that, or fix the two queries in `search-strategy.md` and run a clean scan. If you take one of these issues, comment on it straight away to claim it, as `scope.md` says.

```json
[
  {"item": "https://github.com/anandkaranubc/git-into-gsoc/issues/162", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "anandkaranubc (owner, non-bot) committed 2026-10-21T22:31Z"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived false; pushedAt 2026-10-21; release v0.10.0 2026-10-21"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "One goal: replace real sympy issue refs with OWNER/REPO#NUMBER placeholders in 4 listed files; grep check given"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, 0 comments, no PR mentions #162"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING: 'You are welcome to use AI assistants... mention it in the pull request'"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "labels: documentation, good first issue"},
    {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by repo owner anandkaranubc"}], "verdict": "accept"},
  {"item": "https://github.com/anandkaranubc/git-into-gsoc/issues/164", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "anandkaranubc (owner, non-bot) committed 2026-10-21T22:31Z"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived false; pushedAt 2026-10-21; release v0.10.0 2026-10-21"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Add one-sentence definitions for four named terms in docs/guide/05-choose-an-issue.md"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, 0 comments, no PR mentions #164"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING 'Using AI tools' section welcomes AI with disclosure"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "labels: documentation, good first issue"},
    {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by repo owner anandkaranubc"}], "verdict": "accept"},
  {"item": "https://github.com/anandkaranubc/git-into-gsoc/issues/163", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "anandkaranubc (owner, non-bot) committed 2026-10-21T22:31Z"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived false; pushedAt 2026-10-21; release v0.10.0 2026-10-21"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Fix two stale sentences (lines 9 and 25) in docs/guide/08-track-progress.md"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, 0 comments, no PR mentions #163"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING 'Using AI tools' section welcomes AI with disclosure"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "labels: documentation, good first issue"},
    {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by repo owner anandkaranubc"}], "verdict": "accept"},
  {"item": "https://github.com/anandkaranubc/git-into-gsoc/issues/166", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "anandkaranubc (owner, non-bot) committed 2026-10-21T22:31Z"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived false; pushedAt 2026-10-21; release v0.10.0 2026-10-21"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Reorder docs/configuration.md token methods: gh auth login, env var, then .env"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, 0 comments, no PR mentions #166"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING 'Using AI tools' section welcomes AI with disclosure"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "labels: documentation, good first issue"},
    {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by repo owner anandkaranubc"}], "verdict": "accept"},
  {"item": "https://github.com/rafaelob/brasildetodos/issues/19", "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "rafaelob (owner, non-bot) committed 2026-10-21T22:28Z"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived false; pushedAt 2026-10-21 (no releases; push within 90 days suffices)"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Create README.en.md translating current README; 'Não é necessário traduzir toda a documentação histórica'"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees [], no linked PRs, 0 comments, no PR mentions #19"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "No ban; CONTRIBUTING only says automated agents follow AGENTS.md (main only, no branches/PRs)"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "labels: documentation, good first issue, translations"},
    {"name": "maintainer-endorsed", "grade": "pass", "evidence": "opened by repo owner rafaelob; body says 'Bom para primeira contribuição'"}], "verdict": "accept"}
]
```
