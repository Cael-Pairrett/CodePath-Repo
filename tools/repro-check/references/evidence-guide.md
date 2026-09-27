# Evidence guide: where proof lives in a reproduction package

## Environment

- **Where it lives (eval):** the repro report's environment / env block; sometimes a one-line env mention in the claim. Cross-check against the issue body's stated version/OS/driver/shell/browser and against the `## Repo facts` latest-release line when version delta matters.
- **Where it lives (live):** the student's draft repro comment; the issue body's environment section; `gh issue view` / the issue page for what the reporter targeted; release tags or README only when the draft claims a version.
- **What good looks like:** tool/subject version and OS (or platform) are named with enough precision to place the attempt. When the issue itself hinges on an extra dimension (minikube driver, fish vs zsh, browser language order, debug vs release build, Store vs main), that dimension appears in the record. A missing environment section is not "implied by the steps."

## Steps

- **Where it lives (eval):** the repro report's steps / commands / numbered procedure. Judge them against the issue's own reproduction steps and against whether fixtures are inline or linked in the package (issue body, playground link, or commands that create the file).
- **Where it lives (live):** the draft repro comment's steps; the issue body's "steps to reproduce"; any linked gist/playground the draft cites (open it only if the draft depends on it).
- **What good looks like:** a stranger can start from a clean state, run the named commands, and hit the same trigger without guessing. Inputs are public or created by the steps. Fail signals: "start the cluster" with no driver on a driver-specific bug; "checked out our private monorepo" / "used our internal config" with nothing shareable; "set up the project" with no commands.

## Behavior shown

- **Where it lives (eval):** fenced output excerpts, logs, console dumps, CSS panes, measurements, or explicitly described screenshots inside the repro report. Read them next to the issue body's "actual behavior" / error text / panic / screenshot description — not next to the report's headings or confidence adjectives.
- **Where it lives (live):** the same regions of the draft; attached images on the draft or issue; terminal paste in the draft.
- **What good looks like:** the artifact depicts the issue's specific symptom class (same panic message family, missing `Content-Type`, wrong mode-2031 answer, blank unresponsive pane, lost keystrokes, etc.). For cannot-reproduce, the artifact shows the run that did *not* trigger the bug. Setup-only proof (version prints, "session is running", tabs visible) does not show the bug. An adjacent failure (parse error instead of panic; compile error instead of path error; garbled sixels with a returned prompt instead of a crash) is not a match even when the write-up is polished.

## Honesty

- **Where it lives (eval):** the claim's promise ("I reproduced", "crash confirmed", "cannot reproduce", "root cause is X") plus the repro report's Expected/Actual/Result sentences, judged only against the artifacts in Behavior shown. Also read any version/platform delta sentence for silent mismatches.
- **Where it lives (live):** the same sentences in the drafts; do not grade intent from files the stranger on the thread will never see.
- **What good looks like:** prose and artifact agree. "Reproduced" means the bug symptom is in the paste. "Cannot reproduce" names what was tried, what was observed, and what differed (OS, shell, ARG_MAX, name-length distribution, etc.) — that is a ready package. Red flags: narrating a crash over a graceful error; Expected/Actual swapped relative to the paste; "I verified the race" / confident diagnosis with no transcript; claiming the Store build crashes when the paste shows the window still alive.

## Comms

- **Where it lives (eval):**
  - **Claim quality:** `## Candidate claim comment` against the issue title/body (is the subject concrete? is the next step modest and specific?).
  - **Repo conventions / AI policy:** `## Repo facts` lines for bug-report template asks and contribution / AI policy. Apply disclosure or human-voice rules only when that block states them.
- **Where it lives (live):** the draft claim (and draft repro) against the issue thread; CONTRIBUTING.md / AI_POLICY.md / issue templates in the scoped repo for disclosure and template norms.
- **What good looks like:** the claim could not be pasted onto a different issue; it names this bug and a real next action. Me-too/+1, "assign me, fix in 2 days guaranteed", or empty enthusiasm fail specificity. On AI policy: if repo facts require disclosing AI use on comments/issues, a disclosure sentence is present; if they require human-voiced comments, the text is specific to this issue in a human register; if policy is absent or permissive without those asks, do not invent a disclosure requirement.
