# Evidence guide: where evidence lives in a PR package

Written next to calib-01.md: each heading says where the evidence
lives in a practice submission (eval bundle), where it lives in my own
PR (live mode), and what good looks like.

## Plan fidelity (harness category: silent-drift)

Read by `plan-scope`, `plan-complete`, and `claims-match-diff`.

- Where it lives, eval bundle: the plan is under "Plan context":
  its "Scope" sentence, "Not in scope" sentence, "Files:" line, any
  deferral or "known limit" note, and the deviation note if the plan
  records one. In calib-01 the boundary is "Not in scope: any other
  theme. Files: `themes.gitconfig`." What actually changed is under
  "Candidate PR": the "Diff" section (each file starts with `--- a/`
  and `+++ b/` lines) and the "Commits" list. The fidelity claims are
  in the "Title" and "Description".
- Where it lives, my PR: `plan.md` (its "Scope", "Files to touch",
  "Approach", and `## Deviations` sections); the branch's diff from
  `git diff main...HEAD` and its file list from `git diff
  main...HEAD --stat`; the commit list from `git log main..HEAD
  --oneline`; the claims in `pr_draft.md` (Summary, Changes, Notes
  for Reviewers).
- What good looks like: every changed file falls inside the plan's
  Files line (file, folder, or a phrase like "tests" or "a changelog
  entry"), a deviation note, or an entry the repo's stated asks
  require; no hunk does work the plan does not describe or puts out of
  scope; every deliverable the plan names is in the diff or its
  absence is noted in the plan or description; and every claim in the
  description ("exactly as planned", "CHANGELOG added", "tests cover
  X") has a matching file or hunk. calib-01 is the good case: one
  file, one removed line, exactly the plan's step 1.
- Silent drift in each direction: MORE than the plan (an extra file
  such as calib-03's `property.js`, a refactor commit, a new option or
  flag) with no deviation note; LESS than the plan (a promised test,
  doc, or second fix missing) with no note; or a description that
  claims more than the diff delivers (a docs update or changelog entry
  that is not in the diff).

## Test evidence (harness category: not-tested)

Read by `repro-rerun`, `repo-checks`, and `pasted-output`.

- Where it lives, eval bundle: the plan's "Test plan:" sentence names
  the repro(s) to re-run, the expected-after, any control, and the
  suite; the "Repro evidence" paragraph at the top of "Plan context"
  gives the before. The proof is the candidate PR's "Test evidence"
  section (and any before/after block in the "Description"). In
  calib-01 the test plan is "re-run the configparser repro, expect a
  clean parse", and the evidence re-runs that exact command with
  `parsed OK` printed after.
- Where it lives, my PR: the "Test plan" section of `plan.md` and my
  Unit 2 repro (the before); `test_evidence.md` next to `plan.md`
  (the before on `main`, the after on my branch, and the output of
  `make test-unit`, `make test-integration`, `make lint`, and
  `make typecheck`); and the Testing section of `pr_draft.md`, where
  that output is pasted.
- What good looks like: every repro the test plan names is re-run on
  the branch with the same input as the plan's repro, and the after
  is a concrete observed result (printed output, an exit code, a
  specific message or value) that matches the plan's expected-after;
  the suite or checks the plan or the repo name are reported as run,
  by name, with their outcome, including an honestly reported failure
  and its reason. Best of all, each is pasted command output rather
  than a note after `#`.
- Not tested looks like: "tested locally, works now", "verified on my
  machine for a day", "all tests pass" with no command; evidence that
  runs only the control or a different input than the repro (the path
  that never broke); or a test plan with two failure modes where only
  one is re-run and nothing says why.

## Diff quality (harness category: unreviewable)

Read by `clean-diff`.

- Where it lives, eval bundle: the candidate PR's "Diff" section
  (added `+` and removed `-` lines in each hunk) and its "Commits"
  list. Live: `git diff main...HEAD` on my branch and
  `git log main..HEAD --oneline`.
- What good looks like: every added or removed line is part of the
  fix, its test, or an entry the repo asks for; comments that explain
  the new code are fine. calib-01's diff is one removed line.
- Debris looks like: debug prints or log lines left from debugging
  (`eprintln!("DEBUG ...")`, `print(...)`), commented-out code or a
  commented-out first attempt, a dead or unused helper (often behind
  `allow(dead_code)` or named `_unused...`), new TODO / FIXME notes,
  and churn: re-indented or reformatted blocks, import reordering, or
  unchanged lines removed and re-added. Commit names like "wip" or
  "misc cleanups while debugging" point to where to look, but the
  diff decides.

## Standards and comms (harness category: standards-wall)

Read by `template-asks` and `ai-disclosure`.

- Where it lives, eval bundle: the "Repo facts" block's
  "pull requests:" line (the template's sections and checklist, the
  issue reference, title format, required changelog or whatsnew
  entries) and "contribution policy:" line (including any AI policy),
  plus maintainer directions in "Thread highlights". The PR honors
  them in its "Title", "Description", and "Diff" (for required
  entries). calib-01's repo has no template and asks only for small
  PRs that reference the issue, which the description does with
  "(#2211)".
- Where it lives, my PR: `.github/PULL_REQUEST_TEMPLATE.md` (Summary,
  Issue with `Closes #`, Changes, Testing checklist, Screenshots /
  Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md` (branch name
  `<type>/<issue-number>-<short-description>`, conventional commits,
  "Working on a seeded bug: remove its xfail marker"), the course's
  rule that every PR discloses AI use in Notes for Reviewers, and my
  `pr_draft.md`.
- What good looks like: every template section and checklist item the
  repo asks for is present with real content (not a placeholder), the
  issue reference is there, entries the repo requires are in the diff,
  and when the policy requires AI disclosure the description names
  the tool or says an AI assistant was used, and to what extent. Asks
  that a PR cannot show (CLA, vouch flow) are not graded. In live
  mode, the CI box stays unticked until CI has run, and that is
  correct.
- A standards wall looks like: a required checklist or template
  section missing entirely, no "closes #" when the template asks for
  it, a required whatsnew / changelog entry absent from the diff, or
  no AI disclosure where the policy requires disclosing all AI use.
  (Whether the description's claims match the diff is plan fidelity,
  above.)
