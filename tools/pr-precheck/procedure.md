# Procedure: how this tool grades a PR package

Rewritten from my Unit 3 procedure for a PR. The four stages are the
same; the reads that matter now are side by side: the diff against
the plan, the evidence against the test plan, and the description
against the diff.

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL's
   `owner/repo` matches the `Repo:` line. If it does not, or the line
   is still a placeholder, stop and refuse as SKILL.md says. Eval
   mode: skip this step.
2. Read `rubric.md`: list the nine checks, their weights, and the
   verdict rule.
3. Read the repo's stated asks before anything the contributor wrote:
   the repo-facts block's PR template and contribution / AI-policy
   lines (live: `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md`). Note each visible ask (sections,
   checklist, issue reference, title format, required entries such as
   a changelog) and whether AI disclosure is required.
4. Read the issue and the thread highlights (live: `gh issue view
   <n> --repo <owner/repo> --comments`). Note any maintainer
   direction the PR has to respect.
5. Read the plan before the PR: its repro evidence, diagnosis, Scope /
   Not in scope, Files line, test plan (each repro or failure mode and
   its expected-after, each control, each suite), and its deviation or
   deferral notes (live: `plan.md` including `## Deviations`). Write
   down the plan's file list, its deliverables, and its test-plan
   items. The plan comes first because every later read compares
   something against it; reading the description first lets its
   claims frame the diff.
6. Read the diff and the commit list next (live: `git diff
   main...HEAD`, `git diff main...HEAD --stat`, `git log
   main..HEAD --oneline`). Read the diff before the description so
   you see what the change does before you hear what it claims.
7. Read the title and description last (live: `pr_draft.md`, first
   line = title), then the test evidence (live: `test_evidence.md`).

## Evidence gathering

Record each item below before grading anything. Each one is a pairing
of two things side by side.

1. Diff files against plan scope (for `plan-scope`): list every file
   in the diff. Next to each, write the plan phrase that names it
   (file, folder, "tests", "a changelog entry"), the deviation note
   that records it, or the repo ask that requires it, or "none". Then
   list every hunk that does something the plan does not describe or
   puts out of scope (renames, refactors, new options / flags /
   settings, unrelated rewrites), with its file and first changed
   line.
2. Plan deliverables against the diff (for `plan-complete`): list
   each deliverable the plan names (the code change, each named test
   case, a docs or changelog update). Next to each, write the diff
   file or hunk that delivers it, or the deviation / deferral note or
   description sentence that discloses its absence, or "missing".
3. Description claims against the diff (for `claims-match-diff`):
   quote each factual claim in the title and description (fidelity
   claims, "X added", "tests cover Y", "docs updated", "no Z added").
   Next to each, write the diff line or file that proves it, or
   "no matching diff content". A fidelity claim is proved only if
   item 1 found no unplanned file or hunk.
4. Test evidence against the test plan (for `repro-rerun`): for each
   failing case the plan's repro evidence shows and its test plan
   says to re-run (each repro or failure mode), copy the command
   or steps the evidence re-runs on the branch and the after result it
   shows. Mark whether the input is the same as the plan's repro (not
   only the control, not a different path), and whether the after is
   a concrete observed outcome (output, exit code, message, value, or
   visible behavior) matching the plan's expected-after. Mark any
   test-plan repro that has no re-run, and any note that discloses
   the gap. List controls and extra scenarios separately: a one-line
   result naming the outcome is enough for those.
5. Repo checks against the stated asks (for `repo-checks`): list each
   suite or check the plan's test plan or the repo's contribution
   docs / PR template name. Next to each, copy the evidence line that
   reports it run (command or suite name plus outcome), or "not
   reported". Live mode for Path Review: the template's checks are
   `make test-unit`, `make test-integration`, `make lint`, and
   `make typecheck`.
6. Output form (for `pasted-output`): for the before, the after, any
   control, and the suite results, note whether each is pasted
   command output or only a `#` note or a sentence.
7. Debris (for `clean-diff`): scan every added and removed line and
   list any debug print or log, commented-out code, dead or unused
   function or variable, new TODO / FIXME, and any hunk or line pair
   whose only change is whitespace, formatting, re-indentation, import
   order, or identical lines removed and re-added, each with its file.
   A removed-and-re-added line where part of the line changed is the
   fix; do not list it.
8. Stated asks against the PR (for `template-asks`): for each visible
   ask noted in Read order step 3, write where the title, description,
   or diff meets it, or "unmet". Skip asks that cannot be seen in a PR
   (CLA, vouch, prior discussion).
9. Disclosure (for `ai-disclosure`): quote the policy line on AI use
   and the description's sentence that discloses AI use, or "none".
10. Eval mode: take every fact from the bundle text only. Live mode:
    take issue-side facts (thread, template, CONTRIBUTING) from the
    repo, and candidate-side facts only from `plan.md`, the branch
    diff, `pr_draft.md`, and `test_evidence.md`.

## Check execution

1. Run the checks in rubric table order: `plan-scope`,
   `plan-complete`, `claims-match-diff`, `repro-rerun`,
   `repo-checks`, `pasted-output`, `clean-diff`, `template-asks`,
   `ai-disclosure`.
2. For each check, apply its pass condition word for word to the
   evidence recorded for it in Evidence gathering. Re-read the
   package only when that record is missing a fact the condition
   needs.
3. Grade `pass` or `fail` when the recorded evidence decides it.
   Grade `unclear` only when the evidence the check names is
   genuinely absent from the package (for example, no test evidence
   section at all for `repo-checks` to read); never use `unclear` as
   a soft fail.
4. Write one evidence line per check: the file, diff line, or quote
   that decided it (for a fail, the first offending item).
5. Grade each check on its own. Do not fail a later check because an
   earlier one failed, and do not pass a check because the PR is
   strong elsewhere. A disclosed shortfall (deviation note, deferral
   note, or a plain statement in the description) passes the check
   it would otherwise fail.

## Verdict assembly

1. Apply the rubric's verdict rule: if every required check is
   `pass`, the verdict is `accept`.
2. If any required check is `fail` or `unclear`, the verdict is
   `reject`; `unclear` counts as a fail.
3. `pasted-output` is preferred: report its grade, but never let it
   change the verdict.
4. For a reject, name the deciding check: the first failing or
   unclear required check in rubric table order. Quote its evidence
   line in the summary.
5. Live mode: hold the title and description against
   `voice-guide.md` and list any broken rule; this never changes the
   verdict.
6. End with the fenced JSON block from SKILL.md, one entry per check
   in rubric order, and nothing after it.
