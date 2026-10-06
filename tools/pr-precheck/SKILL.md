---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this ready
to submit? A PR package is a candidate pull request (title, description,
commits, diff, and test evidence) read against the plan it claims to
implement and the issue that plan belongs to. Never answer a different
question, never grade more than one package per run, and never answer
from gut feel: execute the components in this directory.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** is the student's own submission, checked before it goes
out. Gather:

- `plan.md` with its deviation notes (a house-chain student reads the
  house plan and the house repro pack instead; the same checks grade
  the same things).
- The branch's diff: everything the branch changes relative to the
  default branch, produced by `git diff main...HEAD` (three dots) run
  from the working copy, and the commits from `git log main..HEAD
  --oneline`.
- The draft PR title and description.
- The test evidence (captured terminal output of the before and after
  runs, the test and lint results).
- The issue, read against: the thread, the repo's PR template, and the
  stated contribution and AI policy, gathered from the real repo.

**Eval mode** is a package bundle. The bundle is the whole world: every
fact comes from the bundle text, nothing is fetched, nothing else is
read. Grade a complete package: every check, the full verdict rule.

If the mode is unclear, ask which one before grading.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Take from it the
repo the PR must live in and the house rules of that environment. Refuse
to grade a PR whose repo is not the scoped repo, and say so. If the
scope's repo line still carries an unfilled placeholder (a bracketed or
angle-bracketed value such as `<ORG>/<PATH-REVIEW-REPO>`), stop without
grading and tell the student to get the cohort's scope file from the
instructor. Never guess a scope. In eval mode, ignore `scope.md`
entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after reading the package and before
the summary. Hold the outgoing PR text, the title and the description,
against its rules and report any rule the draft breaks in the summary,
quoting the offending words. The voice guide never changes the verdict
on its own, because it is personal style and the verdict is decided by
the rubric's checks alone unless a rubric check reads it. In eval mode,
ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` for the checks and the verdict rule, and
`references/evidence-guide.md` for where each family of evidence lives
and what good looks like. Execute `procedure.md` as written: its read
order, evidence gathering, check execution, and verdict assembly. If
the procedure is silent on a step you need, report the gap in the
summary; never improvise around it. If `rubric.md` has no check rows or
no verdict rule, or `procedure.md` has no written steps (only template
comments count as empty), refuse to grade and say which file is empty.
Never invent checks at runtime.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict, no "accept with reservations", and no score:
reservations belong in check evidence lines. Write a short readable
per-check summary first (in live mode, including any broken voice-guide
rules), then end the reply with one fenced JSON block of this exact
schema. The block must be valid, must be the last thing in the reply,
and nothing may follow it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: name the fact or quote that decided each check
  before grading it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the evidence, and the
  description against the plan, the issue, and the stated standards. A
  terse complete PR can be ready, and a polished confident one can hide
  drift; never grade formatting, length, or tone.
- The rubric decides, not the run: if a check passes by its stated
  condition but feels wrong, it still passes; the fix belongs in the
  rubric.
- The procedure decides how, not the run: follow `procedure.md` as
  written and report its gaps.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict
  rule directs; where the rule is silent, an unverifiable claim is a
  failing one, because a PR that cannot be verified from the package is
  not ready to submit.
