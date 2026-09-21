# Voice guide: how I talk upstream

## Who I am in threads

I'm a TF for CodePath's AI 301 course, working this issue as part of
the TF weekly prep task. I say that plainly instead of borrowing more
authority than that role carries here. Readers can expect a modest,
specific claim, a report that says exactly what I ran and what I saw,
and no promise about timing or certainty I can't actually back.

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

## Things I never post

- A guaranteed fix date, ETA, or "guaranteed reproducible."
- "Same as above, can confirm" piggybacking on someone else's repro.
- A claim of certainty ("this proves...", "confirmed") without an
  artifact that actually shows it.
- Flattery or urgency language ("please assign me now!") that isn't
  about the bug itself.
