# Rubric: is this pull request ready to submit?

Where these checks came from: every "no" in my activity worksheet for
calib-03 is a problem one of these checks catches. Files row 2
(`property.js`, a file the plan never names) is `plan-scope`. Claims
row 5 ("Implements the plan exactly, with no changes beyond it", which
the diff contradicts) and rows 8 and 9 (tests the checklist claims but
the diff does not add) are `claims-match-diff`, and the plan's missing
all-comments control test is `plan-complete`. Rows 11 to 13 (before,
after, control, and suite given only as notes or sentences) are split
three ways: `repro-rerun` requires the plan's own repro to be re-run
on the branch with a concrete observed result, `repo-checks` requires
the repo's suite to be reported as run with its result, and
`pasted-output` (preferred) asks for every one of those as pasted
command output. The last three checks cover the two families the
worksheet's tables did not reach: debris (`clean-diff`) and the repo's
stated asks (`template-asks`, `ai-disclosure`).

Failure family for each check: plan fidelity / silent drift =
`plan-scope`, `plan-complete`, `claims-match-diff`; test evidence /
not tested = `repro-rerun`, `repo-checks`, `pasted-output`; diff
quality / unreviewable = `clean-diff`; standards and comms / standards
wall = `template-asks`, `ai-disclosure`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-scope | The diff's changed files and hunks, read against the plan's Files line, its Scope / Not in scope lines, and any deviation note in the plan | Passes if every changed file is named by the plan (the file itself or its folder, or a phrase such as "tests" or "a changelog entry" that names it), is recorded in a deviation note, or is an entry the repo's stated asks require (a changelog / whatsnew entry, a test); AND no hunk does work the plan does not describe or explicitly scopes out (a rename or refactor pass, a new option, flag, or setting, an unrelated rewrite), even inside a planned file. A file or hunk that fails this is not saved by being mentioned in the description unless the plan's deviation notes record it. | required |
| plan-complete | Each deliverable the plan names (the code change, each named test or test case, a docs or changelog update it promises), read against the diff, with the plan's deviation / deferral notes and the description | Passes if every deliverable the plan names appears in the diff, or its absence is recorded in the plan's deviation or deferral notes or stated plainly in the PR description. A deliverable that is missing with no note anywhere fails. | required |
| claims-match-diff | Every factual claim in the title and description about the change ("implements the plan exactly", "no other changes", "CHANGELOG entry added", "tests cover X, Y, Z", "docs now document ...", "no options added"), read against the diff and commit list | Passes if every such claim is true of the diff: each file, entry, test, or doc the description says was added is in the diff, and any fidelity claim ("exactly as planned", "no changes beyond the plan", "no functional changes outside the fix") holds because the diff contains nothing outside the plan. A claim with no matching diff content fails. A claim that something was NOT added passes when nothing in the diff adds it. | required |
| repro-rerun | The test evidence (and any before/after in the description), read against the plan's test plan and the reproduction evidence it built on | Passes if every failing case the plan's repro evidence shows and the test plan says to re-run (each repro command or failure mode) is re-run on the branch with the same input or steps as the plan's repro, and its after result is stated as a concrete observed outcome (printed output, an exit code, a specific message, value, or visible behavior) that matches the plan's expected-after. The "before" may come from the plan's repro evidence. Controls and extra scenarios the test plan adds (cases that were not failing in the repro, such as an unchanged control or a new warning path) may be reported in one sentence that names their result. Fails if a repro's after is only a general verdict ("works now", "verified on my machine", "tests pass", "matches expected" with nothing observed), if the evidence runs a different input or path than the repro (only the control, or a different request), or if a failing case the test plan names is not re-run at all and no note discloses the gap. | required |
| repo-checks | The test evidence and description, read against the suite or checks named in the plan's test plan and the repo's contribution docs / PR template | Passes if each suite or check that the plan's test plan or the repo's stated asks name is reported as run on the branch, by its command or suite name, with its outcome (pass, or a failure reported honestly with its reason). Also passes if neither the plan nor the repo names any suite or check. Fails if a named suite or check is never mentioned as run, or if the only statement is an unnamed "all tests pass". | required |
| pasted-output | The test evidence's before/after, control, and suite results | Passes if the before, the after, the control (if the plan names one), and the suite results are each shown as pasted command output rather than a note after `#` or a sentence saying what happened. | preferred |
| clean-diff | The unified diff's added and removed lines, and the commit list | Passes if the diff carries no debris: no debug prints or logging left from debugging, no commented-out code or commented-out earlier attempts, no dead or unused functions or variables added, no new TODO / FIXME notes, and no churn: a hunk or line pair whose only change is whitespace, formatting, re-indentation, import ordering, or identical lines removed and re-added, in code the fix does not otherwise change. A line removed and re-added because part of it changed (an argument, a value), or neighbouring lines a diff shows as context-changed while the fix edits them, is the fix, not churn. Comments that explain the new code are not debris. Commit messages alone never fail this check; the diff decides. | required |
| template-asks | The repo's stated PR template sections, checklist, and contribution asks (repo-facts block; live, `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`), read against the title, description, and diff | Passes if every ask that can be seen in a PR is met with real content: each template section or checklist the repo requires is present and filled, the issue reference the repo asks for ("Closes #N", "fixes #N", or the issue number) is present, a title format the repo asks for is followed, and an entry the repo asks for on this kind of change (a changelog or whatsnew entry, a test) is in the diff. Asks that cannot be seen in a PR (a signed CLA, a vouch flow, a prior discussion, response times) are not graded here. In live mode, an unticked CI box is correct before the PR is opened and never fails this check. With no template and no stated asks, passes. | required |
| ai-disclosure | The repo's stated AI-use policy (repo-facts contribution policy; live, `docs/CONTRIBUTING.md`, any AI policy file, and the course rule that every PR discloses AI use) read against the description | Passes if, when the policy requires AI use to be disclosed in pull requests or in any form, the description states the AI tool (or that an AI assistant was used) and the extent of its use. Treat every eval package as AI-assisted work. When the policy only asks that comments be in the author's own words, a description that is not plainly a template passes. With no stated AI policy, passes. | required |

## Verdict rule

Ready (`accept`) if every required check passes; otherwise hold
(`reject`). A `fail` on any required check means reject. An `unclear`
grade on a required check counts as a fail, because a claim that
cannot be verified from the package has not passed. Use `unclear`
only when the evidence the check names is genuinely absent from the
package, not as a soft fail. The preferred check (`pasted-output`) is
graded and reported but never changes the verdict, whatever its
grade.
