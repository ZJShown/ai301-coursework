# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo facts: "last 5 default-branch commits" (dates and authors) and "maintainer first-response sample" | At least one of the last 5 default-branch commits is dated within 30 days of the capture date (today, in live mode); a bot-authored commit only counts if it merges a human-authored pull request. AND at least one entry in the first-response sample shows an owner/member/collaborator reply within 14 days of that issue's open date. | required |
| Repo in use | Repo facts: "archived:" flag and "last push to any branch" | `archived` is `no`, AND the last push to any branch is within 60 days of the capture date. | required |
| Scope fits a newcomer | Issue title, body, and comment thread | The issue asks for one bounded change with a settled approach: it is not framed as an umbrella/tracking issue, the thread shows no open design debate (either a maintainer has confirmed the direction, or the issue is a bug report with a specific diagnosis and a named file/line), and it is not a pure usage/support question ("how do I..."). A terse body still passes if the fix location and expected behavior are both concrete. | required |
| Nobody already on it | Repo facts: "this issue: assignees" and "linked PRs"; comment thread for claim language and PR mentions | No assignee is set, no PR (open, or one that already claims to implement the fix) is linked or mentioned in the thread, and no unwithdrawn claim comment ("I'll take this", "/assign", "working on this", "opened a PR for this") appears. | required |
| AI-assisted contribution allowed | Repo facts: "contribution policy" line (and any linked AI-policy file it names) | The policy contains no outright ban on AI-assisted or AI-generated contributions. Disclosure, human-review, testing, or "must understand every change" conditions are terms to follow, not a fail. Silence (no policy stated) passes. | required |
| Newcomer-friendly signal | Issue labels; presence of a reproduction snippet or acceptance-criteria checklist in the body | The issue carries a `good first issue` (or equivalent) label AND the body gives either a concrete repro/snippet or an explicit acceptance-criteria list. | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. A single required check
graded `fail` or `unclear` rejects the issue — `unclear` is treated as `fail`
because a first issue whose maintainer-life, repo-health, scope, or claim
status can't be verified from the evidence is not one to hand to a
newcomer. `preferred` checks never affect the verdict; they only order
accepted issues by the fit profile in `scope.md` (live mode only).
