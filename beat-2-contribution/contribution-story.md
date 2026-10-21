# Contribution: Synchronous Keycloak initialization in OAuth2Manager blocks host application startup

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Zhaojun Xie
**GitHub Username:** ZJShown
**Issue:** https://github.com/eclipse-tractusx/tractusx-sdk/issues/269
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout, my Unit 1 skill grown a search strategy**

I installed the scout as an overlay on my issue-select skill, kept my rubric and evidence guide, and wrote a three-query Python-focused strategy that traces each query to a sentence of my fit profile. The first scan (300 results, 3 accepted, 2 rejected) showed two things: two machine-filed untether error reports took 2 of my 5 grading slots, and my rubric had no written rule for them or for a maintainer's "comment `I'll take this`" invitation in an issue body. I patched the rubric twice (a machine-generated-issue fail under Scope, and invitations are not claims under Nobody already on it) and the strategy twice (drop `[auto]` issues, one result per repo), and the patches flipped no verdicts. The re-scan no longer surfaced the untether reports or repeats, and the rubric correctly rejected a dead fixture repo (46-day-old last commit) and an issue that already had an open fix PR. A third scan, after I reordered filter 4 to put `good first issue` before comment count, kept the same three accepted issues at the top and then surfaced the case my rubric had mishandled: a repo created ten minutes before its issue, with one commit titled "Commit", passed "Maintainer alive". After scan 2 I had logged that tiny new repos passing was harmless and left it unpatched, and Dynamo showed that was wrong, so I patched the check to require at least 5 default-branch commits; Dynamo #31 flipped from accept to reject, and every other verdict was unchanged because my accepted repos have 37 to 300 commits. That makes three rubric patches (one tightening) and three strategy patches across three scans.

**Unit 6: voice-guard on the posting door, a scope patch, and the wild scopes**

I patched my rubric's scope row (it became `Scope is feasible`, with a difficulty note: straightforward, moderate or ambitious) before the scan, folded my three voice-guide copies into one file at `~/.claude/voice-guard/voice-guide.md`, wrote six upstream rules (claims, questions, pings, each with a wrong/right pair), installed the guard as a `PreToolUse` hook, and swapped repro-check's and plan-check's scopes for the wild ones. The hook's wiring test (`gh issue comment --help`) was stopped with "no body the guard can read", and the guard then stopped my own claim draft twice, once because the AI disclosure sat last instead of before anything else about the bug, once because I had bundled two questions into one. I fixed the draft each time instead of the rules, the claim posted with a `[hook] pass` in `guard-log.md`, and the patched rubric's scan accepted my chosen issue. The scan's first headless launch also stalled on a `gh` permission prompt, and the old claim draft from my sandbox work would have been stopped too (no "I'll say so if I stop"). The repo memory file is written after class and joins this entry then.

**Toolkit**

- Tool: voice-guard (`check_voice.py` as a hook plus the `--draft` manual doorway), reading my canonical voice guide; I ran it through Claude Code, with repro-check grading the claim draft first.
- What it produced: two stops that quoted my own rules (AI disclosure before the bug; one question per question), then a pass, and a dated line for each in `guard-log.md`, including the hook pass on the real post.
- Difficulties: the guard stopped a draft that repro-check had already accepted, so the two checks do not agree on strictness; the disclosure rule also binds even where the repo states no AI policy. A headless scan and grader needed explicit `gh` and read permissions before they would run.
- What changed: I reordered the claim (disclosure under the intent line) and cut it to one question, with the exception type and 503 default stated instead of asked. The rules themselves stayed as written.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my Unit 5 strategy: three queries that each trace to a sentence of my fit profile (Python, backend and API work, bugs with a runnable repro), after I patched the rubric's scope row so open edges inform my choice instead of rejecting the issue. The scan graded five candidates, all from the `good first issue` query: three accepts and two rejects, and both rejects already had an open PR on them, which is the "nobody already on it" check doing its job. My issue, eclipse-tractusx/tractusx-sdk#269 ("Synchronous Keycloak initialization in OAuth2Manager blocks host application startup"), was accepted with all required checks passing and a difficulty note of **ambitious**: the issue itself offers two designs (lazy initialization on the first request, or a background thread), and its `refine-me!` label says the maintainers consider it unrefined. I chose it by the three lenses. Fit: it is a Python SDK's startup and config path with a clear repro (an unreachable Keycloak URL under a minimal FastAPI host), which is my lane. Energy: I would happily work on it as long as it is solvable, and it looks solvable. Thread: open, no comments, no assignee, no linked PR when I checked. Of the other two accepts, ma-lit-bills#20 was easier and I considered switching, but I wanted the harder problem, and the open design edge became the first question in my claim instead of a reason to avoid the issue. I did not choose against my scout: its verdict was accept.

I read the repo's rules before claiming. CONTRIBUTING.md is Eclipse boilerplate whose one hard gate is signing the Eclipse Contributor Agreement before a PR; CI requires SPDX license headers on new files; the repo states no AI policy, so I disclosed AI assistance in the claim under the course's practice. Claim posted 2026-10-21: https://github.com/eclipse-tractusx/tractusx-sdk/issues/269#issuecomment-6323999950

Honest read of the tool: the scope patch did what it was meant to, since the scan accepted three issues instead of none, but it also accepted Conclave#59 (a screenshot and two sentences) on scope, and the patched row no longer carries my 2026-10-12 clauses on machine-generated reports, abandoned PRs and undecided end states, so I will watch for verdicts those clauses would have rejected. The scan also let documentation-labelled issues through filter 3 (exact-label matching, which a substring match would fix), and the freshest issues were the most likely to have a competing PR already, which no ranking filter can see. I have not patched the strategy for either yet, and I have not rerun my Unit 1 harness against the patched rubric.

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric, after the scope patch), launched as `claude "issue-scout: run a scan"`; I re-ran it once with `gh` allowed after the headless run stalled on a permission prompt. The scan is saved in `~/.claude/skills/issue-scout/scan-output.md`.
- What it produced: five graded candidates, three accepts and two rejects; #269 accepted with every required check passing, difficulty ambitious, and the preferred newcomer check passing too.
- Difficulties: the freshest candidates raced open PRs, filter 3 let documentation-labelled issues through, the scan could not write its own friction notes, and Conclave#59 passed scope on very thin evidence.
- What changed: I logged the choice and the scope patch in `scan-notes.md`. The substring-match fix for filter 3 and a harness regression run are still to do.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
