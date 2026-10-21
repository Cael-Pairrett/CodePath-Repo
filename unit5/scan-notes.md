# Scan notes

- 2026-10-21: Scan 1 graded 5 and accepted all 5, but the read-out showed my queries were broken: I passed the search qualifiers as one quoted string after `--`, so gh treated them as plain text and only the flags (label, language, no-assignee, state) applied. All 5 picks were opened that same day, which my `created:<2026-10-01` filter was supposed to block. Fixed search-strategy.md by giving each qualifier its own argument (and `--match title` for csv/json/pandas). Checked by re-running both queries in the terminal: the top results are now all created before 2026-10-01.
- 2026-10-21: 4 of the 5 scan-1 picks came from one repo (git-into-gsoc). Added a ranking rule to keep at most 2 results per repo.
- 2026-10-21: The rubric's repo-in-use check passed repos created that same day with 0-1 stars, because one day of commits counts as "activity". Added a clause to repo-in-use in rubric.md: the repo must be at least 60 days old at capture.
- 2026-10-21: Tried a second scan to see the fixes work, and a grade run on my picks; both stopped with "Credit balance is too low", so the fixes are checked only by running the queries by hand so far.
