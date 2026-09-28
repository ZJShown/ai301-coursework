# Plan: health check reports Redis unhealthy even when Redis is up

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62

## Diagnosis

As reported in my Unit 2 reproduction comment
(https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5768152699):
`health_check()` in `api/routes/health.py` builds its Redis client from
`settings.redis_host` and `settings.redis_port`, but `Settings` in
`core/config.py` only defines `redis_url`. Accessing the missing
attribute raises `AttributeError` while constructing the client, before
any connection is attempted; the route's broad `except Exception`
swallows it and reports `"redis": "unhealthy"` unconditionally. My
repro confirmed Redis itself was reachable (`redis-cli ping` → `PONG`)
at the same time `GET /health` reported it unhealthy, and confirmed the
exact attribute error directly (`python -c "from core.config import
settings; settings.redis_host"` → `AttributeError`).

Grounding beyond my own repro: every other Redis client in this
codebase (`agent/memory/session_store.py`, `safety/rate_limiter.py`,
`safety/monitoring.py`) is constructed elsewhere and injected in, and
`Settings` has never had `redis_host`/`redis_port` fields — only
`redis_url`. The health check is the only place in the codebase that
tries to build a client from host/port fields that don't exist, which
is consistent with this being a one-off mistake in this route rather
than a config field that used to exist and was removed.

## Prior art

PR #76 (`Tiyatrotist`, open) already proposes closing this issue by
building the client from `settings.redis_url` via
`redis.Redis.from_url(...)`, with a regression test. I checked it
before writing this plan. I'm not racing it or piggybacking on it: per
this course's Path Review house rules, my plan, branch, and PR are my
own work regardless of a parallel PR on the same issue. I independently
reached the same `redis_url`-based fix from reading the codebase's own
pattern (see Diagnosis above), so I'll say so plainly in my plan
comment rather than presenting it as if I hadn't seen #76.

## Scope

In scope: the Redis branch of `health_check()` in
`api/routes/health.py` — building the client from `settings.redis_url`
instead of the nonexistent `redis_host`/`redis_port` fields.

Not in scope:
- The Postgres branch's raw-string `db.execute("SELECT 1")` call (a
  separate, already-known issue — issue #61 — unrelated to this one).
- Adding `redis_host`/`redis_port` fields to `Settings`. Nothing else
  in the codebase uses that shape; every other Redis client here is
  built from a URL or injected pre-built, so matching that pattern is
  the smaller, more consistent change.
- Making the Redis ping call non-blocking / async, or closing the
  connection after use. The current code already makes a blocking
  sync call inside an `async def` route and never closes the client;
  both predate this bug and are out of scope for this fix.

## Files

- `api/routes/health.py` — the Redis check block (currently lines
  41-58).
- `tests/unit/test_health.py` (new) — no test file for this route
  exists yet.

## Approach

1. In the Redis check block, replace `redis.Redis(host=settings.redis_host,
   port=settings.redis_port, db=0, decode_responses=True)` with
   `redis.Redis.from_url(settings.redis_url, decode_responses=True)`.
2. Leave the rest of the block (the `try`/`except`, the `r.ping()`
   call, the status strings) unchanged — the fix is the client
   construction, not the check's logic.
3. Add `tests/unit/test_health.py` with a case that patches
   `redis.Redis.from_url` and asserts it is called with
   `settings.redis_url`, and a case that the dependency reports
   `"healthy"` when `ping()` succeeds — mirroring what PR #76's
   description says its own regression test checks, written
   independently against my own repro rather than copied from it.

## Test plan

Re-run my Unit 2 repro steps against the built change:

- Before (from my repro comment): `docker compose up -d`, app running,
  `redis-cli ping` → `PONG`, then `curl http://localhost:8000/health` →
  503, body shows `"redis": "unhealthy"`, server log shows
  `redis_health_check_failed error="'Settings' object has no attribute
  'redis_host'"`.
- After (expected): same environment, same `curl` command → the
  response's `"redis"` field reads `"healthy"`, no `redis_host`
  `AttributeError` in the server log. The already-separate
  `"postgres": "unhealthy"` entry (issue #61) is expected to remain
  unchanged and is not evidence for or against this fix.
- Also run the new `tests/unit/test_health.py` case and confirm it
  passes.

## Test evidence (captured against the built change)

Backing services up (`docker compose up -d`; `db` and `redis`
containers reported `healthy`), server run on a scratch port
(`8010`, so as not to disturb the standing dev server on `8000`) so
before and after could be captured back to back on the same running
Postgres/Redis instances.

**Before** (`api/routes/health.py` temporarily reverted to the
pre-fix state via `git stash`, to get a fresh real-path capture
rather than relying only on my Unit 2 comment's evidence):

```
$ docker exec pathreview-ai301-fa26-s1-redis-1 redis-cli ping
PONG
$ curl -s -w "\nHTTP_STATUS:%{http_code}\n" http://localhost:8010/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"}, ...}}
HTTP_STATUS:503
```
Server log: `redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"`.

**After** (fix restored, same services, same port):

```
$ docker exec pathreview-ai301-fa26-s1-redis-1 redis-cli ping
PONG
$ curl -s -w "\nHTTP_STATUS:%{http_code}\n" http://localhost:8010/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"healthy","vector_db":"healthy"}, ...}}
HTTP_STATUS:503
```
Server log: `redis_health_check_passed` (debug), no `redis_host`
`AttributeError` anywhere. `"postgres": "unhealthy"` is unchanged in
both runs, exactly as expected — issue #61, not this fix.

Also ran `tests/unit/test_health.py`: 3 passed. `ruff check` clean on
both changed files.

## Risks and unknowns

- ~~I have not yet confirmed whether `redis.Redis.from_url` accepts
  `decode_responses` as a keyword~~ — resolved: confirmed both by the
  passing unit test and by the after-fix server log showing
  `redis_health_check_passed` with no error, on the installed `redis`
  package version.
- The pre-existing blocking sync call and unclosed client (noted under
  Not in scope) are real but separate issues; I'm flagging them as
  known, not fixing them, and will mention them in the PR only as an
  aside in case a maintainer wants a follow-up issue filed.

## Deviations

Nothing changed; the plan held. The only thing the plan called
"unverified" (whether `redis.Redis.from_url` takes `decode_responses`
on the installed `redis` version) got confirmed during this same test
pass rather than needing a separate check before the PR — a resolved
unknown, not a deviation from the approach itself.
