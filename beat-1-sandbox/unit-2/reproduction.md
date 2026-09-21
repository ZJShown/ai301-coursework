# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Your identity upstream

**GitHub username**

ZJShown

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5768136979

Hey, I'm a TF from CodePath's AI 301 course. I'd like to take on this
issue as part of the TF weekly prep task. Looking at the `Settings`
class in `core/config.py` and the Redis check inside `health_check()`
in `api/routes/health.py`, it appears that the health check builds its
Redis client from `settings.redis_host` and `settings.redis_port`, but
`Settings` only defines a `redis_url` field — no `redis_host` or
`redis_port` attribute exists on it, so the client construction itself
raises an `AttributeError` before any real connection is attempted. I
will reproduce the bug locally using `docker compose` and `uvicorn`
and report back what I find.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5768152699

Reproduced this on a fresh clone. Environment: Python 3.13.5, macOS,
backing services via `docker compose up -d` (`postgres:16-alpine`,
`redis:7-alpine`), app run with `uvicorn api.main:app --host 0.0.0.0
--port 8000` after `make setup`.

Before hitting the endpoint, confirmed Redis was actually reachable:

```
$ docker exec pathreview-ai301-fa26-s1-redis-1 redis-cli ping
PONG
```

Then:

```
$ curl -s -w "\nHTTP_STATUS:%{http_code}\n" http://localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-21T21:46:17.491283"}}
HTTP_STATUS:503
```

Server log for that request:

```
redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
```

So this confirms the hypothesis from my claim comment: `redis` reports
`unhealthy` even though Redis itself is up and answering `PONG`,
because `health_check()` in `api/routes/health.py` builds its client
from `settings.redis_host`/`settings.redis_port`, and `Settings` in
`core/config.py` only defines `redis_url`. The `AttributeError` fires
while constructing the client, before any connection attempt, and gets
swallowed by the route's broad `except Exception` — so the check
can't ever report Redis healthy, regardless of whether it actually is.
Confirmed directly, outside the API too:

```
$ python -c "from core.config import settings; settings.redis_host"
AttributeError: 'Settings' object has no attribute 'redis_host'
```

Expected: with Redis reachable, `GET /health` should report
`"redis": "healthy"`.

Actual: `"redis": "unhealthy"` unconditionally, for the reason above.

(The `"postgres": "unhealthy"` in the same response is a separate,
already-known issue in this route — unrelated to this one, noting it
only so it isn't mistaken for evidence here.)

Next step for me is looking at whether the fix should add
`redis_host`/`redis_port` fields to `Settings`, or just have the
health check build its client from `redis_url` directly — will follow
up with a PR proposal.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3` (pkg-01, pkg-02, pkg-03): **3/3**.
2. First full run, same rubric: **19/20 scored items (bar: 18/20 — PASS)**. One
   disagreement: `pkg-05` (gold `accept`, graded `reject`; failed `Steps followable`,
   `Comms respect the repo`).
3. Partial re-run, `--only pkg-05`, diagnosing that disagreement without changing the
   rubric: **1/1** — agreed (`accept`), all six checks passed with a named evidence quote
   for each. Read against the isolated run's evidence, the full-run disagreement looked
   like grader variance (the grading model is not deterministic) rather than a gap in the
   check's pass condition, so no rubric change was made.
4. Full confirming run, same rubric, saved with `--save-run`: **18/20 scored items (bar:
   18/20 — PASS)**. Two disagreements this time, neither the same as run 2's: `pkg-05`
   again (this run failing only `Comms respect the repo`) and `pkg-09` (gold `accept`,
   graded `reject`; failed `Steps followable`, `Behavior shown matches the issue` — this
   package had agreed cleanly in run 2). Different packages and different failed checks
   across otherwise-identical runs is further evidence this is grader noise on a couple of
   borderline packages, not a systematic hole in the rubric; the category floor held in
   every full run.

The last score (18/20) matches the `agreement:` line in the committed `eval-run.txt`.

**Package analysis**

`pkg-20` (source `ghostty-org/ghostty#13604`). Gold: `"category": "disclosure",
"verdict": "reject", "note": "excellent repro on every proof check; ghostty's stated AI
policy requires disclosing all AI usage and the comments do not disclose ... the one-item
category the floor exists for"`. My rubric's decision: **reject**, matching gold.

The bundle's repo-facts block states ghostty's policy plainly: `"contribution policy
(CONTRIBUTING.md + AI_POLICY.md): strict AI rules. All AI usage in any form must be
disclosed, stating the tool used and the extent of the assistance..."`. The candidate
claim and repro comments in the bundle are genuinely strong on every other check —
environment, steps, and behavior all read as faithful and complete — but neither comment
contains any disclosure statement. My `Comms respect the repo` check's pass condition
reads: `"When the contribution/AI policy requires disclosing AI assistance, the comments
name the tool and the extent of its use; when the policy is silent ... no disclosure
statement is required to pass."` Since the policy here is not silent, and no disclosure is
present, this check fails, which by my verdict rule (any required check failing rejects
the package) is enough by itself to reject a package that would otherwise pass everything
else. That is exactly what the one-package `disclosure` category is built to force a rubric
to catch, and it is the only package in the set where this specific clause is load-bearing.

**Check rationale**

Check quoted verbatim from the `rubric.md` uploaded to `tools/repro-check/`:

> **Comms respect the repo** | The claim comment's and repro report's own wording, read
> against the repo-facts block's "bug reports" template line and "contribution policy"
> line (including any named AI-policy file). | Both comments are specific to this issue —
> they could not be pasted unchanged onto a different one — and promise only what the
> poster can keep (no guaranteed fix timelines, no reserving the issue by tone alone); the
> repro report answers what the repo's stated template asks. When the contribution/AI
> policy requires disclosing AI assistance, the comments name the tool and the extent of
> its use; when the policy is silent, or merely conditions AI use without requiring
> disclosure, no disclosure statement is required to pass. Fails when a comment is
> interchangeable boilerplate, promises an outcome or timeline it cannot back, or when the
> repo's policy requires AI disclosure and the comments give none. | required

Reasoning behind its current form: I considered giving AI disclosure its own separate
required check, since it is conceptually distinct from boilerplate-detection. I rejected
that in favor of one combined check because the eval set's own category definitions group
them together as one family — `unfollowable-comms` includes both "no environment record"
*and* "a boilerplate over-promising comment," and `disclosure` is really a specific way a
comment can fail to respect a repo's stated conventions, not a different family of proof
entirely. Splitting it out would have meant a check that fires on exactly one package in a
twenty-package set purely to isolate it from a closely related one; keeping them together
matches the evidence guide's own "Comms" heading, which already names "the comments
against the repo's stated templates and contribution policy (including AI-use disclosure
requirements)" as one thing to check, not two.

**Trade-offs**

Merging boilerplate/over-promising and AI-disclosure into one required check means a
package can fail `Comms respect the repo` for either reason, and the failed-checks note in
the run output doesn't say which — only the per-check evidence line does. I confirmed this
doesn't cost accuracy on this eval set: `pkg-19`'s evidence for that check names the
boilerplate/guaranteed-timeline problem specifically, and `pkg-20`'s names the missing
disclosure specifically, even though both are reported under the same check name. The case
this trade-off would actually cost me: if I wanted to *count* how many packages in a larger
set fail specifically on disclosure (say, to report a disclosure-compliance rate
separately from a comms-quality rate), I'd have to read each evidence string instead of
just tallying failed-check names, since the two are folded together. For a 20-package
course eval set that's a minor, accepted cost; it would stop being minor on an eval set
built around many disclosure-policy repos at once.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
