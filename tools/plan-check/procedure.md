# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md` first and confirm the issue's
   `owner/repo` matches the `Repo:` line; stop and refuse if it does
   not, or if the line is still a placeholder. Note the house rules
   (own plan comment even on a shared issue, no piggybacking, branch
   on own fork). Eval mode: skip this step.
2. Read `rubric.md` and list every check name, its weight, and the
   verdict rule, so you know which facts you will need before reading
   the package.
3. Read the repo-facts block (or, live, CONTRIBUTING / AI_POLICY) and
   write down the contribution policy's rules for comments, especially
   any AI-disclosure requirement. This is read first because it decides
   the comms check and is easy to miss later.
4. Read the issue body, then every thread highlight. Write down each
   maintainer, member, or collaborator statement of cause, chosen
   direction, rejected approach, and every open PR or patch mentioned.
5. Read the repro evidence before the plan. Write down: the failing
   command or script, the Actual result (output, value, exit code), and
   what each control run shows and therefore rules in or out. The repro
   comes before the plan so the plan is judged against the evidence,
   not the other way round.
6. Read the candidate plan, then the candidate plan comment, last.

## Evidence gathering

1. For `diagnosis-grounded`: quote the plan's cause sentence; next to
   it, list each repro step/control and each thread cause from Read
   order steps 4-5. Mark each as explained, contradicted, or ignored.
2. For `fixes-cause-not-symptom`: quote the plan's change/approach
   lines and the code site the evidence or a maintainer implicates.
   Record whether the change touches that site.
3. For `scope-bounded`: list every action the plan commits to (each
   numbered approach step, each "also", "plus", "along the way") and
   every in-scope / not-in-scope line. Mark each action as needed for
   this repro or extra.
4. For `executable-by-stranger`: list the files and the function or
   code site named, and the verbs used for the work (change verbs like
   "clamp", "add", "replace" vs. activity verbs like "investigate",
   "explore", "look at").
5. For `test-plan-reruns-repro`: quote the test plan; record whether it
   names the repro command/scenario and an expected after-result that
   differs from the repro's Actual.
6. For `comment-fits-thread-and-policy`: quote the policy line from Read
   order step 3, the thread directions/PRs from step 4, and the
   comment's sentences that disclose AI use or acknowledge those
   directions/PRs. Record any requirement with no matching sentence.
7. For the preferred checks: quote the comment's claims next to the
   plan's, and the plan's risk/unknown lines and any certainty words.
8. In eval mode use only the bundle text; in live mode take the
   issue-side facts from GitHub (issue, thread, CONTRIBUTING, the
   student's posted repro comment) and the candidate side only from the
   drafts.

## Check execution

1. Run the checks in rubric table order, one at a time.
2. For each check, apply its pass condition word for word to the
   evidence you recorded for it in Evidence gathering; do not re-read
   the whole package unless that record is missing a fact the
   condition asks about.
3. Grade `pass` if the condition is met, `fail` if it is broken, and
   `unclear` only if the evidence the check names is absent from the
   package (e.g. no test plan section and no test sentence anywhere).
4. Write one evidence line per check: the quote or fact that decided
   it. A grade without a quote is not allowed.
5. Grade each check independently: a failed diagnosis does not
   automatically fail the test plan, and a strong plan does not excuse
   a comment that breaks the repo's policy.
6. Judge the thing, not the format: a terse plan with the needed facts
   passes; headings, length, and confidence earn nothing.

## Verdict assembly

1. Collect the grades of the `required` checks only.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is
   `reject`; unclear counts as fail, per the rubric's verdict rule.
4. Ignore preferred checks for the verdict; list them as notes.
5. In the summary, name the first failing required check (table order)
   as the deciding check and quote its evidence line.
6. Live mode: after the verdict, hold the comment against
   `voice-guide.md` and list any rule it breaks, quoting the rule; this
   never changes the verdict.
7. End with the fenced JSON block (every check, its grade, its one-line
   evidence, and the verdict) and nothing after it.
