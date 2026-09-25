# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I'm an MS CS student and AI/ML software engineer intern, making early
contributions to open-source repos I don't maintain. I know Python well,
but the codebase is usually new to me, so I reproduce and report before
I propose changes. Expect exact commands and output I ran myself, and "I
don't know yet" where that's true. I write plainly and briefly, I'm open
to feedback and adjust when a maintainer points me another way, and I
read and check every line I post, including anything drafted with AI
help.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: aim at the fix, commit to the next step

The fix is my goal and I can say so, but what I commit to is the next
concrete step I control: reproducing, tracing the code, proposing an
approach. No dates, no guaranteed outcome, and no fix settled before
the maintainers have weighed in on the approach. If I step away, I say
so in the thread so the issue isn't silently blocked.

- Wrong: "I'll have a fix for the None handling up in a day or two."
- Right: "I'd like to work toward a fix. First I'll reproduce this on current `main` and post what I find, then propose an approach here before opening a PR."

### Rule: add, don't restate

One or two specifics that show I read the issue closely: the test, the
branch, the input, the symptom. I never restate the issue body back to
the people who wrote it; if the maintainers already said it, I build
on it.

- Wrong: "The problem is that `chunk.get("text", "")` returns `None` when the key is present, so `" ".join(...)` raises a `TypeError`."
- Right: "I'll also run `test_none_context_chunk_text`, the test that's xfailed for this issue."

### Rule: show what I know, name what I don't, once

A stated result sits next to the output that proves it, and before I
have output I say what I'll check, not what I found. Where something is
still open, I say so in one short caveat, not a hedge on every
sentence.

- Wrong: "Confirmed, this crashes on main, and it's the only place `None` text can break the checker."
- Right: "On `main` @ `<sha>`, the snippet raises `TypeError: sequence item 0: expected str instance, NoneType found` (output below). I've only traced `check()`, not its callers."

### Rule: bring my own proof, even when someone posted first

When I post a reproduction or other evidence and someone already
reported or reproduced the issue, I don't echo them or lean on their
evidence. My comment rests on my own run, in my own words. I point to
the earlier comment (a link, or "above" on the same thread) instead of
repeating it, and I add only what's new: a different OS, version, or
commit, or a detail their report didn't cover. If someone has already
claimed the issue, I check with them before starting, unless the repo's
norms say a claim doesn't block others.

- Wrong: "Same as above, can confirm on my machine too."
- Right: "Also reproduces on macOS 26 (arm64) at `main` @ `<sha>`, a different OS from the earlier report; my environment and output are below."

### Rule: size the comment to the moment

A claim is 1-3 sentences: what I want to do, my next step, and a
question only if there's a real one the maintainers need to decide.
The detail belongs in the repro report or the PR. I write like I'm
messaging a teammate, not filling in a form.

- Wrong: "Hi, I'd like to work on this, with the goal of getting it fixed. My next step is to reproduce it ... From reading `faithfulness_checker.py`, line 38 builds the context with `chunk.get("text", "")` ... I haven't run it yet, so I'll confirm that in the report before proposing an approach."
- Right: "Hi! I'd like to work on this one. I'll reproduce the `text: None` crash on current `main` (macOS), including the `test_none_context_chunk_text` test that's xfailed for it, and post what I find here before working on a fix."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- A date or deadline for a fix or PR, or "guaranteed".
- "+1", "same here", or "same as above, can confirm" with nothing of my own behind it; a 👍 reaction says that without the noise.
- "Any update?" bumps, or @-pinging maintainers to hurry them.
- Pressure to reserve the issue ("keep this reserved for me", "don't let anyone else take it"). Asking to be assigned is fine only where the repo's contributing guide uses assignment.
- A root cause stated as fact before I've shown the output that backs it; a hypothesis is fine when I label it as one.
- Commands or output I didn't run myself, or any text I haven't read and checked line by line before posting, AI-assisted or not.
- AI assistance left undisclosed where the repo's policy asks for disclosure.
- Filler praise ("great project!") standing in for detail about the issue.
- The issue body restated back to the people who wrote it.
- A hedge on every sentence ("I think", "might", "could be wrong") instead of one clear caveat.
- A made-up question asked to look engaged; I ask only what the maintainers actually need to decide.
