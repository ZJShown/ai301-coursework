# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62

**Verdict output**

```
## Evidence gathered

**Repo-level (applies to all three):**
- **Maintainer alive** — pass: most recent default-branch commit (`996fabe`, 2026-08-24) is 22 days before capture date 2026-09-15, inside the 30-day window.
- **Repo in use** — pass: `archived: false`, last push 2026-09-10 (5 days ago).
- **AI-assisted contribution allowed** — pass: `docs/CONTRIBUTING.md` (the linked policy) and README state no AI-use restriction at all; silence passes.

**Per-issue (all three: no assignees, no comments, no linked PRs — timeline shows only label events):**

| # | Title | Scope | Claim status | Newcomer signal (preferred) |
|---|---|---|---|---|
| 72 | `verify_password` raises instead of returning `False` | Settled: fail closed on malformed hash; names `core/security.py`, test, xfail id H-05 | Open | `good first issue` label but **no repro/acceptance list** in body — fail |
| 62 | Health check references nonexistent `settings.redis_host` | Settled: `Settings` model missing field; explicit repro (`GET /health`, `grep` command) | Open | `good first issue` + explicit repro — pass |
| 61 | Health check DB probe passes raw SQL string | Settled: wrap `"SELECT 1"` in `sqlalchemy.text()`; explicit repro with exact error text | Open | `good first issue` + explicit repro — pass |

All three clear every required check → **accept** on all three.

**Ranking by fit profile** (Python/backend/API, wants to practice reading a service codebase and reproducing a bug from a short repro before fixing, wants to avoid docs/pure-test-fixture work):

1. **#62** — request-handler + config-schema bug, exactly matches the stated interest ("config, request handlers"), and comes with a two-command repro to run before touching the fix.
2. **#61** — also a request-handler bug in the same file with a precise repro and exact error text, slightly more SQLAlchemy-specific than pure config work.
3. **#72** — good backend/security-module bug and clean scope, but the body gives no explicit repro to reproduce first, which is the one thing the fit profile called out wanting practice with.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last default-branch commit 2026-08-24, 22 days before capture date 2026-09-15"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push 2026-09-10 (5 days ago)"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Body: 'Settings model does not define that field' with explicit repro command; single settled fix"},
      {"name": "Nobody already on it", "grade": "pass", "evidence": "assignees: []; no comments; no linked PRs"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; silence passes"},
      {"name": "Newcomer-friendly signal", "grade": "pass", "evidence": "Labeled 'good first issue' and body gives explicit repro steps (GET /health + grep command)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/61",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last default-branch commit 2026-08-24, 22 days before capture date 2026-09-15"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push 2026-09-10 (5 days ago)"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Body: raw SQL string needs sqlalchemy.text() wrapper; explicit repro with exact ArgumentError text"},
      {"name": "Nobody already on it", "grade": "pass", "evidence": "assignees: []; no comments; no linked PRs"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; silence passes"},
      {"name": "Newcomer-friendly signal", "grade": "pass", "evidence": "Labeled 'good first issue' and body gives explicit repro steps with exact error message"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last default-branch commit 2026-08-24, 22 days before capture date 2026-09-15"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push 2026-09-10 (5 days ago)"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Body: verify_password should fail closed instead of raising UnknownHashError; names files, xfail id H-05, effort estimate"},
      {"name": "Nobody already on it", "grade": "pass", "evidence": "assignees: []; no comments; no linked PRs"},
      {"name": "AI-assisted contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; silence passes"},
      {"name": "Newcomer-friendly signal", "grade": "fail", "evidence": "Labeled 'good first issue' but body gives no repro steps or acceptance-criteria checklist, only relevant files and an effort estimate"}
    ],
    "verdict": "accept"
  }
]
```
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run, initial rubric (`Maintainer alive` required both a recent commit AND a fast issue reply; `Scope fits a newcomer` required either a maintainer comment or a named file/line): **14/20 scored items** (below the 18/20 bar). `categories: clear-accept 2/8` was the failure.
2. Partial re-run, 14 items, after changing `Maintainer alive` from an AND to an OR (a repo can be actively maintained before its issue threads show engagement) and rewriting `Scope fits a newcomer` to accept a settled ask without a named file/line: **11/14**.
3. Partial re-run, 12 items, after sharpening `Scope fits a newcomer` to separate an undecided *end state* (fails) from an open *implementation approach*, an informal "etc." example list, or an optional lower-priority stretch item (none of which fail by themselves): **12/12**.
4. Partial re-run, 8 remaining items, confirming no regressions on the untouched claimed/dead-repo/policy cases: **8/8**.
5. Full confirming run, final rubric, saved with `--save-run`: **20/20 scored items (bar: 18/20 — PASS)**.

The last score (20/20) matches the `agreement:` line in the committed `eval-run.txt`.

**Issue analysis**

`issue-01` (source `conda/conda#16475`). Gold: `"category": "clear-accept", "verdict": "accept", "note": "docs task with a stated home and scope; active repo, unclaimed"`. My rubric's final decision: **accept** — matching gold, but only after two revisions.

The bundle's issue body lays out one settled documentation deliverable ("Create a new task page, for example: `Installing PyPI packages with conda`" plus an exact content outline) and updates to three named existing pages, then adds one aside: `"### Consider a global troubleshooting.rst entry ... Lower priority, but worth naming."` My first revision of `Scope fits a newcomer` still rejected this issue; the model's own evidence quote from that run was: `"Issue lists five separate deliverables (new page + edits to three other docs) and explicitly marks one as unsettled: 'Consider a global troubleshooting.rst entry... Lower priority, but worth naming.'"` It was treating the multi-file scope and the deprioritized aside as disqualifying.

The current check's text fixes this directly: `"an optional, explicitly lower-priority stretch item rides alongside an otherwise-settled core ask, in which case grade the core ask, not the stretch goal"` and `"multi-file scope... [is] not disqualifying by [itself]"`. Under that wording the core ask (one new stable docs page with a specified outline, plus three named page edits) is a single settled deliverable, the `troubleshooting.rst` mention is the stretch goal, and the check now passes — which is what flips the final verdict to `accept` and matches gold.

**Check rationale**

Check quoted verbatim from the `rubric.md` uploaded to `tools/issue-select/`:

> **Scope fits a newcomer** | Issue title, body, and comment thread, including any explicit TBD/undecided language, any repro or expected-behavior statement, and any abandoned PRs mentioned in the thread | The issue names one settled, executable deliverable, not a decision still to be made — whether the reporter calls it a bug or a request, and even when the work touches several files or several possible fix approaches. It fails when: the issue is itself a list of independently separable tasks meant to be split among different contributors (a genuine umbrella or tracking issue, or a self-described megaissue); the desired end state itself is undecided (an explicit "TBD", a missing asset or input with no source, or years of unsettled debate over what the feature should even do, with no maintainer resolution); the thread shows a history of abandoned closed PRs attempting the same fix; or it is a pure usage/support question. It passes when the target behavior is clear even if: the implementation approach is left open (a bug report naming multiple possible causes, or suggesting several ways to fix them, is still one settled ask if the "it should stop doing X" behavior is unambiguous); the scope is given informally via examples plus "etc." rather than an exhaustive list, as long as the category of work is bounded and recognizable; or an optional, explicitly lower-priority stretch item rides alongside an otherwise-settled core ask, in which case grade the core ask, not the stretch goal. Length, authorship, multi-file scope, and "bug" vs. "request" framing are not disqualifying by themselves — grade whether the target behavior is settled, not its label, polish, or file count. | required

Reasoning behind its current form: my first draft conflated "not fully specified" with "not settled." A real first-time contributor doesn't need every implementation detail decided in advance — they need to know what "done" looks like. This check now draws the line at the *end state*: an undecided end state (an explicit "TBD," an unresolved product decision, a self-described umbrella of independently separable tasks) is a genuine scope failure, but an open *implementation approach*, an informal "etc." list of examples, or an optional deprioritized stretch item riding along a settled core ask should not fail the issue by itself, because none of those leave a newcomer without a clear target to build toward.

**Trade-offs**

Being this lenient about "the implementation approach is left open" and "etc." example lists means the check will occasionally accept an issue where the informality is hiding real ambiguity rather than genuinely being a bounded, recognizable category — e.g. an "etc." that actually means "we haven't decided which cases count," not "you'll obviously know the rest." I accept this because, in this eval set, every case using that kind of informal language (`issue-04`: "remove identity, fuse spiders, remove self loops, etc.") is gold-labeled `accept`, while every case meant to fail on scope has an explicit, checkable signal instead of mere informality: `issue-05` ("not necessarily anywhere in the codebase"), `issue-10` (a literal list of unrelated issue numbers), `issue-15` (years of documented design debate), and `issue-20` ("Logo asset TBD"). I confirmed this is not a coincidence with a canary: re-running `--only` on those four scope-designed rejects together with the three newly-fixed accepts (`issue-01`, `issue-04`, `issue-19`) showed **12/12** agreement — loosening the "etc." and "stretch item" language did not cost any of the scope-reject cases in this set.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. My fit profile in `scope.md` says I'm most comfortable in Python/backend-API work and want practice reading an unfamiliar service codebase and reproducing a bug from a short repro before touching the fix. `#62` is exactly that: one missing field on a FastAPI `Settings` model, surfaced through `api/routes/health.py`, with a two-command repro (`GET /health`, then `grep redis_host core/config.py`) I can run in a few minutes before opening an editor. Given this is spare-time coursework, a fix this narrow is realistic to actually finish rather than something I might abandon partway.

2. The verdict correctly refused to read this repo's total silence (zero comments across all 72 open issues) as a dead maintainer, once I fixed `Maintainer alive` to accept commit-recency alone — that's a real judgment call encoded in the rubric, not something I overrode by hand. What the rubric can't see: I picked `#62` over the also-accepted `#61` partly on something outside its evidence — I've hit real "Pydantic Settings field doesn't exist" bugs before, so I have a faster mental model for that failure mode than for `#61`'s SQLAlchemy 2.x `text()` gotcha, even though the rubric rates them as equally bounded and equally unclaimed.

3. Because this is a freshly-seeded classroom repo with no comments or assignees on any of the 72 open issues, there's no visible competition yet, and the Path Review house rule in `scope.md` means another student's claim comment wouldn't block me even if one appears first. The real risk isn't claiming it — it's that the fix is a one-line diff on an obviously attractive `good first issue`-labeled bug, so more than one classmate may submit close to the same patch.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
