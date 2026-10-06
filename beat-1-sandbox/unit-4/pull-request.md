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

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/93

**Branch**

fix/62-redis-health-check-url

**My own pr-precheck verdict on my draft (live mode, run against my branch and `pr_draft.md`)**

`accept`. All seven required checks passed; the preferred check, Commits legible, also passed. My first live run graded `reject` on "Template and policy honored": `docs/CONTRIBUTING.md` and the PR template ask for the seeded bug's `pyproject.toml` suppression to be removed, and my branch had not done it. I removed `attr-defined` from the `api.routes.health` mypy override, added a deviation note to `plan.md`, updated the draft, and re-ran. The grader's closing JSON line: `"verdict": "accept"`. Its evidence for the check that had failed: "All template sections carry real, honestly-caveated content; Closes #62 present; xfail/suppression checklist item answered correctly."

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Two full runs, in order: 20/20, then 20/20. The last matches the committed `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. I made no revisions to the tool between the runs, and I did no `--only` re-runs.

**Package analysis**

`pkg-09` (silent-drift). My rubric decided `reject`, and the gold label is `reject`. It failed four checks. The decisive one was "Diff matches plan", with this evidence: "Diff adds a new --time-format flag and rewrites src/error.rs's print_error for all errors, both explicitly excluded by the plan ('new flags', 'other error messages'); plan's 'one unit test per failure kind' deliverable is absent with no deferral note." "Description matches diff" also failed: "Description says 'CHANGELOG entry added' but no CHANGELOG.md change appears in the diff, and 'exactly as planned' is contradicted by the --time-format flag and error.rs change." The rubric reads it this way because it reads each hunk and each description claim against the plan's boundary, not against how plausible or adjacent the extra work is. The gold note calls this one arguable, since the extra work is adjacent. The rubric rejects it anyway: a flag the plan scoped out is unannounced scope, and a description saying "exactly as planned" over it is the drift.

**Check rationale**

Exactly as it reads now in `tools/pr-precheck/rubric.md`:

```text
| Diff matches plan | The diff's changed files and hunks, read against the plan's stated scope (the in-scope files and approach), its not-in-scope lines, and its deviation notes or stated deferrals. | Every changed file and hunk falls inside what the plan names or inside a deviation note that announces it, and every planned deliverable is present in the diff or is explicitly deferred in the plan's own scope note, a deviation note, or the description. A one-line fix plus a test the plan lists is a pass. Fails when a hunk does work the plan never named or scoped out (a new option, a rewrite of an adjacent function, a rename pass, a file the plan never touches), or when a planned deliverable is missing with no note anywhere, no matter how good the extra or remaining work is. An honestly disclosed deferral is a pass, not a fail: "less than everything" holds the package only when the shortfall is silent. | required |
```

It reads this way because of the honest-outcome lesson from my Unit 3 plan-check revision, where I moved a pass condition from "explains every control" to "consistent with the controls": judge the outcome, not a perfect transcript. Here that became the sentence "An honestly disclosed deferral is a pass, not a fail: 'less than everything' holds the package only when the shortfall is silent." I rejected the stricter reading where any planned deliverable missing from the diff holds the package, because it would reject the honestly-deferred packages (`pkg-13`, `pkg-16`), which gold marks accept. I did not revise this row in this unit: I wrote it once with that lesson built in, and it agreed with gold on the first full run.

**Trade-offs**

Nothing changed after the first full run, and here is how I know: both full runs scored 20/20 with the category floor met (`clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`), so there was no disagreement to revise against and no loosened check that needed an `--only` canary. A case I accept it will miss: "Diff reviewable" treats any added TODO or mechanical churn as debris, so a PR with one harmless, repo-conventional TODO would be held. I also accept that "AI disclosure present" passes whenever the repo states no policy, even though a reviewer might still want a disclosure. It never needed one to pass, and my own PR includes one anyway.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
