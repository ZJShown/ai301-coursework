# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Each query names the fit-profile sentence it came from. -->

# Q1: "Search Python repositories first" + "I'd like more practice reading an unfamiliar service codebase": broad beginner-labelled Python issues
gh search issues --label "good first issue" --language python --state open --no-assignee --archived=false --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# Q2: "config handling, settings models... bug in a Python service's settings" + "bugs with clear, runnable repros": config bugs in Python
gh search issues "config" --label "bug" --language python --state open --no-assignee --archived=false --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# Q3: "health checks, request validation, error handling" + "backend services": health-check work that is open for contributors
gh search issues "health check" --label "help wanted" --language python --state open --no-assignee --archived=false --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

## Ranking filters

<!-- Applied in the order written, over the --json fields only. -->

1. Drop any result with a non-empty `assignees` list (belt and braces: the queries already pass --no-assignee).
2. Drop any result whose `updatedAt` is more than 60 days before today.
3. Drop any result whose `labels` include "documentation", "docs", or "question" (from "I'd rather avoid ... issues that are really documentation-writing").
   Also drop any result whose `title` starts with "[auto]" or whose `labels` include one starting with "auto" (a machine-filed report is not a task I can pick up; added 2026-10-12 after untether #1049/#1050).
4. Order the survivors first by label: results whose `labels` include "good first issue" before those without (a maintainer's own newcomer signal; added 2026-10-12 after two scans where every top-5 result was a 0-comment tie). Within each group, order by `commentsCount` ascending (fewest comments first; fewer comments means less chance a stranger is already on it).
5. Tie-break: label "bug" present before absent (from "bugs with clear, runnable repros over feature requests"), then `updatedAt` newest first.
6. Keep only the best-ranked result per `repository`, so one repo cannot fill several of the graded slots (added 2026-10-12 after two untether duplicates took 2 of 5).

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
