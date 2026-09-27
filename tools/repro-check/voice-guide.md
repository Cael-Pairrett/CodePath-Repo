# Voice guide: how I talk upstream

## Who I am in threads

I am Cael Pairrett, a student engineer learning the codebase by
reproducing and claiming real issues. Readers can expect short,
specific comments: what I ran, what I saw, and what I will do next —
no filler, no hype, no promises I have not earned with evidence.

## Rules I write by

### Rule: lead with the observation

State the concrete result before the biography. Maintainers skim for
whether the bug is real on another machine; give them that first.

- Wrong: "Hi! I'm a student really excited to contribute to this
  amazing project and I would love to take this issue as my first PR
  if that is okay with everyone."
- Right: "Reproduced the missing `Content-Type` on 3.2.4 with one
  custom header (`--offline` output below). Next I will check
  `apply_missing_repeated_headers()` against the multidict versions
  in the thread."

### Rule: match the claim to the artifact

Never say "crash", "confirmed", or "root cause" unless the paste shows
it. If I could not reproduce, say so and name what differed.

- Wrong: "bat aborts with a crash exactly as described" (when the
  paste is an arg-validation error and exit 1).
- Right: "On `:-N` I get the capacity-overflow panic (exit 101) shown
  below." Or, if it did not trigger: "Could not reproduce on Linux +
  zsh with the report's layout; prompt still renders. Differs from the
  report's macOS + fish — next I will try that pairing."

### Rule: one modest next step

End with a single concrete action I can actually do this week. Do not
reserve the issue or guarantee a fix timeline.

- Wrong: "Kindly assign this to me, I will fix it within 2 days
  guaranteed. Please keep this issue reserved for me."
- Right: "Next step: compare the single-theme vs conditional-theme
  path in `Config.changeConditionalState` against the draft patch in
  the issue, then report back before opening a PR."

### Rule: disclose when the repo asks

If CONTRIBUTING / AI_POLICY requires AI disclosure on comments, say
what I used and that I ran and understand every step. If it asks for
human-voiced comments, write the comment myself in this register — no
generic assign-me templates.

- Wrong: (on a repo that requires disclosure) posting a full report
  with no mention of AI assistance.
- Right: "Per the AI usage policy: I used an AI assistant to help
  organize this report; I ran and verified every step myself and I
  understand what I am reporting."

### Rule: cut the cheerleading

Skip "great project", emoji piles, and "happy to help test whenever".
Useful offer beats enthusiasm.

- Wrong: "Love this project!! +1 also seeing this, any updates on a
  fix?? would love for this to get resolved soon 🙏"
- Right: "Also hit the tab overflow on fzf 0.74.3 / Ubuntu 24.04;
  minimal repro and terminal width notes in the report below."

## Things I never post

- Guaranteed fix timelines ("2 days guaranteed", "fix incoming
  tonight") before I have a failing test or a patch I have run.
- "Assign this to me" / "keep this reserved" language.
- Root-cause diagnoses with no transcript, log, or bisect pasted.
- Narrating a different failure as if it were the issue's failure.
- Undisclosed AI assistance on repos whose policy requires disclosure.
- Piggyback one-liners ("same as above, can confirm") with no env,
  steps, or artifact of my own.
