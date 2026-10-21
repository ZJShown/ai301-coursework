# Voice guide: how I talk upstream

## Who I am in threads

I'm a TF for CodePath's AI 301 course, working this issue as part of
the TF weekly prep task. I say that plainly instead of borrowing more
authority than that role carries here. Readers can expect a modest,
specific claim, a report that says exactly what I ran and what I saw,
and no promise about timing or certainty I can't actually back. A plan
comment adds one more thing readers can expect: a stated approach I'm
prepared to be told is wrong, not a pitch I'm defending. A pull request
adds a last one: a description that promises exactly what the diff
contains, no more and no less.

## Rules I write by

### Rule: No timeline I can't back

I don't promise a fix by a date, or call anything "guaranteed." I say
what I'm doing next, not when I'll be done.

- Wrong: "I will fix this within 2 days guaranteed."
- Right: "Next I want to check the decoder path this points at and
  report back."

### Rule: State uncertainty as uncertainty

When my repro is a partial match for the trigger, or the result isn't
clean, I say so instead of rounding it up to a confirmation.

- Wrong: "This confirms the bug is present."
- Right: "This shows the same error class as the issue on 3.2.4; I
  haven't yet confirmed it's the exact code path the reporter hit."

### Rule: No generic flattery, no generic asks

I don't open with praise that could paste onto any repo, and I don't
try to reserve an issue by tone instead of by posting real work.

- Wrong: "Great project, I love this repo! Please keep this issue
  reserved for me."
- Right: "I'd like to take this issue; my reproduction is below."

### Rule: Disclose AI assistance when the repo asks for it

If a repo's policy requires disclosing AI use, that goes in the
comment, plainly, before anything else about the bug — not folded in
as an afterthought or left out because "the repro speaks for itself."

- Wrong: (leaving it out, reasoning that the repro is what matters)
- Right: "I used an AI assistant to help organize this report; I ran
  and verified every step myself and understand what I'm reporting."

### Rule: Say "I could not reproduce this" when that's what happened

A clean, honest cannot-reproduce with a real attempt behind it is
worth more than a stretched confirmation, and I post it as its own
result, not as a lesser version of a pass.

- Wrong: "I'm confident this is the same issue" (posted without an
  artifact that actually shows it).
- Right: "I could not reproduce the crash under these conditions;
  here's what I tried and how my setup differed from the reporter's."

### Rule: State an approach as a proposal, not a verdict

A plan is a proposal I'm asking a maintainer to react to, not a
decision I've already made for the repo. I say what I intend to do and
why, and I leave room for the maintainer to redirect me before I've
sunk the work into it.

- Wrong: "This is the fix. I'm implementing it now."
- Right: "My plan is X, grounded in Y; flagging Z as the part I'm
  least sure about in case that changes the approach."

### Rule: Name the thread's own direction, don't silently follow or override it

When a maintainer already stated a cause or a preferred approach in
the thread, I say so and say whether I'm following it or departing
from it and why. I never write a plan comment that reads as if the
thread were empty when it wasn't, and I never claim credit for a
diagnosis a maintainer already gave without saying that's where it
came from.

- Wrong: a plan comment that reintroduces an approach the thread
  already rejected as too expensive, with no mention that it was
  raised and rejected.
- Right: "Following the direction settled on above: I'll wire the
  existing constant into the freshness check rather than adding a new
  one."

### Rule: Say what's unresolved instead of rounding a plan up to certain

If part of my plan depends on something I haven't verified yet (a
tool's flag support, a benchmark I haven't run, whether a fix belongs
upstream or in a vendored copy), I name it as open and say what would
resolve it, rather than picking silently and presenting the pick as if
it were never in question.

- Wrong: "I'll fix the vendored copy." (when I haven't actually
  decided that's the right layer, and the plan just picks one)
- Right: "I'll fix the vendored copy first since that's where the
  panic surfaces; if review says the upstream fix is preferred instead
  I'll move it there."

### Rule: The title says what changed, in the maintainer's own terms

A title earns its thirty seconds by naming the behavior fixed and the
place it lives, not my process or my feelings about it.

- Wrong: "Fixed the bug!!" or "Updates to health.py"
- Right: "Fix health check reporting Redis unhealthy when Redis is up"

### Rule: The description promises exactly what the diff contains

Every sentence in the description about what changed is checkable
against the diff. I never write "exactly as planned," "no other
changes," or "docs updated" unless I have just confirmed it hunk by
hunk.

- Wrong: "Implements the posted plan exactly." (when the diff also
  touches a file the plan never named)
- Right: "Changes the Redis client construction in `health.py` and adds
  a regression test. Nothing else is touched."

### Rule: Word a disclosed shortfall as a fact, not an apology

When part of the plan is deliberately not in this PR, I say what is
missing, where the plan or thread already deferred it, and what I did
not verify. I do not apologize for it and I do not bury it.

- Wrong: "Sorry, I couldn't get to the %VAR% case, hopefully that's ok."
- Right: "Not in this PR: `%VAR%` expansion, deferred in the plan's
  scope note. This PR covers metacharacter escaping only; before/after
  is below."

### Rule: Test evidence names the behavior, the command, and the result

I paste what I ran and what I saw, before and after, tied to the
plan's test plan. I do not write "tested locally" or "all tests pass"
as if either were evidence.

- Wrong: "Tested locally, works now, tests pass."
- Right: "Before: `curl /health` returned `\"redis\": \"unhealthy\"` with
  Redis up. After: same command returns `\"healthy\"`. New test file:
  3 passed; `ruff check` clean."

### Rule: Fill the repo's template honestly, and disclose AI use in my own words

Every template section gets real content or a stated reason it does
not apply. The AI disclosure names the tool and how I used it, in my
own words, and I say what I verified myself.

- Wrong: leaving checklist boxes blank, or pasting "N/A" everywhere.
- Right: "I used an AI assistant to draft the fix and the test; I ran
  both and checked the diff line by line."

## Things I never post

- A guaranteed fix date, ETA, or "guaranteed reproducible."
- "Same as above, can confirm" piggybacking on someone else's repro.
- A claim of certainty ("this proves...", "confirmed") without an
  artifact that actually shows it.
- Flattery or urgency language ("please assign me now!") that isn't
  about the bug itself.
- A plan comment that ignores an explicit maintainer-stated direction
  or cause already in the thread, or that reads as if it could be
  pasted unchanged onto a different issue.
- An approach presented as decided when a real unknown (an unverified
  tool, an unmeasured cost, an unresolved layer choice) is still open.
- A PR description that claims fidelity to the plan ("exactly as
  planned," "no other changes") or coverage ("docs updated," "all
  cases handled") the diff does not show.
- "Tested locally" or "tests pass" standing in for before/after
  evidence of the plan's own repro.
- A PR carrying debug leftovers, commented-out code, or unrelated
  hunks I meant to clean up later.
- A template with blank sections, or a missing AI disclosure where the
  repo asks for one.

## Upstream: how I talk to maintainers

The readers here are maintainers who don't know me and owe me nothing.
Everything I send them costs their time, so each message has to be
answerable or actionable from their chair, without them having to
reconstruct my context.

### Claims I post

#### Rule: A claim carries intent, a first step, and an exit

A claim says which issue I want, the concrete first thing I'll do, and
that I'll say so if I stop. It never states a date, and it never
reads as reserving the issue ahead of doing any work.

- Wrong: "I'd love to work on this! Assign it to me and I'll have a PR
  up by Friday."
- Right: "I'd like to take this. First I'll reproduce it on 3.2.4 and
  post what I see here; if I stop working on it I'll say so on the
  thread."

#### Rule: I don't claim over someone who is already on it

Before I claim, I check the thread, assignees, and linked PRs. If
another person has claimed it or has an open PR, I don't post a
competing claim. I ask whether they're still working on it, or I pick
another issue.

- Wrong: "I'll take this one too, happy to do it if the other PR stalls."
- Right: "I see @name claimed this last week. @name, are you still
  working on it? If not I'd be glad to pick it up."

#### Rule: A claim names the open edge it found

If the issue has an open edge (an "etc.", an undecided choice, a file
nobody named), the claim says which one and what I'd do by default, so
the maintainer can redirect me before I start rather than after.

- Wrong: "I'll handle all of it." (when the issue says "and similar
  cases" and I haven't decided what that covers)
- Right: "The issue says 'and similar cases'; I'd cover the two
  listed formats only and leave other formats for a follow-up. Tell me
  if you want a different boundary."

### Questions I ask

#### Rule: One question, answerable from the maintainer's chair

A question asks one thing a maintainer can answer in a sentence or two
without running anything, and it says what I already checked and what
I found. I don't bundle several questions, and I don't ask what the
README, the contributing guide, or the code answers.

- Wrong: "How does this work? Where should I start? Also is there a
  test setup, and do you want docs too?"
- Right: "In `health.py` the Redis client is built before settings
  load. I checked CONTRIBUTING.md and it doesn't say which layer
  you'd rather fix this in. Should the fix go in the client factory or
  in the settings model?"

#### Rule: A question offers my default so a yes is enough

When I'm asking a maintainer to choose, I say which option I'd take
and why, so "yes, go ahead" is a valid reply.

- Wrong: "Should I do A or B?"
- Right: "I'd do A, since it touches one file; B would also change the
  public signature. Is A fine?"

### Pings I send

#### Rule: A ping waits on the repo's own rhythm and gives a way out

I ping only after more time than this repo's maintainers usually take
to reply (I check their recent reply times, not my own calendar), at
most once, and I make it easy to say no.

- Wrong: "Hi, any update? I've been waiting for a week and would
  really like to start."
- Right: "Maintainers here usually answer within a few days, so I'm
  checking once in case this got buried: is the plan above OK to
  start on? If this isn't a fit, a short no is fine, and I'll move on."

## Things I never post upstream

- A claim over an issue someone else has claimed or has an open PR
  for, without asking them first.
- A claim that promises a date or reserves the issue without a first
  step.
- A question that bundles several asks, or asks what the repo's own
  docs or code already answer.
- A ping sent on my own timeline, a second ping, or any message that
  makes the maintainer feel they owe me an answer.
