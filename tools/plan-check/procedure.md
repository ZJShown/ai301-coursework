# Procedure: how this skill grades a plan package

## Read order

1. **Issue** section first: the reported behavior, the expected
   behavior, and (live mode) the repo-facts block's template and
   policy lines. Note the reported behavior in one sentence — every
   later check measures against this, not against what the plan says
   the issue is about.
2. **Thread highlights** next, before the repro evidence or the plan.
   Note any explicit maintainer-stated (OWNER/MEMBER/COLLABORATOR/
   CONTRIBUTOR) direction, cause, or rejected approach, and separately
   note any non-maintainer speculation — the Comms check needs the
   former only, and reading this before the plan stops the plan's own
   framing from coloring what "explicit direction" looks like.
3. **Repro evidence** third: every step, control run, and artifact,
   in order. Note, for each control, what variable it isolates and
   which way the behavior went. This is the fact base every other
   check is read against; reading the plan first invites grading the
   plan's own story instead of what the evidence shows.
4. **Candidate plan** fourth: its stated cause, its scope statement,
   its files/approach, its test plan, and any stated risks or open
   questions — wherever each appears, labelled or not (see the
   evidence guide's note on varying shapes).
5. **Candidate plan comment** last, immediately before grading Comms:
   read it once for what it commits to, once against the thread notes
   from step 2.

Live mode only: read `scope.md` before step 1, and confirm the issue
sits in the scoped repo before doing anything else. Read
`voice-guide.md` after step 5, immediately before checking Comms.

## Evidence gathering

For each check, pull the fact from the read-order note taken above
rather than re-scanning the package:

- **Diagnosis grounded**: the stated-cause note from step 4, plus the
  full control-run list from step 3. Record, for each control, whether
  the stated cause predicts that control's actual result.
- **Scope bounded**: the in-scope/not-in-scope note from step 4, plus
  the one-sentence reported behavior from step 1. Record every file,
  area, or goal the plan names that is not required to fix that
  reported behavior.
- **Executable**: the approach/files note from step 4. Record the
  first concrete action verbatim, and flag any step whose verb is an
  open-ended research verb ("investigate," "look into," "profile and
  see") rather than a decision, and any explicitly deferred choice.
- **Test plan decisive**: the test-plan note from step 4, matched
  against the specific repro steps/artifacts from step 3. Record
  whether the named result is observable and tied to this fix, or is
  a general activity.
- **Honest about unknowns**: the risk/open-question note from step 4,
  read against the plan's own confidence language elsewhere in the
  same text.
- **Comms respects thread and repo**: the maintainer-direction note
  and the AI-policy line from step 2/step 1, matched against the
  candidate plan comment from step 5.

Live mode: gather issue-side evidence (thread, repo facts, AI policy)
live per `references/evidence-guide.md`'s live-mode pointers; take the
repro evidence from the student's own posted repro comment, or the
house repro pack on the house issue, per `SKILL.md`'s Inputs section.

## Check execution

Execute the checks in table order: Diagnosis grounded, Scope bounded,
Executable, Test plan decisive, Honest about unknowns, Comms respect
thread and repo. Earlier checks establish facts (what the repro
evidence actually shows, what the issue actually asks for) that later
checks reuse without re-deriving them.

Grade each check `pass`, `fail`, or `unclear` using the gathered
evidence and the rubric's pass condition, and write the one-line fact
or quote that decided it before moving to the next check. `unclear`
is reserved for evidence that is genuinely absent from the package
(no test plan appears at all; the thread has zero comments to check
against) — not for evidence that is present but makes the check fail.
Do not stop at the first failing required check: grade every check in
the table regardless of earlier results, since the output reports a
grade for each one and the summary should tell the student everything
that needs work, not just the first thing.

An eval-mode package is graded whole, every check, every time. A
live-mode claim-only state does not apply to this skill (plan-check
always has a candidate plan and comment to grade); if a live package is
missing the plan, the comment, or the repro evidence entirely, grade
every check that can still be evaluated and grade the rest `unclear`
with evidence "not present in the package."

## Verdict assembly

Apply the rubric's verdict rule: `accept` only if every `required`
check graded `pass`; any `required` check graded `fail` or `unclear`
means `reject`; `preferred` checks (Honest about unknowns) never
change the verdict. Quote, for each check in the output JSON, the
one-line fact recorded during Check execution — the control run that
confirmed or ruled out the diagnosis, the file named or missing, the
observable result named or absent, the maintainer comment engaged or
ignored. In live mode, after the JSON, list any voice-guide rule the
draft comment breaks. This assembly is mechanical: the same set of
check grades produces the same verdict every time, regardless of which
package produced them.
