# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: In an eval bundle, the candidate plan's stated cause
  — look for a "Diagnosis" or "Cause" label, but treat an unlabelled
  opening paragraph the same way if that is where the plan states why
  the bug happens. Read it against the "Repro evidence" section's
  steps, control runs, and artifacts (never against the "Thread
  highlights" section's guesses — a maintainer or reporter can guess
  wrong, and the repro evidence is what actually pins the behavior
  down). In live mode: the draft plan's stated cause, checked against
  the student's own posted repro comment (or the house repro pack) the
  same way.
- What good looks like: the stated cause is consistent with every
  control run in the repro evidence — nothing it predicts is
  contradicted by what a control actually showed. A diagnosis is an
  inference, not a transcript: it may offer its own plausible
  mechanism for why a supporting control came out the way it did (for
  example, "the cache-clear control fits because an empty cache forces
  one refetch along the same path a fresh attach uses") even when the
  repro evidence never states that mechanism in those words. That kind
  of reading is grounding, not a gap. The check fails only when a
  control's actual result actively points elsewhere — rules the
  claimed mechanism out, isolates a variable the diagnosis never
  addresses, or shows the claimed defect already absent before the
  claimed cause could act. Watch for the operator-swap trap: a plan
  that adopts a cause the thread already suggested is not thereby
  grounded — check the repro evidence's own controls against that
  specific cause, independent of who proposed it.

## Scope

- Where it lives: The candidate plan's in-scope/not-in-scope statement
  and the files or areas it names (a "Scope" label, a "Not in scope:"
  line, or a "Files"/"Files and areas" list), read against the "Issue"
  section's actual reported behavior and stated expected behavior —
  not against every improvement the surrounding code could use.
- What good looks like: one bounded change that addresses exactly the
  reported behavior. A plan that explicitly defers adjacent work
  ("not in scope: the tombstone rework; deferring because...") is
  doing scope right, not failing it — deferral with a stated reason is
  a pass. A plan whose "Proposed changes" section lists multiple
  independent fronts (a rewrite, a version bump, a new option, a test
  harness migration, a module restructure) alongside the actual fix is
  scope creep even when the write-up frames it as "doing it properly,"
  and even when the core fix buried inside it is itself correct.

## Executability

- Where it lives: The candidate plan's stated approach — an
  "Approach" section, a numbered steps list, or the files/areas
  named — read for whether the concrete first action and every
  choice it depends on are already made, not left as a question.
- What good looks like: a stranger could open the named file and start
  without asking the author anything. Contrast a real first action
  ("add a monotonic generation counter to the page," "clamp the fill
  with `saturating_sub` at line 934") against a research task dressed
  as a step ("investigate how lazygit reads mouse events," "profile
  and see what's slow," "look into whether X behaves differently").
  A plan that leaves a real decision open for build time ("upstream or
  vendored, whichever is easier") is not executable yet, no matter how
  many steps surround it.

## Test plan

- Where it lives: The candidate plan's stated test plan (a "Test
  plan"/"Test" label, or the closing paragraph describing how success
  will be checked), read against the "Repro evidence" section's own
  steps, artifacts, and control runs.
- What good looks like: a decisive, fix-specific check — re-running
  the repro's own steps with a named expected result (an exit code, a
  specific string in the output, a control that must stay unchanged,
  a value that must now differ), or an equally concrete check tied to
  this fix. Watch for the "runs the process but proves nothing" trap:
  "run the full test suite and make sure nothing regresses" and
  "should feel fast now" are both activities, not observable outcomes
  for this specific bug — they fail even attached to an otherwise
  strong plan, because they would pass whether or not the fix actually
  worked.

## Honesty

- Where it lives: A "Risk," "Open question," or similar line in the
  candidate plan, read against that same plan's confidence language
  elsewhere (words like "clearly," "definitely," or a diagnosis stated
  with no hedge where the repro evidence leaves something genuinely
  untested).
- What good looks like: an unknown the repro evidence does not settle
  (an unmeasured cost, an unverified platform, a tool not yet checked
  against its docs) is named as open, with what would resolve it —
  not asserted as fact and not silently omitted. A mid-build deviation,
  live, gets recorded in `plan.md`'s Deviations section the same way:
  named plainly, not buried in a diff.

## Comms

- Where it lives: The candidate plan comment's own wording, read
  against two things: the "Thread highlights" section for an explicit
  maintainer-stated (OWNER/MEMBER/COLLABORATOR/CONTRIBUTOR) direction,
  cause, or rejected approach; and the "Repo facts" block's
  contribution-policy and AI-policy lines. In live mode: the draft
  plan comment, read against the live thread and the repo's actual
  CONTRIBUTING.md / AI-policy file per `references/evidence-guide.md`'s
  live-mode note below and `scope.md`'s house rules.
- What good looks like: when the thread carries an explicit
  maintainer-stated direction (a named cause, a preferred fix, an
  approach already tried and rejected as too expensive), the comment
  engages it by name — follows it, or gives an evidence-backed reason
  for a different path — rather than reading as if the thread were
  empty. A thread with only reporter/bystander speculation, or no
  comments at all, carries no such direction to engage, and the check
  passes by default. When the repo's stated policy requires disclosing
  AI assistance, the comment names the tool and the extent of its use
  in its own words; when the policy is silent or merely welcomes
  AI-assisted work without requiring disclosure, no disclosure
  statement is needed. A comment that could be pasted unchanged onto a
  different issue on the same repo is the shape a failing comment
  usually takes, even when it happens to pass the checks above.
