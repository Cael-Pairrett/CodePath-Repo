---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this pull request
ready to submit? A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence) read against the
plan it claims to implement (the plan's diagnosis, scope, files, test
plan, and any deviation or deferral notes) and against the issue that
plan belongs to (the issue body, the thread, and the repo's stated PR
template and contribution policy). Do not grade the plan itself, do
not re-diagnose the bug, do not review code style beyond what the
rubric asks, and never grade more than one package per run. Answer by
executing the components in this directory, never from gut feel.

## Inputs and modes

Decide the mode first. If you were handed an eval package bundle (a
markdown file headed "Eval package: <id>"), you are in eval mode.
Otherwise you are in live mode.

**Live mode** grades the student's own PR before it is opened. Run
from the top folder of the student's fork clone, on their fix branch.
Read exactly these inputs:

1. `plan.md` in that folder, including its `## Deviations` section.
   This is the plan the PR must implement.
2. The diff on this branch: every committed change the branch makes
   relative to the default branch, produced by running
   `git diff main...HEAD` (three dots) in the working copy. Also run
   `git diff main...HEAD --stat` and `git log main..HEAD --oneline`
   for the file list and commit list. Uncommitted or untracked files
   are not part of the diff.
3. `pr_draft.md`: its first line is the PR title, and everything
   under it is the PR description.
4. `test_evidence.md`: the captured before/after repro output and the
   repo checks' output.
5. The issue named in the command (its URL): read the issue body and
   comment thread with `gh issue view <number> --repo <owner/repo>
   --comments`.
6. The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md` in the working copy (or on the repo's
   default branch on GitHub).

A student on the house chain reads the house plan and the house repro
pack in place of `plan.md` and their own repro; every check grades the
same things there. Do not read other files in the working directory
as part of the package (for example `comment.md` or scratch notes):
the package is what a maintainer will see, which is the diff, the
title and description, and the plan they point to.

**Eval mode** grades a frozen package bundle. The bundle is the whole
world: every fact comes from the bundle text (its repo facts, issue,
thread highlights, plan context, and candidate PR sections). Fetch
nothing, run nothing, read no other file, and ignore `scope.md` and
`voice-guide.md`. Treat every eval package as AI-assisted work when a
check asks about AI disclosure. Eval mode always grades the complete
package: every check in the rubric, then the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Take the repo on
its `Repo:` line and confirm the issue URL, and the PR's target, are
in that repo. If they are not, refuse to grade and say which repo the
scope allows. If the `Repo:` line still carries the placeholder
(`<ORG>/<PATH-REVIEW-REPO>` or any bracketed text), stop without
grading and tell the student to fill the `Repo:` line in `scope.md`
with their section's Path Review repo. Never guess a scope. Apply the
house rules in `scope.md` as part of the evidence: the PR comes from
a `fix/<issue-number>-<slug>`-style branch on the student's fork, it
delivers one posted plan for one issue, and the repo's PR template is
always used. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the
outgoing text, the PR title (first line of `pr_draft.md`) and the
description, against each rule in it. In the summary, list every rule
the draft breaks, quoting the rule's name and the sentence that breaks
it, or say that no rule is broken. The voice guide never changes the
verdict on its own: no rubric check reads it, so a voice note is
advice, not a fail. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read and use the components in this order and for these jobs:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. Every check in it is graded, and only
  those checks are graded.
- `references/evidence-guide.md` is the map: for each evidence family
  (plan fidelity, test evidence, diff quality, standards and comms) it
  says where the evidence lives in an eval bundle and in live mode,
  and what good looks like.
- `procedure.md` is the operating procedure. Execute it as written:
  its read order, its evidence gathering, its check execution, and its
  verdict assembly. Where the procedure is silent on a step you need,
  say so in the summary as a procedure gap and take the narrowest
  reading of the rubric; never invent a new check or step to get
  around the gap.

If `rubric.md` has no filled-in checks or no verdict rule, or
`procedure.md` has no filled-in steps, stop and say that the tool
cannot grade without a rubric and a procedure. Do not invent checks at
runtime.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; reservations go in the
check evidence lines. Before the JSON you may print a short readable
summary: one line per check (name, grade, the deciding fact), the
deciding check for a reject, and, in live mode, the voice-guide notes.
Then end the reply with the fenced JSON block below, filled in, valid,
and last, with nothing after it. Use the PR URL (live mode, or the
issue URL if the PR is not open yet) or the bundle id (eval mode) as
`item`, and one entry per rubric check, in rubric order, using the
rubric's check names.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact, quote,
  file name, or diff line that decided it. "Looks fine" is not
  evidence.
- Grade the thing, not the polish: read the diff against the plan,
  the evidence against the test plan, and the description against the
  diff. A terse complete PR can be ready, and a confident, beautifully
  formatted one can be hiding drift. Never pass or fail a check on
  length, tone, or formatting.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Honest shortfalls are not failures: a PR that leaves something out
  and says so in the plan's deviation or deferral notes, or in its
  description, can be ready. Only an unannounced gap fails.
- Unclear: treat `unclear` as the rubric's verdict rule directs. If
  the rule is silent, `unclear` on a required check counts as a fail,
  because a claim you cannot verify from the package is not ready to
  submit.
