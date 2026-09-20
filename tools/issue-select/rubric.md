# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: "last 5 default-branch commits" and "maintainer first-response sample" | At least one non-bot-authored commit within 30 days of the capture date, OR at least one sampled issue shows a first response from an owner/member/collaborator within 30 days of that issue being opened | required |
| repo-in-use | Repo facts: "archived:" flag, "last push to any branch", "latest release" | `archived` is `no`, AND (last push to any branch is within 90 days of the capture date OR the latest release is within 180 days of the capture date) | required |
| bounded-scope | The issue body and comment thread | The issue asks for one identifiable change working toward a single stated goal, even if it touches several files or sections to get there (e.g. one new docs page plus updating its cross-references is still one change). Fails if: the issue explicitly frames itself as a tracking/umbrella issue whose listed sub-items are meant to become separate issues or PRs; the thread shows an unresolved technical/design debate (disagreement about exact behavior or format) that no maintainer ever settled; a maintainer states the fix needs deep/core internal changes; the thread shows two or more closed-unmerged linked PRs from different contributors who each abandoned the attempt (a sign the fix is harder than the description suggests); or it is a feature request with a product decision embedded in it (what the feature should be, whether it belongs in core) that no maintainer has triaged or endorsed at all | required |
| unclaimed | Repo facts: "this issue: assignees" and "linked PRs"; the comment thread | `assignees` is empty, AND no linked PR is open, AND no *recent* comment shows a maintainer acknowledging or assigning the issue to a specific claimant. A claim (even one a maintainer once acknowledged) that has had no follow-up activity in over a year, with no open PR, is stale and does not count as currently claimed; only an active, recent claim blocks this check | required |
| ai-contribution-allowed | Repo facts: "contribution policy" line | The contribution policy does not contain an outright ban on AI-generated or AI-assisted contributions. Disclosure requirements, human-review requirements, and "you must understand and test everything" conditions all pass. No stated policy also passes | required |
| good-first-issue-label | The issue's labels | The issue carries a "good first issue" label or clear equivalent (e.g. "beginner-friendly", "help wanted: easy") | preferred |
| maintainer-endorsed | The issue body and comment thread | The issue was opened by, or has an explicit comment of endorsement/approval from, someone with an owner/member/collaborator badge | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. Reject if any
`required` check grades `fail` or `unclear` — treat `unclear` the same as
`fail`, since a first issue you cannot verify is not one you should take.

`preferred` checks never change the verdict. They only rank issues that
are already accepted: prefer an accepted issue with more `preferred`
passes, and note in the summary which preferred checks made the
top-ranked issue the best pick.
