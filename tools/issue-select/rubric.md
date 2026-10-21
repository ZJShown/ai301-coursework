# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo facts: "last 5 default-branch commits" (dates and authors) and "maintainer first-response sample" | Either signal alone is enough (a repo can be actively maintained before its issue threads show engagement): the default branch has at least 5 commits, and at least one of the last 5 is dated within 30 days of the capture date (today, in live mode) — a bot-authored commit only counts if it merges a human-authored pull request — OR at least one entry in the first-response sample shows an owner/member/collaborator reply within 14 days of that issue's open date. Fails only when neither signal is present. | required |
| Repo in use | Repo facts: "archived:" flag and "last push to any branch" | `archived` is `no`, AND the last push to any branch is within 60 days of the capture date. | required |
| Scope is feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. | required |
| Nobody already on it | Repo facts: this issue's assignees and linked PRs, noting open vs. closed; comment thread for claim language, stale-bot markers, and any later maintainer comment | No assignee is currently set. The issue is blocked by an open linked PR, or by a claim comment ("I'll take this", "/assign", "working on this", "opened a PR for this") that nothing since has undercut. A closed or unmerged PR is an abandoned attempt and does not block by itself. A standing invitation written by the issue's author or a maintainer in the issue body or a template (e.g. "comment `I'll take this` and we'll assign you") is not a claim comment; only a comment from someone else claiming the issue counts. A claim comment stops blocking once it has gone stale: no PR ever followed it, a long gap of inactivity has passed since, and something since — an automated stale-bot marker, a maintainer's later bump, or an explicit re-invitation — treats the issue as open again. | required |
| AI-assisted contribution allowed | Repo facts: "contribution policy" line (and any linked AI-policy file it names) | The policy contains no outright ban on AI-assisted or AI-generated contributions. Disclosure, human-review, testing, or "must understand every change" conditions are terms to follow, not a fail. Silence (no policy stated) passes. | required |
| Newcomer-friendly signal | Issue labels; presence of a reproduction snippet or acceptance-criteria checklist in the body | The issue carries a `good first issue` (or equivalent) label AND the body gives either a concrete repro/snippet or an explicit acceptance-criteria list. | preferred |

## Difficulty note

With each accepted candidate, report a one-line difficulty note in the
read-out summary:

- **straightforward**: the issue names where the change lives and the
  change is bounded.
- **moderate**: the location is mine to find, or several files are
  involved.
- **ambitious**: an open design edge, or an unfamiliar subsystem; name
  the edge, because claiming this issue means raising it with the
  maintainer early.

The note is information for the chooser, never a verdict input: it does
not appear in the fenced JSON block, it never changes accept or reject,
and in eval mode it is skipped entirely.

## Verdict rule

Accept only if every `required` check grades `pass`. A single required check
graded `fail` or `unclear` rejects the issue — `unclear` is treated as `fail`
because a first issue whose maintainer-life, repo-health, scope, or claim
status can't be verified from the evidence is not one to hand to a
newcomer. `preferred` checks never affect the verdict; they only order
accepted issues by the fit profile in `scope.md` (live mode only).
