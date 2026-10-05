# Rubric: is this plan ready to post and build from?

A plan is ready when it explains the bug the repro actually shows, fixes
that one thing, could be started by a stranger, proves the fix with the
repro itself, and its comment fits the thread and the repo's rules.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (Diagnosis / Cause line) read against what the repro evidence's steps, controls, and Actual line show, and against any cause a maintainer, member, or collaborator states in the thread highlights | Pass if the stated cause explains every behavior the repro pins down, including each control run (why the control behaves differently), and does not contradict the repro's own isolation or a cause a maintainer/collaborator gave in the thread. Fail if the cause blames a component the repro or the thread rules out, calls the reproduced trigger a "red herring", or names a place where the damage cannot happen given the evidence (e.g. the repro shows the data is already wrong before the step the plan blames). | required |
| fixes-cause-not-symptom | The plan's Change / Approach read against the cause the repro evidence and the thread point at | Pass if the change acts on the mechanism that produces the bug (the site the evidence or a maintainer points at). Fail if the plan leaves that mechanism untouched and only documents a workaround, masks the result downstream (catch/recover, re-pad, retry), or patches a different layer than the one the evidence implicates, when the evidence points at a fixable cause. | required |
| scope-bounded | The plan's scope statement (In scope / Not in scope / Change lines) and the full list of things the Approach says it will do, read against what the issue and repro require | Pass if everything the plan commits to doing serves fixing this issue's reproduced behavior (one fix plus its regression tests; touching sibling sites of the same defect and stated deferrals are fine), and anything bigger is named as out of scope or deferred. Fail if the plan commits to extra work the repro does not need: rewrites, redesigns, new options/settings, migrations, dependency or architecture changes, module splits, "fix the whole class", or scanning/fixing other components. | required |
| executable-by-stranger | The plan's Files, Approach, and Change lines | Pass if a stranger could start the work without asking the author: it names where the change goes (a file, or a specific code path such as "the reattach handshake in the client attach path") and says concretely what the change is (what will be different in the code afterward). An exact function still to be pinned is fine when the plan names the code path and how it will pin it, and a named site the plan honestly flags as possibly one layer off still passes. Fail if the work is described as activities instead of changes ("investigate", "explore", "look at", "optimize whatever shows up hot", "track down what changed", "sort out the handling"), or names no code location at all. | required |
| test-plan-reruns-repro | The plan's Test plan read against the repro evidence's steps, artifacts, and controls | Pass if the test plan re-runs the repro (its command, script, or scenario) and states the observable after-result that differs from the repro's Actual (the output, value, exit code, or behavior expected after the fix). Fail if success is only "it works", "tests pass", "faster", "behaves", or a check that never touches the reproduced behavior. | required |
| comment-fits-thread-and-policy | The candidate plan comment read against the repo-facts block's contribution policy (especially any AI-use/disclosure rule) and against the thread highlights (maintainer directions, open PRs, patches, rejected approaches) | Pass if the comment (a) meets every requirement the contribution policy places on comments, e.g. an AI-use disclosure stating tool/extent when the policy requires disclosure of all AI use or of AI-assisted comments; (b) does not ignore or contradict an explicit maintainer/collaborator direction, rejected approach, or existing open PR/patch shown in the thread (acknowledging it and explaining the relationship is enough); and (c) is the author's own plan, not a piggyback ("same as above"). A repo with no AI policy needs no disclosure. Fail if any of (a), (b), (c) is broken. | required |
| comment-matches-plan | The candidate plan comment read against the candidate plan | Pass if the comment's description of the cause, change, and scope says the same thing as the plan and promises nothing the plan does not (no guaranteed timelines, no extra work). | preferred |
| risks-and-unknowns-honest | The plan's Risk / unknowns lines and any certainty language ("root cause", "simply", "red herring"), read against the repro and thread | Pass if the plan names at least one real risk or unknown, or states that none were found, and does not present as settled something the package shows is unsettled. | preferred |

## Verdict rule

Grade every check `pass`, `fail`, or `unclear` (`?`). `unclear` means
the evidence the check names is genuinely missing from the package
(for example, no test plan at all), not that the grader is unsure.

- **accept** (ready) only if every `required` check is `pass`.
- **reject** (hold) if any `required` check is `fail` **or** `unclear`:
  a required check that cannot be verified from the package counts as
  a fail, because a plan nobody can verify is not ready to build from.
- `preferred` checks never change the verdict, whether pass, fail, or
  unclear; report them as notes only.
- Ties do not exist: the verdict is binary, and the first required
  failure (in table order) is the one quoted as the deciding check.
