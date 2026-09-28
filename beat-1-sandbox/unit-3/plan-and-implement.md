# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

## Posted upstream

**GitHub username**

ZJShown

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5876517744

I reproduced this in my repro comment above: `health_check()` builds
its Redis client from `settings.redis_host`/`settings.redis_port`,
which `Settings` never defines (only `redis_url`), so the client
construction itself raises `AttributeError` before any real connection
attempt, and the route's broad exception handler reports it as
`"redis": "unhealthy"` regardless of whether Redis is actually up.

Plan: build the client from `settings.redis_url` via
`redis.Redis.from_url(settings.redis_url, decode_responses=True)`
instead of the nonexistent host/port fields — this matches how every
other Redis client in this codebase is constructed (from a URL, or
injected pre-built), so it's the smaller and more consistent fix
compared to adding `redis_host`/`redis_port` fields that nothing else
here uses. One-line change in `api/routes/health.py`'s Redis check
block, plus a new `tests/unit/test_health.py` covering the healthy and
unhealthy cases. Test: re-running my repro steps above, expecting
`"redis": "healthy"` with Redis reachable and no `redis_host`
attribute error in the log. The separate `"postgres": "unhealthy"`
in the same response (issue #61) is unrelated and out of scope here.

One open item I'll confirm before opening the PR: whether
`redis.Redis.from_url` takes `decode_responses` the same way the
direct constructor call did on the installed `redis` version — I
expect it does, but haven't verified yet.

## Your branch

**Branch**

`fix/62-redis-health-check-url`

https://github.com/ZJShown/pathreview-ai301-fa26-s1/tree/fix/62-redis-health-check-url

Commit: `898fb4ee495361d36b2a3ff76be6978cb9500b05`; claimed issue: #62.
The plan is in this course repo at `beat-1-sandbox/unit-3/plan.md`.

**Evidence**

These are the original September 28 before/after captures, recovered from the
implementation session and checked against the saved server logs. Both used the
same running Postgres/Redis containers after `docker compose up -d`, with the app
started using `uvicorn api.main:app --host 0.0.0.0 --port 8010` in the project's
virtual environment. Port 8010 kept the existing development server on 8000 undisturbed.

Before: the implementation session temporarily stashed the fix to test the original route.

```text
$ docker exec pathreview-ai301-fa26-s1-redis-1 redis-cli ping
PONG
$ curl -s -w "\nHTTP_STATUS:%{http_code}\n" http://localhost:8010/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-28T19:12:27.660293"}}
HTTP_STATUS:503
$ tail -8 /tmp/pathreview-before.log
2026-09-28 15:12:17,735 INFO sqlalchemy.engine.Engine COMMIT
2026-09-28 15:12:17 [info     ] application_startup_completed
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8010 (Press CTRL+C to quit)
2026-09-28 15:12:27 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=5fabac74-403f-45ec-9a49-37e5400730bd
2026-09-28 15:12:27 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=5fabac74-403f-45ec-9a49-37e5400730bd
2026-09-28 15:12:27 [debug    ] vector_db_health_check_passed  request_id=5fabac74-403f-45ec-9a49-37e5400730bd
INFO:     127.0.0.1:55300 - "GET /health HTTP/1.1" 503 Service Unavailable
```

After: the fix was restored and the app restarted on the same port with the same services.

```text
$ docker exec pathreview-ai301-fa26-s1-redis-1 redis-cli ping
PONG
$ curl -s -w "\nHTTP_STATUS:%{http_code}\n" http://localhost:8010/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"healthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-28T19:13:10.173146"}}
HTTP_STATUS:503
$ tail -8 /tmp/pathreview-after.log
2026-09-28 15:12:57,165 INFO sqlalchemy.engine.Engine COMMIT
2026-09-28 15:12:57 [info     ] application_startup_completed
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8010 (Press CTRL+C to quit)
2026-09-28 15:13:10 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=e4d1ac46-862d-49e7-9733-99f984d42e5b
2026-09-28 15:13:10 [debug    ] redis_health_check_passed      request_id=e4d1ac46-862d-49e7-9733-99f984d42e5b
2026-09-28 15:13:10 [debug    ] vector_db_health_check_passed  request_id=e4d1ac46-862d-49e7-9733-99f984d42e5b
INFO:     127.0.0.1:55306 - "GET /health HTTP/1.1" 503 Service Unavailable
```

Redis changes from `"unhealthy"` to `"healthy"`, and the missing-attribute error
becomes `redis_health_check_passed`. HTTP 503 and Postgres remaining unhealthy
are expected because the separate SQLAlchemy issue #61 is outside this fix.

Regression tests, captured during the implementation session:

```text
$ .venv/bin/python -m pytest tests/unit/test_health.py -v
collected 3 items

tests/unit/test_health.py::TestHealthCheck::test_redis_client_built_from_redis_url PASSED [ 33%]
tests/unit/test_health.py::TestHealthCheck::test_reports_healthy_when_redis_ping_succeeds PASSED [ 66%]
tests/unit/test_health.py::TestHealthCheck::test_reports_unhealthy_when_redis_ping_fails PASSED [100%]
======================== 3 passed, 4 warnings in 1.82s =========================
```

The four warnings were existing Pydantic configuration and `datetime.utcnow()`
deprecations. Lint also passed:

```text
$ .venv/bin/ruff check api/routes/health.py tests/unit/test_health.py
All checks passed!
```

## Eval iterations

**Run history**

Recovered from the original harness outputs, in order:

1. Smoke run, `--limit 3`: `agreement: 3/3 scored items`.
2. First full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.
   Only `pkg-14` disagreed: gold `accept`, rubric `reject`, failing `Diagnosis grounded`.
3. After revising the diagnosis check and evidence guide, targeted run with
   `--only pkg-14,pkg-01,pkg-07,pkg-11,pkg-16,pkg-02,calib-03 --include-calibration`:
   `agreement: 6/6 scored items`; `calib-03` also matched (`reject`) but was not scored.
4. Confirming full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.
   All category tallies matched: clear-accept 7/7, scope-creep 4/4,
   thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4.
   This run saved detailed JSON, but did not use `--save-run`.
5. Later attempt with `--save-run`: `agreement: 5/5 scored items`, with 15
   CLI errors. The harness reported `partial run: NOT written to` the target file.
   This was an incomplete run, not a full-run passing score; Claude usage was exhausted.
6. Submission-preparation attempt in the Codex sandbox: `agreement: 0/0 scored items`,
   with all 20 CLI calls failing. The harness again refused to save a partial run.

**Pending before submission:** `eval-run.txt` currently records "Usage Limit Reached"
and a note to update it when credits reset.
A complete run using `--save-run` is required when Claude usage is available again;
append that run's agreement here so the final listed score matches the generated file.
The earlier 20/20 result is not a substitute for that harness-generated artifact.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174): the first full run rejected it, while gold said
`accept`; the revised rubric accepted it in the targeted and confirming full runs.
The package's diagnosis says: "The cache control fits: with an empty cache the color
data is refetched along the fresh-attach path once."
The first grader called this "an unevidenced assumption, not something the repro evidence states".
That applied the original check too literally: a diagnosis can infer a plausible
mechanism without the reproduction having already proved every internal step.
The controls show fresh attach is clean, repeated reattach leaks, 0.44.1 is clean,
and clearing the cache gives one clean attach. None contradicts the proposed mechanism.
The revised check allows that inference while still rejecting a cause that an actual
control result rules out. This explains the change to `accept`, matching gold.

**Check rationale**

Exact current `Diagnosis grounded` row from `tools/plan-check/rubric.md`:

```text
| Diagnosis grounded | The candidate plan's stated cause (however it is labelled — "Diagnosis," "Cause," or an unlabelled opening paragraph), read against the repro evidence block's own steps, control runs, and artifacts — never against what the thread merely guesses or assumes the cause to be. | The stated cause is consistent with every control run and artifact the repro evidence shows, and no control's actual result rules it out. The cause is allowed to explain a supporting control with a plausible mechanism the repro evidence does not spell out in those words (a diagnosis is an inference, not a transcript) — that kind of plausible reading of a control that fits the stated cause is a pass, not a gap. Fails only when a control's actual result actively points to a different mechanism, isolates a variable the stated cause does not touch, or shows the claimed defect already absent before the claimed cause could act — even when the diagnosis matches a cause the thread already floated, since a thread's own guess can be wrong and adopting it does not ground a diagnosis the repro evidence itself contradicts. | required |
```

The original pass condition began: "The stated cause explains every control run and
artifact the repro evidence shows". The revision changes that burden to consistency
with the controls, explicitly allowing plausible inference. The evidence guide was
revised with the same distinction. The `pkg-14` failure exposed why this mattered:
requiring every causal inference to already appear in a repro report mistakes a
hypothesis supported by observations for an unsupported contradiction.

**Trade-offs**

Allowing a "plausible mechanism the repro evidence does not spell out" can accept
an explanation that fits the observations but later turns out to be wrong. I accept
that limitation for a plan that will still be tested during implementation; consistency
with controls is evidence of readiness to investigate/build, not proof of causality.

The targeted rerun kept the four wrong-cause canaries `pkg-01`, `pkg-07`, `pkg-11`,
and `pkg-16` rejected, kept `pkg-02` accepted, and kept the unscored trap `calib-03`
rejected. It reported `agreement: 6/6 scored items`. That targeted run did not cover
the small thread-convention category, so it alone could not establish the category
floor. The subsequent full run supplied that check: `thread-convention 2/2` and
`wrong-cause 4/4`, with `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; the installed skill's
files in `tools/plan-check/`.
