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
**Pull Request:** https://github.com/eclipse-tractusx/tractusx-sdk/pull/290

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

I patched my rubric's scope row (it became `Scope is feasible`, with a difficulty note: straightforward, moderate or ambitious) before the scan, folded my three voice-guide copies into one file at `~/.claude/voice-guard/voice-guide.md`, wrote six upstream rules (claims, questions, pings, each with a wrong/right pair), installed the guard as a `PreToolUse` hook, and swapped repro-check's and plan-check's scopes for the wild ones. The hook's wiring test (`gh issue comment --help`) was stopped with "no body the guard can read", and the guard then stopped my own claim draft twice, once because the AI disclosure sat last instead of before anything else about the bug, once because I had bundled two questions into one. I fixed the draft each time instead of the rules, the claim posted with a `[hook] pass` in `guard-log.md`, and the patched rubric's scan accepted my chosen issue. The scan's first headless launch also stalled on a `gh` permission prompt, and the old claim draft from my sandbox work would have been stopped too (no "I'll say so if I stop"). I deleted that first claim the day I posted it and re-posted the same guarded draft a week later (2026-10-28) after the scan showed the issue still unclaimed; the maintainer then replied and confirmed the direction, so the claim loop worked once it stayed up. I wrote the repo memory (`CLAUDE.local.md`, 32 lines, kept out of the PR with `.git/info/exclude`) after one scoped read of the OAuth2 manager, its tests and CONTRIBUTING.md, and it already earned its place: the repo ships no CLAUDE.md or AGENTS.md, and the notes record the ECA-email, license-header and CHANGELOG rules, the 14-test command I verified, and the repro numbers, so a new session starts with them instead of rediscovering them. Verifying the notes also caught that my git email must match the ECA account.

**Toolkit**

- Tool: voice-guard (`check_voice.py` as a hook plus the `--draft` manual doorway), reading my canonical voice guide; I ran it through Claude Code, with repro-check grading the claim draft first.
- What it produced: two stops that quoted my own rules (AI disclosure before the bug; one question per question), then a pass, and a dated line for each in `guard-log.md`, including the hook pass on the real post.
- Difficulties: the guard stopped a draft that repro-check had already accepted, so the two checks do not agree on strictness; the disclosure rule also binds even where the repo states no AI policy. A headless scan and grader needed explicit `gh` and read permissions before they would run.
- What changed: I reordered the claim (disclosure under the intent line) and cut it to one question, with the exception type and 503 default stated instead of asked. The rules themselves stayed as written. The repo memory joined the entry once written (see above).

**Unit 7: ship-gate on the PR door, with a manifest of my repo's own CI checks**

I installed the ship-gate as a second `PreToolUse` hook on `gh pr create` and wrote `ship-manifest.md` from the repo's workflows (setup, pytest, `ruff check`, `mypy src`, the SPDX license-header grep), with an empty excluded table because nothing failed that I wanted to excuse. Its first run on my real branch rejected the PR: pr-precheck saw a 107-file diff, which was my local `main` being stale against upstream, not my change; I moved local `main` to `upstream/main` and the re-run passed. The second thing it stopped was my PR description, which had the AI disclosure at the bottom instead of right after the intent line, so I moved it, re-ran the gate (PASS, all five checks ok), pushed the branch to my fork and set `gh repo set-default` to the original repo. The hook then let `gh pr create` through within its 30-minute window and PR #290 opened on `eclipse-tractusx/tractusx-sdk`.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my Unit 5 strategy: three queries that each trace to a sentence of my fit profile (Python, backend and API work, bugs with a runnable repro), after I patched the rubric's scope row so open edges inform my choice instead of rejecting the issue. The scan graded five candidates, all from the `good first issue` query: three accepts and two rejects, and both rejects already had an open PR on them, which is the "nobody already on it" check doing its job. My issue, eclipse-tractusx/tractusx-sdk#269 ("Synchronous Keycloak initialization in OAuth2Manager blocks host application startup"), was accepted with all required checks passing and a difficulty note of **ambitious**: the issue itself offers two designs (lazy initialization on the first request, or a background thread), and its `refine-me!` label says the maintainers consider it unrefined. I chose it by the three lenses. Fit: it is a Python SDK's startup and config path with a clear repro (an unreachable Keycloak URL under a minimal FastAPI host), which is my lane. Energy: I would happily work on it as long as it is solvable, and it looks solvable. Thread: open, no comments, no assignee, no linked PR when I checked. Of the other two accepts, ma-lit-bills#20 was easier and I considered switching, but I wanted the harder problem, and the open design edge became the first question in my claim instead of a reason to avoid the issue. I did not choose against my scout: its verdict was accept.

I read the repo's rules before claiming. CONTRIBUTING.md is Eclipse boilerplate whose one hard gate is signing the Eclipse Contributor Agreement before a PR; CI requires SPDX license headers on new files; the repo states no AI policy, so I disclosed AI assistance in the claim under the course's practice. Claim posted 2026-10-28: https://github.com/eclipse-tractusx/tractusx-sdk/issues/269#issuecomment-6479455742 (I posted the same claim on 2026-10-21 and deleted that comment, so this is the live one; the thread was still unclaimed when I re-posted).

Honest read of the tool: the scope patch did what it was meant to, since the scan accepted three issues instead of none, but it also accepted Conclave#59 (a screenshot and two sentences) on scope, and the patched row no longer carries my 2026-10-12 clauses on machine-generated reports, abandoned PRs and undecided end states, so I will watch for verdicts those clauses would have rejected. The scan also let documentation-labelled issues through filter 3 (exact-label matching, which a substring match would fix), and the freshest issues were the most likely to have a competing PR already, which no ranking filter can see. I have not patched the strategy for either yet, and I have not rerun my Unit 1 harness against the patched rubric.

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric, after the scope patch), launched as `claude "issue-scout: run a scan"`; I re-ran it once with `gh` allowed after the headless run stalled on a permission prompt. The scan is saved in `~/.claude/skills/issue-scout/scan-output.md`.
- What it produced: five graded candidates, three accepts and two rejects; #269 accepted with every required check passing, difficulty ambitious, and the preferred newcomer check passing too.
- Difficulties: the freshest candidates raced open PRs, filter 3 let documentation-labelled issues through, the scan could not write its own friction notes, and Conclave#59 passed scope on very thin evidence.
- What changed: I logged the choice and the scope patch in `scan-notes.md`. The substring-match fix for filter 3 and a harness regression run are still to do.

---

## Understanding the Issue

**Problem Description**

`OAuth2Manager` fetches Keycloak's OIDC configuration from inside its constructor, with a blocking network call. If the host application builds the manager during startup and Keycloak is unreachable or slow, the host cannot start: it never binds its port or serves health checks.

**Expected Behavior**

The SDK should not block the host's startup. The Keycloak connection should be made lazily, on the first authentication request (the maintainer confirmed this direction), and if Keycloak is unreachable then, the SDK should raise something the host can turn into a 503 or 401 instead of failing at boot.

**Current Behavior**

On `main`, constructing `OAuth2Manager` against a Keycloak that accepts connections but never answers blocks about 120 s and then raises `KeycloakConnectionError`; a minimal FastAPI host never binds its port in that time. A refused connection fails in under 0.1 s. The block is bounded, not indefinite as the issue says.

**Affected Components**

`src/tractusx_sdk/dataspace/managers/oauth2_manager.py` (`__init__`, `connect`, `get_token`, `add_auth_header`, `is_authenticated`); `DiscoveryFinderService.find_discovery_urls`, which reads `oauth.connected` as a precondition; the 14 tests in `tests/dataspace/manager/test_oauth2_manager.py` and one in `tests/dataspace/services/test_discovery_services.py`; the `connected` documentation in `docs/core-concepts/authentication-security/authentication.md`; and, because upstream added an exception hierarchy while I worked, `src/tractusx_sdk/exceptions.py` with its test and its table in `docs/admin/logging.md`.

**Toolkit**

- Tool: Claude Code reading the issue, the code and the docs, with my `CLAUDE.local.md` repo memory loaded; no course tool graded this section.
- What it produced: the four answers above, plus the finding that `find_discovery_urls` and the docs both depend on `connected`, which the issue never mentions.
- Difficulties: none met; the dependency on `connected` only showed up because I searched for every reader of the flag instead of stopping at the file the issue names.
- What changed: that finding widened my plan from one file to a handful (see Solution Approach).

---

## Reproduction Process

**Environment Setup**

macOS (Darwin 23.4.0, arm64), Python 3.12.12 in a fresh venv made with `uv venv --python 3.12`. I cloned eclipse-tractusx/tractusx-sdk at `main` (commit c55dee9, 2026-10-28, version 0.8.4rc1, ahead of the latest release v0.8.3) and ran `uv pip install ./tractusx-sdk`, which pulled `python-keycloak` 4.7.3, `fastapi` 0.143.5, `requests` 2.34.2 and `uvicorn` 0.36.0. No Keycloak server is needed, only a URL that does not answer. Challenges: the repo's own tool is poetry, which I do not have, so I installed with uv instead; and the issue says "hangs indefinitely", so before running anything I read `python-keycloak`'s defaults (`timeout=60`, `max_retries=1`) to know what to expect.

**Steps to Reproduce**

1. Start a stand-in for a Keycloak that is up but stuck: a socket that accepts connections and never answers (`python silent_server.py 18080`; the three scripts are pasted in full in `repro-draft.md`).
2. Time `OAuth2Manager(auth_url=..., realm="r", clientid="c", clientsecret="s")` called directly, once against a closed port (`http://127.0.0.1:9/`) and once against `http://127.0.0.1:18080/`.
3. Build `OAuth2Manager` at module level in a minimal FastAPI app (`host.py`), start it with `KC_URL=http://127.0.0.1:18080/ uvicorn host:app --port 18000`, and poll `curl http://127.0.0.1:18000/health`.
4. Observed: the closed port fails in under 0.1 s; the silent listener blocks the constructor for about 120 s and then raises; the host never binds its port, prints no startup message, and exits after about 121 s.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the scripts and raw output live in `repro-269-evidence/` next to my drafts, and the PR description will carry the proof).
- **Screenshots/logs:** direct call against the silent listener: `120.03s  raised keycloak.exceptions.KeycloakConnectionError`; against the closed port: `0.04s  raised keycloak.exceptions.KeycloakConnectionError`. FastAPI host: no `Application startup complete`, `/health` never answered, process exited after 121 s with a traceback through `OAuth2Manager.__init__` -> `connect()` -> `keycloak_openid.well_known()` (full output in `out-host2.txt`).
- **My findings:** the symptom in the issue is real: constructing `OAuth2Manager` blocks the host's startup on a network call, and a host with a stuck Keycloak cannot come up for about two minutes. It is bounded, not the "indefinite" hang the issue describes; 120 s is the library's 60 s timeout times two attempts; I have since confirmed in the installed library that the session retries once (`Retry(total=1)`), which I had not done when I posted my repro note. A refused connection fails fast, so only an unresponsive Keycloak blocks. Not tested: a real Keycloak, a non-routable IP, the SDK's other auth paths. The maintainer has already confirmed lazy initialization as the direction.

**Toolkit**

- Tool: repro-check, run headless with `claude -p "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue <URL>"` plus read access to my skill folders; I also ran my voice-guard on the draft first.
- What it produced: a first-round **reject** on "Steps followable" because the report said the scripts were "available if useful" instead of containing them, then an **accept** (all six checks pass) once I pasted the three scripts inline; it also graded the claim and report together and checked my numbers against the raw output files.
- Difficulties: the headless run stalled until I allowed it to read my skill folders; repro-check could not run the scripts itself, so it graded the report against my saved output and I re-ran the exact posted scripts myself to confirm the numbers (0.04 s and 120.03 s); and the sandbox-era check does not know the claim promised to post the repro on the thread while this course step says not to.
- What changed: the report now carries its scripts and exact commands; I noted the unconfirmed retry setting and the untested cases in the report instead of leaving them out.

---

## Solution Approach

**Analysis**

`OAuth2Manager.__init__` (`src/tractusx_sdk/dataspace/managers/oauth2_manager.py`) calls `self.connect(...)`, and `connect()` calls `self.keycloak_openid.well_known()`, a blocking HTTP request (`python-keycloak` 4.7.3: `timeout=60`, and the session's `Retry` has `total=1`, which I checked in the installed library, so a silent server gets two 60 s attempts). So any host that builds the manager at startup waits on Keycloak before it can bind its port. My reproduction on `main` (c55dee9, and the file is unchanged in upstream `main` 9a7f6e5): against a Keycloak that accepts connections and never answers, the constructor blocks about 120 s and then raises `KeycloakConnectionError`, and a FastAPI host never starts; a refused connection fails in under 0.1 s. The three methods that use the connection (`get_token`, `add_auth_header`, `is_authenticated`) all guard on `self.connected`. One more reader of that flag matters: `DiscoveryFinderService.find_discovery_urls` (`src/tractusx_sdk/dataspace/services/discovery/discovery_finder_service.py`) refuses to run unless `oauth.connected` is True, and `docs/core-concepts/authentication-security/authentication.md` documents `connected` and tells users to check it. Moving the connection out of the constructor therefore changes what `connected` means before first use, and those two places have to follow. Upstream also added an exception convention while I was working (`src/tractusx_sdk/exceptions.py`): every error the SDK raises on purpose derives from `TractusXSDKError`, and where it replaces a builtin such as `ConnectionError` it inherits from both.

**Proposed Solution**

Lazy initialization, as the maintainer confirmed on the issue:

- `__init__` stores the connection parameters (and `clientid`/`clientsecret`), sets `connected = False`, and makes no network call.
- A new `AuthServiceUnavailableError(TractusXSDKError, ConnectionError)` in `exceptions.py`, so existing `except ConnectionError` code keeps working and the SDK convention is followed. This changes what I said on the issue ("the builtin `ConnectionError`, open to a dedicated type"), because the convention landed upstream after I wrote that.
- A private `_ensure_connected(action)` connects on first use. If already connected it returns. If no parameters were stored (a manager built without `__init__`, as several existing tests do) it raises the same `RuntimeError("Not connected. ...")` as today. Otherwise it calls `connect()` and turns `KeycloakConnectionError`, or the `ConnectionError` that `connect()` raises for an empty well-known response, into `AuthServiceUnavailableError` (chained). A failed attempt leaves `connected` False, so the next call retries.
- `get_token`, `add_auth_header` and `is_authenticated` call `_ensure_connected` in place of their direct "not connected" raise.
- `is_authenticated` maps `AuthServiceUnavailableError` to `HTTPException(503)`, since the method already speaks HTTP (it raises 401 for bad tokens) and here the fault is Keycloak, not the caller's credentials.
- `find_discovery_urls` drops its `connected` precondition: `add_auth_header` now connects on demand and raises the new error itself.
- Docs: `authentication.md` (the `connected` row and the "check connected" advice) and the exception table in `docs/admin/logging.md`.

Deliberately not changing: the public `connect()` signature and behavior (an explicit `connect()` still works and still raises what it raised); the 60 s timeout and retry settings (the first request after a Keycloak outage can still block for up to about two minutes, which is a separate change); no background thread; no other managers; no CHANGELOG edit unless the maintainers want one. Open for the maintainer: whether `connected` staying False until first use is acceptable for hosts that read it, and the 503 mapping.

**Implementation Plan**

1. `exceptions.py`: add `AuthServiceUnavailableError`; add it to `tests/test_exceptions.py` and to the exception table in `docs/admin/logging.md`.
2. `oauth2_manager.py`: change `__init__` (it also stores `clientid` and `clientsecret`, which `get_token` reads and only `connect()` used to set); add `_ensure_connected`; use it in the three methods; add the imports.
3. `discovery_finder_service.py`: remove the `connected` precondition in `find_discovery_urls`.
4. Tests in `tests/dataspace/manager/test_oauth2_manager.py`: constructing never contacts Keycloak (mocked, `well_known` not called); constructing against a closed port (`http://127.0.0.1:9/`) returns immediately (this fails on `main` and is the regression test for the issue); the first `get_token` connects once; an unreachable Keycloak raises `AuthServiceUnavailableError` from `get_token` and the next call retries; an empty well-known response on first use raises it too; `is_authenticated` returns 503 when Keycloak is unreachable. Update `test_find_discovery_urls_not_connected` in `tests/dataspace/services/test_discovery_services.py` to the new behavior.
5. Update `docs/core-concepts/authentication-security/authentication.md`.
6. Run the manifest checks (tests, ruff, mypy, license headers); re-run my reproduction on the branch and show that the host binds its port at startup.

*Deviations (2026-11-03):* (a) three existing tests failed because they build the manager, set `connected = True` by hand and then call `get_token`, which reads `self.clientid`; I store the credentials in `__init__` as well. (b) After I had written the plan, I found upstream `main` had moved 35 commits and added an exception hierarchy; I rebased onto it (no conflicts) and replaced the builtin `ConnectionError` with `AuthServiceUnavailableError` (step 1 above, and the new empty-well-known test). Nothing else changed from the plan.

**Toolkit**

- Tool: plan-check, run headless as `claude -p "plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue <URL>"`, plus my voice-guard on the comment draft. I did not post the comment.
- What it produced: an accept on the first run (all four required checks pass), with two things I would have missed: the plan and comment both stated "one retry" as fact while my posted repro said I had not confirmed it, and the comment's disclosure line ("I checked every claim myself") was false for that claim. I then checked the library (`Retry(total=1)`, `timeout=60` in `python-keycloak` 4.7.3) and replaced the hedge with the verified fact.
- Difficulties: my voice guard, a model check, stopped the comment on two different rules across reruns (it flagged the exception choice as stated as decided; then, on later runs, passed the same text three times), so I fixed the wording to name the change from my claim and ran it repeatedly before trusting a pass. The plan-check scope also had to be the wild one from unit 6, or it would have asked for a Path Review link.
- What changed: `plan.md`, `plan-comment.md` and this section carry the verified retry fact; the exception choice now says it changes my claim's "dedicated exception" and why; and my repo notes now say no CHANGELOG entry unless the maintainers ask, so the two drafts and the notes agree.

---

## Testing Strategy

**Before and after, on my reproduction.** On `main` (c55dee9; `oauth2_manager.py` is unchanged in upstream), against a Keycloak that accepts connections but never answers, `OAuth2Manager(...)` blocked about 120 s and the FastAPI host never bound its port or answered `/health` (it exited after 121 s). On the rebased branch the constructor returns in 0.00 s in the same setup, the host answers `GET /health -> {"status":"ok"}` four seconds after starting, and the first use of an unreachable Keycloak (closed port) raises `AuthServiceUnavailableError` (a `ConnectionError`, chained from `KeycloakConnectionError`) in 0.06 s with `connected` still False, so the next call retries (0.02 s, same error).

**New tests** in `tests/dataspace/manager/test_oauth2_manager.py`: the constructor returns against a closed port (`http://127.0.0.1:9/`, the regression test for this issue), the constructor never contacts Keycloak, the first `get_token` connects exactly once, an unreachable Keycloak raises `AuthServiceUnavailableError` and the next call retries, an empty well-known response on first use raises it too, and `is_authenticated` raises HTTP 503. `tests/test_exceptions.py` also checks the new class is a `ConnectionError` and is caught by a single `TractusXSDKError` clause. One existing test in `tests/dataspace/services/test_discovery_services.py` changed to the new contract. Against the unfixed code, 6 of these fail; the first-use-connects-once test passes on both sides and guards the happy path.

**The repo's checks** (my ship manifest, run in a fresh shell on the rebased branch): the full suite gives 975 passed, 1 skipped; `ruff check` passes; `mypy src` reports no issues in 178 files; the license-header check passes. Also run, outside the manifest: `xenon` passes and the flake8 serious-errors check reports 0. Not tested: first use against a silent Keycloak (still about 120 s by design, since I did not change the timeout), and a real Keycloak server. The full output is in `test_evidence.md` in my clone.

**Toolkit**

- Tool: pytest, ruff and mypy from my ship manifest, run by hand in a fresh shell with no venv active; the ship-gate itself has not been run on the branch yet (that is the next step).
- What it produced: the before and after reproduction output, the 4-of-5 new tests failing on `main`, 813 passed on the branch, and `test_evidence.md` collecting it.
- Difficulties: the first manifest run failed on `main` before any change (a missing test dependency, `moto`), and flake8 failed on the old base; and I had not rebased before building, so I only learned days later, when I checked, that upstream had moved 35 commits and added `ruff` and `mypy` jobs to CI that my manifest did not run.
- What changed: `moto` went into the setup row, and after the rebase the manifest got `ruff check` and `mypy src` rows and lost the flake8 exclusion (upstream fixed that error); I now fetch `upstream` before planning or checking, instead of trusting my first clone.

---

## Implementation Notes

**2026-11-03**

Plan accepted by plan-check; not posted, as the course step says. Created `fix/269-lazy-oauth2-init`. Wrote the five new tests first and ran them on the unfixed code: 4 failed, as intended. Then changed `oauth2_manager.py`: `__init__` stores the parameters, a private `_ensure_connected()` connects on first use, `get_token`, `add_auth_header` and `is_authenticated` call it, and `is_authenticated` maps `ConnectionError` to HTTP 503. Wrong turn: three existing tests failed because `get_token` read `self.clientid`, which only `connect()` set; I now store the credentials in `__init__` too (recorded as a deviation in the Implementation Plan). Commit 786915e.

**2026-11-03**

Ran the discovery tests after the manager change: exactly one failed, `test_find_discovery_urls_not_connected`, confirming what my search for readers of `connected` had found. Dropped the precondition in `find_discovery_urls` and rewrote that test to check that the manager's `ConnectionError` propagates (commit 2f52b01). Updated `authentication.md`: the `connected` row, the error-handling advice and the "Not connected errors" tip, and removed an example that checked `oauth_manager.connected` before first use and would have always bailed out (commit 975ee2d). Kept the CHANGELOG untouched, as planned. Re-ran my reproduction on the branch and the manifest checks and saved them in `test_evidence.md`.

**2026-11-03**

Checking whether my clone was current (it was from Oct 28), I found upstream `main` 35 commits ahead, including a new exception hierarchy (`exceptions.py`) whose rule is that errors the SDK raises on purpose derive from `TractusXSDKError` and, where they replace a builtin like `ConnectionError`, also inherit it. I tested a rebase on a throwaway branch (clean, 972 tests passing), then rebased my branch and added `AuthServiceUnavailableError(TractusXSDKError, ConnectionError)` in place of the bare builtin, with its tests and a row in the exception table in `docs/admin/logging.md`. I also added a test for an empty well-known response, rebuilt the history as four commits in dependency order (exception class, manager, discovery service, docs), added `ruff` and `mypy` to the manifest, and re-ran my reproduction and all checks. Updated `plan.md` and the Implementation Plan to say why the exception type differs from the posted plan note.

**Toolkit**

- Tool: Claude Code, working from `plan.md` and my `CLAUDE.local.md` repo memory, with the test-first order enforced by me; three focused commits.
- What it produced: the fix in `oauth2_manager.py`, six tests, the new exception class, the `find_discovery_urls` change and its test, the docs updates, and `test_evidence.md`.
- Difficulties: the existing tests' hidden dependency on `connect()` setting `clientid`, caught only by running the old suite against the new code; and one shell slip (BSD `sed` rejected a GNU-style edit) that I redid with a script.
- What changed: the credentials are stored in `__init__`, the exception is now `AuthServiceUnavailableError` after a rebase onto current upstream, both deviations are recorded in the plan and the story, and the memory file now points at `plan.md` and says to fetch `upstream` first.

---

## Pull Request

**PR Link:** https://github.com/eclipse-tractusx/tractusx-sdk/pull/290

**PR Description:** Title: "Connect OAuth2Manager to Keycloak lazily so it does not block host startup". `OAuth2Manager` no longer contacts Keycloak in its constructor and connects on first use instead, following the lazy-initialization direction the maintainer suggested on the issue. The description opens with my AI disclosure (Claude Code), then gives before/after evidence from my reproduction (a Keycloak that never answers used to block construction for about 120 s; now construction returns immediately), a new `AuthServiceUnavailableError` (503 from `is_authenticated`), the removed `connected` precondition in `find_discovery_urls`, the docs changes, and FURTHER NOTES on the behavior change (`connected` is False until first use, with industry-core-hub as an example reader), what is not in the PR (CHANGELOG, a lock), and the three reproduction scripts in a collapsed section. `Fixes #269`.

**Maintainer Feedback:**

- 2026-11-03: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate (`check_gate.py` as a `PreToolUse` hook on `gh pr create`, which runs my manifest and then pr-precheck), run by hand first and then through Claude Code on the real `gh pr create`.
- What it produced: a PASS in `gate-log.md` after the five manifest checks and a pr-precheck accept, and a PR that opened without a second gate run.
- Difficulties: the first run rejected on a 107-file diff, because it compares against local `main`, which was stale; it also held my disclosure to the same place rule as the voice guard, so one more edit to `pr_draft.md` was needed.
- What changed: I moved local `main` to `upstream/main`, moved the disclosure under the intent line, and now fetch upstream before any gate run.

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
