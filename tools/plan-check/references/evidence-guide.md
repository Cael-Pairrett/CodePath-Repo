# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: eval, the candidate plan's "Diagnosis" or "Cause"
  line, checked against the "Repro evidence" block (Steps, Control
  runs, Actual) and against cause statements by MEMBER / OWNER /
  COLLABORATOR / CONTRIBUTOR authors in "Thread highlights". Live, the
  draft plan.md's diagnosis against the student's posted repro comment
  on the issue and the maintainer comments in the thread.
- What good looks like: the stated cause explains the failing run AND
  every control (why the control does not fail), and agrees with, or
  explicitly argues against with evidence, any cause a maintainer gave.
  A bad diagnosis blames a part the controls rule out (e.g. control
  shows the subsystem works, plan says it is missing), calls the
  reproduced trigger a red herring, or blames a step that runs after
  the repro shows the data is already wrong. A plan that fixes a
  different layer than the one the evidence points at (docs for a code
  bug, downstream patch-up for an upstream cause, crash catching for a
  logic bug) is treating the symptom.

## Scope

- Where it lives: the plan's "Scope", "In scope / Not in scope",
  "Change" lines, and every numbered approach step (scope creep hides
  in approach steps and in the comment's "beyond", "also", "while I'm
  there").
- What good looks like: one bounded change to the implicated site plus
  regression tests, with bigger related work named as deferred or out
  of scope. Fixing a sibling site of the same defect is still bounded.
  A drive-by rewrite adds redesigns, state machines, new options or
  settings, dependency migrations, module splits, retries, or scans of
  other components the repro never needed.

## Executability

- Where it lives: the plan's "Files" list, "Approach" steps, and
  "Change" line.
- What good looks like: a named file and function / handler / line
  site, and change verbs that say what will be different afterward
  ("clamp with saturating_sub", "append `--` before the path",
  "normalize `\` to `/` in the candidate"). Not executable: "investigate
  the input stack", "explore caching", "optimize whatever is hot", "sort
  out the handling". A plan that flags its site as possibly one layer
  off is still executable; honesty about the exact line is fine. So is
  a plan that names the code path and the concrete change but leaves the
  exact function to be pinned with a stated method (debug logs, tracing):
  the change is decided, only the line number is not. What fails is a
  plan where the change itself is undecided.

## Test plan

- Where it lives: the plan's "Test plan" (or "Test:") line, read next
  to the repro evidence's Steps, Actual, and Control runs.
- What good looks like: it re-runs the repro's own command / script /
  scenario and names the expected after-result in observable terms
  (the output that prints, the value, exit 0, the behavior), different
  from the repro's Actual, and keeps the controls unchanged. A vague one
  says "confirm it works", "tests pass", "report back when faster", or
  tests something the repro never showed.

## Honesty

- Where it lives: the plan's "Risk" / "Unknowns" / "open question"
  lines, certainty words in the Diagnosis and in the comment ("root
  cause", "simply", "red herring"), and, after a build, the plan's
  "## Deviations" section.
- What good looks like: real risks or unknowns are named (or "none
  found" with a reason), unverified items are labeled as unverified,
  and nothing the thread shows as unsettled is presented as settled.
  An honest mid-build change is written under Deviations with what
  changed and why; a deviation that exists only in the diff is not
  honest.

## Comms

- Where it lives: eval, the "Candidate plan comment" read against the
  "Repo facts" contribution-policy line (AI-use / disclosure rules,
  review-bandwidth notes) and against "Thread highlights" (maintainer
  directions, rejected approaches, open PRs, posted patches). Live,
  comment.md read against CONTRIBUTING.md / AI_POLICY.md and the live
  thread, plus scope.md's house rules.
- What good looks like: if the policy requires AI disclosure (all AI
  use, or AI-assisted comments), the comment says which tool helped and
  how much; if it asks for human-written comments, the comment is in
  the author's own words. The comment names any maintainer direction,
  rejected approach, or open PR in the thread and says how this plan
  relates to it (following it, deferring to it, adding tests to it). It
  is the author's own plan built from their own repro, never "same
  approach as above", and it promises nothing the plan does not.
  Boilerplate ignores all of this ("I'll fix it, PR soon").
