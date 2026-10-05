# Rubric: is this plan ready to post and build from?

The first three checks are my group's "Our rubric" from the activity
worksheet, worded as closely to the group's text as a table allows.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis | The plan's Diagnosis / Cause line | Passes if the plan says what causes the bug. | required |
| scope | Look where the plan says what it will change (its Scope / Change / Files lines) | Passes if it names each file it will change and says what it won't touch. | required |
| test | The plan's Test plan | Passes if the plan adds an automated test. | required |
| comms *(addition beyond the group rubric)* | The candidate plan comment read against the repo-facts contribution policy (any AI-use / disclosure rule) and the thread highlights (maintainer directions, rejected approaches, open PRs or patches) | Passes if the comment meets every requirement the contribution policy places on comments (e.g. states the AI tool and extent of use when the policy requires disclosure of all AI use in any form, or of AI-assisted issues/comments; a disclosure rule that applies only to pull requests or code does not apply to a plan comment, and with no AI policy no disclosure is needed), and does not ignore an explicit maintainer/collaborator direction, rejected approach, or open PR/patch in the thread (acknowledging it is enough). | required |

Why the addition: with only the group's three checks (full run 3), the
eval scored 18/20 with zero margin, and the thread-convention category
matched only by accident: pkg-04 was rejected for its manual test, not
for its comment, and pkg-20, whose comment skips the repo's mandatory
AI-use disclosure, was graded accept. None of the group's checks reads
the plan comment, so `comms` is the one check added.

## Verdict rule

Ready (accept) if every required check passes; otherwise hold (reject).
An unclear grade (`?`) on a required check counts as a fail, because a
check that cannot be verified from the package has not passed.
