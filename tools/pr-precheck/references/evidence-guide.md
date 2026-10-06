# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives: In an eval bundle, the "Plan context" block holds the
  plan's scope pair (in scope / not in scope), its files, its approach,
  and any deviation or deferral note; the "Candidate PR" block's Diff
  shows what actually changed, and its Description makes the fidelity
  claims. In live mode: `plan.md` (including its Deviations section),
  `git diff main...HEAD` run from the working copy for the diff, and
  the draft description.
- What good looks like: every changed file and hunk falls inside the
  plan's stated boundary or inside a deviation note, and every planned
  deliverable is present or explicitly deferred. Drift runs in both
  directions: more than the plan (an extra option, rename, rewrite, or
  file) or less with no note (a promised docs or test change missing).
  An honest deviation or deferral re-ties a mismatch. A description
  that claims more or less than the diff delivers ("exactly as
  planned", "docs updated") is silent drift, not a comms problem; read
  every such claim against the hunks, never against the confidence of
  the wording.

## Test evidence (harness category: not-tested)

- Where it lives: In an eval bundle, the Candidate PR's test-evidence
  section, read against the plan's test plan and the repro evidence's
  own steps in the Plan context block. In live mode: your captured
  terminal output (the before and after runs, the test and lint
  output) read against `plan.md`'s Test plan.
- What good looks like: an observable behavior is named, the plan's
  repro is re-run on the changed code, the expected-after matches, and
  the outcome of the repo's own checks or the new test is visible.
  Decisive: "before: HTTP 503, redis unhealthy; after: redis healthy;
  3 passed". Not decisive: "tested locally", "works on my machine",
  "cargo test passes" with nothing named, a run of a control or
  unchanged path, a test that covers a side behavior and not the fix,
  or a transcript that covers one of two failure modes the plan's test
  plan names.

## Diff quality (harness category: unreviewable)

- Where it lives: the unified diff and the commit list in the
  Candidate PR block (live: `git diff main...HEAD` and
  `git log main..HEAD --oneline`).
- What good looks like: the fix is visible and nothing unrelated rides
  along. The debris tells to look for in added lines: debug prints
  (`print`, `eprintln`, `console.log`, `DEBUG`), commented-out code or
  earlier attempts, functions or imports nothing uses (including
  `allow(dead_code)`), added TODOs, re-indent or whitespace-only
  hunks, re-printed identical lines, import reshuffles, and edits to
  files or functions unrelated to the plan. Mechanical churn around a
  correct fix is still debris.

## Standards and comms (harness category: standards-wall)

- Where it lives: In an eval bundle, the Repo facts block's "pull
  requests" line (the PR template's required sections and artifacts)
  and "contribution policy" line (including any AI-use policy); the
  Issue's thread highlights for explicit maintainer direction. In live
  mode: the repo's PR template, CONTRIBUTING.md, and the issue thread
  on GitHub.
- What good looks like: each template ask is answered with real
  content in the description or visibly satisfied in the diff (a
  required changelog or whatsnew entry appears among the changed
  files; the issue-closing line is present). The AI disclosure, where
  the policy requires one, appears in the description in the author's
  own words. An explicit maintainer direction or question in the
  thread is followed or answered. Boilerplate, "N/A" everywhere, a
  blank checklist, or a silent skip of a stated ask is non-compliant.
  (Whether the description's claims match the diff is plan fidelity,
  above.)
