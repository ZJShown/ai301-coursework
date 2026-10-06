# Procedure: how this tool grades a PR package

## Read order

1. **Repo facts and issue first.** Note the reported behavior in one
   sentence, the PR template's stated asks (each section and each
   required artifact), and the contribution policy including any
   AI-use requirement. Every later check measures against these, not
   against what the PR says they are.
2. **Thread highlights second**, before the plan or the PR. Note any
   explicit maintainer direction, cause, or open question addressed
   to the author, separately from other commenters' speculation.
3. **Plan context third.** Note: the in-scope list, the not-in-scope
   list, the files named, the approach steps (list every deliverable),
   the test plan (list every scenario and failure mode it names), and
   any deviation or deferral note. The plan comes before the diff so
   the diff is read against a fixed boundary instead of the boundary
   being reverse-engineered from the diff.
4. **Diff and commits fourth.** List every changed file, and for each
   hunk write what it does in a few words. Mark each hunk "in plan",
   "deviation-noted", or "not in plan". Separately list any debris
   (debug prints, commented-out code, dead code, TODOs, formatting or
   import churn).
5. **Description and test evidence last.** Note each factual claim in
   the description (fidelity, coverage, "no other changes"), the
   disclosure statement if any, and each template section's content.
   Note exactly what the test evidence ran, what it showed before and
   after, and which repo checks have a visible outcome. The
   description comes last so its claims are checked against the diff
   instead of coloring how the diff is read.

Live mode only: read `scope.md` before step 1 and stop if the PR is
out of scope or the scope is unfilled (per SKILL.md). Read
`voice-guide.md` after step 5. Gather inputs per SKILL.md: the diff
from `git diff main...HEAD`, commits from `git log main..HEAD
--oneline`, `plan.md`, the draft title and description, the captured
test output, and the repo's PR template, CONTRIBUTING.md, and issue
thread from GitHub.

## Evidence gathering

Pull each fact from the read-order notes rather than re-scanning:

- **Diff matches plan**: the step-4 hunk marks against the step-3
  scope pair and deviation notes. Record every "not in plan" hunk, and
  every plan deliverable with no matching hunk and no deferral.
- **Description matches diff**: the step-5 claim list against the
  step-4 hunk list. For each claim record "true", "false", or
  "unsupported" and the hunk or absence that decided it.
- **Test evidence decisive**: the step-5 evidence note against the
  step-3 test-plan scenarios. For each scenario record whether it was
  re-run on the changed path with a before and an after, and whether
  the repo's checks or the new test show an outcome. Record the quote
  that decided it.
- **Diff reviewable**: the step-4 debris list. Record each item with
  its file; an empty list is itself the finding.
- **Template and policy honored**: the step-1 template asks against
  the step-5 section contents and the step-4 changed-file list (for
  artifacts like a changelog entry). Record each ask as satisfied or
  not, with where.
- **AI disclosure present**: the step-1 policy line against the
  description text. Record the disclosure sentence or its absence.
- **Maintainer direction engaged**: the step-2 direction note against
  the description and diff. Record how it was followed or answered, or
  that none exists.
- **Commits legible**: the commit messages against the diff.

## Check execution

Execute the checks in rubric order. Grade each `pass`, `fail`, or
`unclear` from the gathered evidence and the rubric's pass condition,
and write the one-line fact or quote that decided it before moving on.
Use `unclear` only when the evidence is genuinely absent from the
package (no test evidence section, no plan scope at all), not when it
is present and the check fails. Do not stop at the first failing
check: grade every check, since the output reports each one. A check
may be graded from the gathered notes without re-reading the package,
unless a note is missing, in which case read only the part named by
the evidence guide for that check. If the rubric's pass condition
holds but the result feels wrong, grade it by the condition. If this
procedure is silent on a step you need, say so in the summary rather
than inventing a step. Eval mode grades the whole package, every
check, every time.

## Verdict assembly

Apply the rubric's verdict rule: `accept` only if every `required`
check graded `pass`; any `required` check graded `fail` or `unclear`
means `reject`; `preferred` checks never change the verdict. `unclear`
counts as `fail`. When more than one required check failed, name the
first failing required check in rubric order as the deciding check in
the summary line before the JSON. Put each check's one-line deciding
fact or quote in its `evidence` field. In live mode, after the
per-check summary and before the JSON, list any voice-guide rule the
draft title or description breaks; the JSON block is still the last
thing in the reply. The same set of grades always yields the same
verdict.
