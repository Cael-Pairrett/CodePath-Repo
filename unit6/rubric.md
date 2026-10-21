# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: "last 5 default-branch commits" and "maintainer first-response sample" | At least one non-bot-authored commit within 30 days of the capture date, OR at least one sampled issue shows a first response from an owner/member/collaborator within 30 days of that issue being opened | required |
| repo-in-use | Repo facts: "archived:" flag, repo creation date, "last push to any branch", "latest release" | `archived` is `no`, AND the repo was created at least 60 days before the capture date, AND (last push to any branch is within 90 days of the capture date OR the latest release is within 180 days of the capture date) | required |
| scope-feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. | required |
| unclaimed | Repo facts: "this issue: assignees" and "linked PRs"; the comment thread | `assignees` is empty, AND no linked PR is open, AND no *recent* comment shows a maintainer acknowledging or assigning the issue to a specific claimant. A claim (even one a maintainer once acknowledged) that has had no follow-up activity in over a year, with no open PR, is stale and does not count as currently claimed; only an active, recent claim blocks this check | required |
| ai-contribution-allowed | Repo facts: "contribution policy" line | The contribution policy does not contain an outright ban on AI-generated or AI-assisted contributions. Disclosure requirements, human-review requirements, and "you must understand and test everything" conditions all pass. No stated policy also passes | required |
| good-first-issue-label | The issue's labels | The issue carries a "good first issue" label or clear equivalent (e.g. "beginner-friendly", "help wanted: easy") | preferred |
| maintainer-endorsed | The issue body and comment thread | The issue was opened by, or has an explicit comment of endorsement/approval from, someone with an owner/member/collaborator badge | preferred |

## Difficulty note

With each accepted candidate, report a one-line difficulty note in
the read-out summary:

- **straightforward**: the issue names where the change lives and the
  change is bounded.
- **moderate**: the location is yours to find, or several files are
  involved.
- **ambitious**: an open design edge, or an unfamiliar subsystem;
  name the edge, because claiming this issue means raising it with
  the maintainer early.

The note is information for the chooser, never a verdict input: it
does not appear in the fenced JSON block, it never changes accept or
reject, and in eval mode it is skipped entirely.

## Verdict rule

Accept only if every `required` check grades `pass`. Reject if any
`required` check grades `fail` or `unclear` — treat `unclear` the same as
`fail`, since a first issue you cannot verify is not one you should take.

`preferred` checks never change the verdict. They only rank issues that
are already accepted: prefer an accepted issue with more `preferred`
passes, and note in the summary which preferred checks made the
top-ranked issue the best pick.
