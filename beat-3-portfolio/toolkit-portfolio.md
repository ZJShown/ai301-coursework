# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.

**Student:** Zhaojun Xie

## Featured Tools

I feature issue-scout, voice-guard, pr-precheck with ship-gate (one tool), and pr-monitor. Each stood at a door I walked through on one real contribution, tractusx-sdk PR #290, so each has a log behind it. issue-scout and the ship-gate moved the most; voice-guard stopped the most drafts but its rules barely changed; pr-monitor has the thinnest history and says so. repro-check and plan-check mostly confirmed drafts I would have posted anyway, so they are in the Inventory.

**issue-scout**

**What it is, and the judgment hole I wrote:** It searches GitHub for first-contribution issues, ranks them with a search strategy, and grades the top five against a rubric. Both are mine: three queries that each trace to a sentence of my fit profile, and a rubric with checks like "Maintainer alive" and "Nobody already on it". One rule I wrote: a repo needs at least 5 default-branch commits to pass, since a first commit shows a repo exists, not that anyone reviews PRs.

**How it evolved, dated:** 2026-09-15: as Unit 1's `issue-select`, my first rubric scored 14/20 (bar 18); I changed `Maintainer alive` from AND to OR and rewrote the scope check, and the final run scored 20/20. 2026-10-12: the first scan spent 2 of 5 grading slots on machine-filed error reports, so I patched the rubric (machine-generated issues fail Scope; "comment `I'll take this`" in a body is not a claim) and the strategy (drop `[auto]` issues, one result per repo). A third scan accepted Dynamo #31, a repo ten minutes old with one commit, so I added the 5-commit rule. 2026-10-21: the scope row became `Scope is feasible` with a difficulty note.

**Evidence:** From `scan-notes.md`: "Dynamo #31 flips from accept (old text) to reject (new text)... Nothing else flips: community-web-django (37 commits), SmartBench (156), dataquality (300)..." After the scope patch the scan accepted tractusx-sdk#269 with every required check passing, difficulty ambitious. I claimed it and opened PR #290.

**voice-guard**

**What it is, and the judgment hole I wrote:** A `PreToolUse` hook on `gh issue|pr comment|create`. A model call reads the outgoing text against my `voice-guide.md` and stops it, quoting the broken rule. The guide is mine: 21 rules with wrong/right pairs, such as "If a repo's policy requires disclosing AI use, that goes in the comment, plainly, before anything else about the bug" and "One question, answerable from the maintainer's chair".

**How it evolved, dated:** Honestly, the rules did not change after I installed it on 2026-10-21 (when I folded three guide copies into one); every stop quoted a rule I still agreed with, so I fixed the draft and not the rule. What I learned is that it is a model check, so its verdicts vary from run to run.

**Evidence:** `guard-log.md`, 2026-10-21, claim for #269: "14:41 stop: rule broken: 'If a repo's policy requires disclosing AI use, that goes in the comment, plainly, before anything else about the bug'", then "14:42 pass" after I moved the disclosure under the intent line. A second stop caught two bundled questions; the hook passed the real post at 14:54. Over the term: 15 stops and 47 passes in 64 lines.

**pr-precheck and ship-gate (one tool)**

**What it is, and the judgment hole I wrote:** pr-precheck grades a PR (title, description, diff) against its plan and issue with checks like "Diff matches plan" and "AI disclosure present". The ship-gate is a hook on PR creation that runs my manifest's commands, then pr-precheck, and blocks the PR unless both pass. My manifest, `ship-manifest.md` in my clone, is built from the repo's own CI: setup, `pytest`, `ruff check`, `mypy src`, and the SPDX license-header grep.

**How it evolved, dated:** 2026-10-06: in Unit 4 pr-precheck scored 20/20 on its eval, and on my sandbox branch it rejected "Template and policy honored" until I removed a seeded suppression. 2026-11-03: I found my clone was 35 commits behind upstream, which had added an exception hierarchy and `quality.yaml`; I rebased and added the ruff and mypy rows to the manifest. The gate's first real run rejected the PR because it diffs against a stale local `main`; I moved `main` to `upstream/main` and it passed.

**Evidence:** `gate-log.md`, 2026-11-03 18:41, BLOCK, all five manifest checks ok but: "Diff matches plan [fail]: Diff was taken against stale local main: 107 files and ~40 commits, mostly upstream's... none named in the plan." At 18:42, same head commit 18c8007: "verdict: PASS". Only the base changed.

**pr-monitor**

**What it is, and the judgment hole I wrote:** A program I run on demand. It reads my PR's state with read-only `gh` calls, picks one move, and drafts it to `drafted-move.md`; `--state` prints the state with no model call. The judgment is my `playbook.md`, e.g. "I count 7 days of quiet as normal and allow a nudge from day 8", based on outside contributors' PRs here waiting 2 and 7 days to merge.

**How it evolved, dated:** The least history: the runner did not change, only my playbook. I wrote it on 2026-11-04 and trimmed it to two or three rules per section on 2026-11-10. I kept 7 days because two outside PRs are too few to justify another number.

**Evidence:** On 2026-11-04 the canary read PR #290 as "FEEDBACK-IN", though the only comment was SonarCloud's passing Quality Gate; the full run still drafted a wait note. On 2026-11-10 it drafted another and quoted my rule: "A nudge sent now would break your own rule." I sent neither.

## Inventory

- repro-check: grades a claim and repro report before I post. It scored 18/20 on its Unit 2 eval, and on #269 it rejected my first draft because the scripts were linked, not inline, so I pasted them in.
- plan-check: grades a plan and plan comment before posting. It accepted my #269 plan and I logged no friction, so I did not feature it.

## The Arc

**Beat 1**
Date: 2026-09-15
Friction: My first Unit 1 rubric scored 14/20 on the eval (bar 18/20) and rejected sound issues.
Change: I changed `Maintainer alive` from AND to OR and rewrote the scope check to separate an undecided end state from an open approach.
Result: Re-runs went 11/14, 12/12, 8/8, and the final run scored 20/20; I selected sandbox issue #62.

**Beat 2**
Date: 2026-10-06
Friction: My pr-precheck rejected my sandbox branch on "Template and policy honored": I had not removed a seeded `pyproject.toml` suppression.
Change: I removed it, noted the deviation in `plan.md`, and re-ran.
Result: The re-run said accept, and I opened sandbox PR #93.

**Beat 3**
Date: 2026-10-12
Friction: My first scan spent 2 of its 5 slots on machine-filed untether error reports my rubric had no rule for.
Change: I patched the rubric twice and the strategy twice (drop `[auto]` issues, one result per repo).
Result: The re-scan went 300 to 213 to 150 results, the reports did not return, and no verdicts flipped.

**Beat 4**
Date: 2026-10-12
Friction: A third scan accepted Dynamo #31, a repo ten minutes old with one commit.
Change: I required at least 5 default-branch commits for "Maintainer alive".
Result: Dynamo flipped to reject and no other verdict changed.

**Beat 5**
Date: 2026-10-21
Friction: The guard stopped my claim draft twice: disclosure came after the bug, and I had bundled two questions.
Change: I moved the disclosure under the intent line and cut to one question; the rules stayed.
Result: The hook passed the claim at 14:54; after I re-posted it on 2026-10-28 the maintainer replied on 10-29 that lazy initialization was the intended direction.

**Beat 6**
Date: 2026-11-03
Friction: The ship-gate rejected PR #290's package on a 107-file diff caused by a stale local `main`.
Change: I moved `main` to `upstream/main`, moved the AI disclosure under the intent line, and re-ran.
Result: The gate passed at 18:42 on the same commit, and PR #290 opened that evening.

**Beat 7**
Date: 2026-11-10
Friction: The monitor labeled my PR FEEDBACK-IN on a bot comment, and the PR had 6 days of silence.
Change: I trimmed my playbook and ran the full monitor, which drafted a wait note quoting my day-8 rule.
Result: I sent nothing; the earliest nudge is 2026-11-11, and PR #290 is awaiting review.

## Limitations and What's Next

Every tool here checks only what I hand it. The gate could not see that industry-core-hub reads the `connected` attribute my change alters; I found that by searching code. The guard and pr-precheck are model checks, so a pass is evidence, not proof. I have not rerun my Unit 1 harness on the patched scout rubric, and PR #290 has no human review after 6 days, so I cannot say yet whether the toolkit helped it merge.

Next: add a downstream-consumer search to the plan-check or manifest, rerun the Unit 1 harness, make scout's filter 3 an exact-label match, and teach the monitor to tell bots from humans.

**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.

issue-scout: the freshest issues are the likeliest to already have a competing PR, which no filter can see, and filter 3 still lets documentation-labelled issues through.
voice-guard: its verdicts vary; on 2026-10-30 `repro-note.md` was stopped and passed within the same minute, so a pass does not guarantee the next run agrees.
pr-precheck and ship-gate: it diffs against local `main`, so it rejected a good branch until I moved `main` myself, and it cannot see downstream code that reads an attribute my change alters.
pr-monitor: it cannot tell a bot's comment from a human review, so it labeled my PR FEEDBACK-IN on a passing SonarCloud summary.
