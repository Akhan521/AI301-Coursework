# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.

**Student:** Aamir Khan

## Featured Tools

Three or four tools, presented in depth. Open with a few sentences
on why you chose these.

Full credit asks the featured set to show real change: **at least two** of your featured
entries should show a tool that visibly moved through use. An honest entry for a tool that
barely changed — saying so, dating what did happen, and giving the reason — earns the solid
middle tier and never fails you.

Each tool's honest limit is not written here. It goes in the tool-limits list at the
bottom of this page, one line per featured tool, so that the limits sit together and can be
read as a set.

issue-scout was one of the three tools that I've selected because it was essentially the best skill that I had that allowed me to find an issue worth working on that fit my profile. Not only that, but I was able to tweak it such that it could lean in on my interests as well as the type of work that I'm looking forward to doing in the future. I also had a lot of friction with the skill, as in it kept getting things wrong initially (scan 1 on 2026-10-16 accepted 1 of its top 5, and the other 4 already had open PRs; it also accepted codex-lb#1467 while someone was fixing it on a fork), and so I feel like it grew a lot out of all of the improvements and refinements I made (scan 2 accepted 5 of 5, with no PR conflicts). There were a lot of changes that I made to this issue-scout skill so that it could be more robust and useful in open-source development (it accepted pudl#5566, which became my merged PR #5729).

voice-guard was one of the three that I chose because, essentially, every post that I made on the PUDL repository went through it (the claim on #5566, my plan, my plan change, four replies on #5729, both PR openings, and my thank-you on #5737, from 2026-10-23 to 2026-11-09) to make sure that every piece of work that I contributed was in line with the way I would communicate naturally (on 2026-10-24 and 10-26 its plan-comment rule stopped my plan draft again and again until I rewrote the rule). It was also what enabled me to very conveniently and smoothly interact with maintainers (a day after my plan posted, the maintainer replied: "That plan sounds reasonable to me.").

The pr-precheck + ship-gate skill was one of the three that I chose because it essentially was what allowed me to make sure that, any time I was trying to make a PR, I was keeping my local main up to date with upstream (on 2026-10-27 and 10-30 its first run blocked because my local main was behind upstream, so upstream's changes showed up mixed into my diff). Initially, my rubric had a rule that was forcing me to name an issue in the PR description (even though sometimes maintainers, like in my example, did not want a specific issue for a follow-up; I changed my repo-asks-met check on 2026-10-30, commit 01fa5ef, and my Unit 4 eval still scored 20/20). It was also what allowed me to make sure that all of my PRs were in line with what maintainers were expecting on this repository (my manifest ran the checks in PUDL's AGENTS.md PR checklist before every PR). Both of my PRs that I made passed the gate, and eventually both were merged in (#5729 on 2026-10-30 and #5737 on 2026-11-09).

Copy the entry shape below once per featured tool and fill it there.

**[Tool name]**

**What it is, and the judgment hole I wrote:** [what the tool does,
and the part of it that is your judgment: the rubric, strategy,
guide, manifest, or playbook you wrote.]

**How it evolved, dated:** [the changes that made it yours, each
with its date and the friction that drove it; your weekly Toolkit
entries are the source.]

**Evidence:** [show, don't claim: an output excerpt, a before/after
pair, or a dated commit reference, inline here. Draw it from your real
term work where you can - a catch this tool actually made, a
before/after from your own repo or PR - which is graded separately from
whether an artifact is present at all. A short, telling excerpt beats a
pasted transcript.]

## Inventory

One line each for the rest of the toolkit: what it is and where it
stands (one bullet per tool; a compact sentence or two is fine).

- [Tool name: one line.]

## The Arc

The story of the toolkit in order: friction, change, result, dated.
Your weekly Toolkit entries are the source; read your own record and
tell it oldest first.

Write one block per beat, in the shape below, oldest first. Keep all four labels on every
beat, each on its own line. The `Result:` line is what the beat was for: it says what
happened *after* the change, which is not the same sentence as the change itself. If a beat
never reached a result, write `Result: none` — an honest beat that did not land still counts
fully as a beat.

Full credit asks for **at least two** beats whose `Result:` line names something that
actually happened afterwards. A beat about something that did not work counts fully as a
beat, and when the work happened never moves this score.

**Beat 1**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

**Beat 2**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

Copy the beat shape above for as many beats as your term had.

## Limitations and What's Next

What the toolkit as a whole still does not do, and what you would
build or change next. Write this section last; it will be better
after the rest.

[The honest limits, and what comes next, in your own sentences.]

**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.

issue-scout: [where this tool still stops, in plain words, or the word none]
[tool name]: [where this tool still stops, in plain words, or the word none]
