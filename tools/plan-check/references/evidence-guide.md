# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: eval, the candidate plan's "Diagnosis" or "Cause"
  line. Live, the diagnosis section of the draft plan.md.
- What good looks like: the plan states a cause (a mechanism or code
  site that produces the bug), not just a restated symptom.

## Scope

- Where it lives: the plan's "Scope", "In scope / Not in scope",
  "Change", and "Files" lines.
- What good looks like: every file the change will touch is named, and
  there is a line saying what the plan will not touch.

## Executability

- Where it lives: the plan's "Files" list and "Approach" steps.
- What good looks like: the named files and steps are concrete enough
  to start from.

## Test plan

- Where it lives: the plan's "Test plan" (or "Test:") line.
- What good looks like: the plan adds an automated test (a new or fixed
  unit, integration, or regression test that runs in the suite).

## Honesty

- Where it lives: the plan's "Risk" / "Unknowns" lines and, after a
  build, its "## Deviations" section.
- What good looks like: risks and unknowns are named; deviations are
  recorded in the plan, not only in the diff.

## Comms

- Where it lives: the "Candidate plan comment", read against the
  "Repo facts" contribution policy and the "Thread highlights". Live,
  comment.md against CONTRIBUTING / AI_POLICY and the live thread.
- What good looks like (read by the `comms` check, an addition beyond
  the group rubric): if the policy requires AI disclosure (all AI use,
  in any form, or AI-assisted issues/comments), the comment names the
  tool and the extent. A rule that asks for disclosure only in the pull
  request, or that says no disclosure is asked for issue comments, does
  not apply to a plan comment; with no AI policy, none is needed. "Own
  words" rules pass unless the comment is plainly a template. The comment names any
  maintainer direction, rejected approach, or open PR in the thread and
  says how the plan relates to it (following it, deferring to it, adding
  tests to it). It is the author's own plan, not "same as above".
