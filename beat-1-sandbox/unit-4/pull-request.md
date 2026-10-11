# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/136

**Branch**

`fix/64-partial-overlap-fixture`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Every run, in order (scores are the harness's agreement lines):

1. Run 1, full run with `--save-run eval-run.txt`, first version of all four files: **incomplete, 9 of 20 graded, 7/9 agreed.** The other 11 packages errored with `claude exited 1` because the API key ran out of credit ("Credit balance is too low"). The harness printed `categories: clear-accept 0/7  not-tested 3/4  silent-drift 3/4  standards-wall 1/2  unreviewable 0/3` and `agreement: 7/9 scored items`. Because it was partial, it did not write `eval-run.txt`. The two disagreements were pkg-02 (gold accept, graded reject: `clean-diff`, plus the preferred `pasted-output`) and pkg-05 (gold accept, graded reject: `repro-rerun`, plus `pasted-output`). I then loosened `repro-rerun` and `clean-diff` (see Check rationale).
2. Run 2, `--only pkg-05` with a new API key, after those two edits, to confirm the key worked: **1/1** (`categories: clear-accept 1/1`, pkg-05 now accept).
3. Run 3, full run with `--save-run eval-run.txt`, same files as run 2: **20/20** (`categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, `agreement: 20/20 scored items  (bar: 18/20: PASS)`). This is the run in `eval-run.txt`.

There were two full runs: run 1, which stopped partway, and run 3. After run 3 I also ran calibration canaries with the same files, which are never scored and have no agreement score: `--only calib-03,calib-04 --include-calibration` gave calib-03 reject (agrees) and calib-04 reject (agrees), and `eval-run.txt` was not touched.

Final score, the last in the list above: **20/20**, matching the agreement line in `eval-run.txt`.

**Package analysis**

**pkg-02** (Textualize/rich#4208, category clear-accept). Gold label: **accept**. My rubric's verdict: **reject** in run 1 and **accept** in run 3 (the run in `eval-run.txt`).

In run 1 the deciding check was `clean-diff`, graded fail with the evidence "save_html/save_svg hunks re-print unchanged lines 'html = self.export_html(' and 'theme=theme,' as removed+re-added when only clear=clear→clear=False changed." That reading was wrong for this PR. Those lines are in the `export_html(...)` / `export_svg(...)` calls whose `clear=clear` argument the fix changes to `clear=False`, so the diff shows them as removed and re-added because the fix edits that call. That's the fix, not churn. My first `clean-diff` wording banned "re-printed-unchanged-line churn outside the lines the fix needs", and the grader counted the call's other lines as unneeded. Everything else about pkg-02 was a clear accept: the diff touches only the three files the plan names (`rich/console.py`, `tests/test_console.py`, and `CHANGELOG.md`, "the repo's stated entry ask"), the repro is re-run with `'IMPORTANT OUTPUT\n'` printed after the failed save, and the template's "Type of changes: Bug fix." and CHANGELOG asks are met. After I rewrote churn as "a hunk or line pair whose only change is whitespace, formatting, re-indentation, import ordering, or identical lines removed and re-added", run 3 graded `clean-diff` pass ("No debug prints, dead code, TODOs, or pure whitespace/churn hunks found in the diff") and the verdict was accept. The preferred `pasted-output` check still fails on pkg-02, because the pytest result is a sentence ("`pytest tests/ -q` passes (1462 passed, 12 skipped)"), but a preferred check never changes the verdict.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md` as it reads now, the `clean-diff` row's pass condition (weight: required):

> Passes if the diff carries no debris: no debug prints or logging left from debugging, no commented-out code or commented-out earlier attempts, no dead or unused functions or variables added, no new TODO / FIXME notes, and no churn: a hunk or line pair whose only change is whitespace, formatting, re-indentation, import ordering, or identical lines removed and re-added, in code the fix does not otherwise change. A line removed and re-added because part of it changed (an argument, a value), or neighbouring lines a diff shows as context-changed while the fix edits them, is the fix, not churn. Comments that explain the new code are not debris. Commit messages alone never fail this check; the diff decides.

Why it reads this way: my first version said only "no formatting, re-indent, import-reordering, or re-printed-unchanged-line churn outside the lines the fix needs". In run 1 that failed pkg-02, a gold-label accept, because the grader treated the lines of an `export_html(...)` call whose `clear=` argument the fix changes as "re-printed unchanged" churn. I kept the debris list from the activity and from the unreviewable packages (pkg-12's commented-out debug line and dead `_unused_buffer_probe`, pkg-15's DEBUG `eprintln` and dead `count_open_fds`, pkg-18's import restructure and identical re-printed lines). I also defined churn by what it is, a change whose only effect is whitespace, formatting, ordering, or identical lines, and added the sentence saying that a line removed and re-added because part of it changed "is the fix, not churn". I chose that over dropping churn from the check, because pkg-18's only real debris besides one commented-out `eprintln` is churn, and the check still has to catch it. "Commit messages alone never fail this check; the diff decides" is there so that pkg-12's "wip" commits don't stand in for reading the diff.

**Trade-offs**

Loosening `clean-diff` and `repro-rerun` after run 1 gave something up on both:

- `clean-diff` now passes any removed-and-re-added line where part of the line changed. A PR that slips an unrelated edit into a line it touches anyway (for example, renaming a variable on the same line as the real fix) now passes this check, and only `plan-scope` can catch it as unplanned work. The unreviewable packages still all matched in run 3 (`unreviewable 3/3`: pkg-12, pkg-15, pkg-18), so the loosening didn't flip them.
- `repro-rerun` now lets controls and extra scenarios ("cases that were not failing in the repro") be reported in one sentence, which is what flipped pkg-05 (its same-key warning scenario is stated, not pasted). The risk is calib-04, where the second failure mode (`-j 200000 -x echo .`) could be read as an "extra scenario". After run 3, I re-ran calib-04 and calib-03 as canaries with `--only calib-03,calib-04 --include-calibration`. Both stayed reject, and calib-04 still fails `repro-rerun` because "repro 2 (-j 200000 -x echo .), named by the test plan, is never re-run or mentioned". The not-tested category held at 4/4 in run 3 (pkg-04, pkg-07, pkg-10, pkg-14).
- A case I accept the rubric will miss: a PR whose after is only a confident `#` note with a specific-sounding value (like pkg-20's "after: ESC[?997;1n (dark)") passes `repro-rerun` even though no command output is pasted. Only the preferred `pasted-output` check flags it, and that never changes the verdict. I made that check preferred on purpose: the gold labels accept pkg-11 and pkg-13, whose afters are notes like that.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
