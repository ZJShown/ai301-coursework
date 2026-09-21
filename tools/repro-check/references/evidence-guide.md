# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: In an eval bundle, the repro report's "Environment:"
  line, read together with the Repo facts block's "latest release"
  line and the Issue section's own stated affected version(s). In live
  mode, the draft repro comment's own environment statement, checked
  against the issue's version/OS fields and the repo's actual latest
  release (`gh release view`, the releases page, or the issue body).
- What good looks like: the tool/library version, OS/platform, and any
  other variable the issue's trigger names (browser, driver, install
  method, dependency version) are all stated — enough that a stranger
  could place the exact conditions of the attempt. When the tested
  version or platform differs from what the issue targets (or the
  latest release, if the issue pins none), the report names the
  difference in its own words instead of quietly substituting one
  version's result for another's.

## Steps

- Where it lives: In an eval bundle, the repro report's "Steps:"
  section, read against the Issue section's own reproduction steps or
  trigger description to see whether the same trigger is used. In live
  mode, the draft repro comment's steps, checked against the issue's
  stated repro steps or minimal example.
- What good looks like: a stranger with the stated environment could
  run the steps as written — a defined starting point (files, config,
  or input shown or attached in the report) leading to the specific
  action the issue names as the trigger. Nothing lives only in the
  reporter's private setup (an unshared config, a monorepo path nobody
  else has); nothing skips past the trigger straight to a result.

## Behavior shown

- Where it lives: In an eval bundle, the artifact inside the repro
  report (the command output, log excerpt, error, or described screen
  state) plus its "Expected"/"Actual" lines, read against the Issue
  section's own excerpt of the current and expected behavior. In live
  mode, the draft repro comment's pasted output/log/screenshot, read
  against the issue's stated current and expected behavior.
- What good looks like: the artifact shows the same failure mode, or
  the same present-vs-absent behavior, the issue names, produced from
  the issue's actual trigger (not a different flag, syntax, or input).
  An honest "I could not reproduce this" counts as behavior shown too,
  as long as the attempt used the real trigger and the report says
  plainly what happened and what differed from the issue's setup.
  Watch for the adjacent-symptom trap: a different exit code, a
  different error class, or the target program still running when a
  crash was claimed are not the issue's behavior even when the
  transcript looks polished and confident.

## Honesty

- Where it lives: In an eval bundle, the repro report's own concluding
  language (its "Actual"/"Analysis"/summary lines) and the claim
  comment's stated certainty, read against the artifact found for
  "Behavior shown" above. In live mode, the same, in the draft
  comments.
- What good looks like: the words claim exactly what the artifact
  supports — no "confirmed," "guaranteed," or "proves" attached to a
  result the artifact doesn't carry, and nothing generalized to a
  platform, release, or scenario that was never actually tested.
  Running the same command many times is repetition, not new evidence,
  and the report should not lean on it as if it were. A cannot-
  reproduce result stated as exactly that is honest; a different
  result quietly reframed as confirmation is not.

## Comms

- Where it lives: In an eval bundle, the claim comment's and repro
  report's own wording, read against the Repo facts block's "bug
  reports" template line and "contribution policy" line (including any
  named AI-policy file). In live mode, the draft comments, read
  against the repo's actual issue template and CONTRIBUTING.md /
  AI-policy file.
- What good looks like: the comment could not be pasted unchanged onto
  a different issue — it names this issue's actual behavior or a
  thread detail; it promises only what the poster can keep (no
  guaranteed fix timelines, no reserving the issue by tone alone); the
  repro report answers what the repo's template asks. When the
  contribution/AI policy requires disclosing AI assistance, the
  comments name the tool and the extent of its use; when the policy is
  silent, or merely conditions AI use without requiring disclosure, no
  disclosure statement is needed to pass.
