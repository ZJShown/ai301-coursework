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
