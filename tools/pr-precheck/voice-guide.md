# Voice guide: how I talk upstream

## Who I am in threads

I'm an MS CS student and AI/ML software engineer intern, making early
contributions to open-source repos I don't maintain. I know Python well,
but the codebase is usually new to me, so I reproduce and report before
I propose changes. Expect exact commands and output I ran myself, and "I
don't know yet" where that's true. I write plainly and briefly, I'm open
to feedback and adjust when a maintainer points me another way, and I
read and check every line I post, including anything drafted with AI
help.

## Rules I write by

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

A plan comment is the plan in short form, a few lines at most: the
cause in one line, the change and where it goes, how I'll check it, and
the one decision I'd like a maintainer's view on, if there is one. The
full reasoning lives in the plan and the PR.

### Rule: propose the approach, don't decide it for them

A plan comment puts one concrete approach in front of the people who
maintain the code. I state it plainly, give the one-line reason I
picked it over the obvious alternative, and leave room for a maintainer
to redirect me before I build. If a maintainer already suggested a
direction, I follow it or say why I'm not. If the build later departs
from what I posted, I say so in the thread instead of letting the PR
quietly disagree with the plan.

- Wrong: "I'm going to rewrite the parsing to handle this properly; PR coming."
- Right: "Plan: default the missing value to an empty string at the one call site, rather than skipping the entry, so the score's inputs stay the same shape. If you'd rather reject bad input earlier, I can do that instead."

### Rule: a PR describes exactly the diff it ships

The title names the change in the repo's title convention, short
enough to read in a list of PRs. The description says what changed,
why, and how I checked it, then lets the evidence speak; every file,
test, or effect it mentions is in the diff. Anything I left out or
changed from the plan gets one plain sentence with the reason, stated
as a fact rather than an apology.

- Wrong: "Fix bug" / "Fixes the None crash and makes the checker robust to any bad chunk. Sorry, I didn't get to RelevanceScorer."
- Right: "fix(rag): treat None chunk text as empty in faithfulness checker" / "`check()` now treats a `None` chunk text as empty, so it scores instead of raising `TypeError`. `RelevanceScorer` has the same crash; it's outside this issue, so I'll raise it separately."

## Things I never post

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
- A rewrite or "while I'm in here" cleanup pitched as the fix for a bounded bug; extra work goes in its own issue.
- A plan comment that only points at someone else's plan ("same approach as above").
- A posted plan left standing after my build changed course; I add a follow-up comment that says what changed and why.
- A PR description that claims a check passed when I didn't run it, or coverage the tests don't show.
- "No other changes" or "exactly as planned" over a diff I haven't re-read hunk by hunk against the plan.
