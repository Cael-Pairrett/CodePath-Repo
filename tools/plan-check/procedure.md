# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md` first and confirm the issue's
   `owner/repo` matches the `Repo:` line; stop and refuse if it does
   not, or if the line is still a placeholder. Eval mode: skip this.
2. Read `rubric.md`: list each check, its weight, and the verdict rule.
3. Read the repo-facts block (contribution policy, AI rules), then the
   issue body and thread highlights, then the repro evidence,
   so you know what bug the plan is about before reading the plan.
4. Read the candidate plan, then the candidate plan comment.

## Evidence gathering

1. For `diagnosis`: quote the plan's sentence that says what causes the
   bug (its Diagnosis / Cause line).
2. For `scope`: from the plan's Scope / Change / Files lines, list every
   file the plan says it will change, and quote the line that says what
   it won't touch (Not in scope / Out).
3. For `test`: quote the plan's Test plan and note any automated test it
   adds (a new or fixed unit, integration, or regression test that runs
   in the test suite), as opposed to manual-only steps.
4. For `comms` (addition): quote the repo-facts contribution-policy
   line (or, live, CONTRIBUTING / AI_POLICY), list each maintainer or
   collaborator direction, rejected approach, and open PR/patch in the
   thread highlights, and quote the comment's sentences that disclose
   AI use or acknowledge those items. Record any requirement with no
   matching sentence.
5. In eval mode use only the bundle text; in live mode take issue-side
   facts from GitHub and the candidate side only from the drafts.

## Check execution

1. Run the checks in rubric table order.
2. Apply each pass condition word for word to the evidence recorded for
   it; re-read the package only if that record is missing a fact.
3. Grade `pass`, `fail`, or `unclear` (`unclear` only when the evidence
   is absent from the package), with a one-line quote or fact.
4. Grade each check independently and judge the content, not the
   formatting.

## Verdict assembly

1. If every required check is `pass`, the verdict is `accept`.
2. If any required check is `fail` or `unclear`, the verdict is
   `reject`: unclear counts as a fail.
3. Name the first failing required check (table order) as the deciding
   check and quote its evidence.
4. Live mode: hold the comment against `voice-guide.md` and list any
   broken rule; this never changes the verdict.
5. End with the fenced JSON block and nothing after it.
