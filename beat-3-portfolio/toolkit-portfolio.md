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

**issue-scout**

**What it is, and the judgment hole I wrote:** The issue-scout skill searches through GitHub with my queries. It ranks results with filters that I've defined for free. It then grades the top five choices against my rubric. I'm specifically using 15 queries as my search strategy, made up of 5 families of organizations, such as evals and observability, agent LLM apps, wider machine learning, data Python infrastructure, and midsize LLM tooling. I'm also running these queries against labels such as "good first issue", "help wanted", or "bug". My rubric consists of 13 checks, 10 of which are required and 3 preferred.

- `not-claimed` (rubric): fails on "a commit referencing the issue pushed within the last 30 days by someone other than a maintainer, including on a fork, which shows work in progress even when nothing was posted on the thread"
- `scope-feasible` (rubric): "It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks."
- Ranking filter 5 (search strategy): "first issues opened 30 to 365 days before the scan date, then issues under 30 days old, then issues over a year old."
- Search strategy: "`-- -linked:pr` drops issues that already have a pull request linked in GitHub's Development sidebar"

Initially, the scout accepted an issue and I recommended claiming it (codex-lb#1467, on 2026-10-16). However, someone had already pushed a fork commit naming the issue previously (gcs8 pushed it on 2026-10-15, the day before). My rule initially was looking only at assignees, linked PRs, and claim comments, so it couldn't see when an individual was pushing work without mentioning anything on the thread (I added the fork-commit clause that same day; my full Unit 1 eval still scored 20/20, and on the site screen the new rule caught 13 issues).

**How it evolved, dated:** What I realized is that most of my changes came from one day of real scans. It made me realize that sometimes you can uncover problems just from actually going through the process yourself instead of having to rely on evals. Evals can only help you so much, but actually doing the dirty work of running scans and going through the work yourself is what allows you to uncover problems that you would really face.

- **2026-09-23** (commit `a70493c`): "the single bounded-scope check carried too many independent failure modes ... which made it hard to revise one concern without risking a regression elsewhere (issue-01 regressed this way during Unit 1 iteration). Splits it into single-bounded-goal, no-unresolved-debate, not-core-internals, not-support-question, no-abandoned-history, and feature-request-confirmed. Re-confirmed 20/20 agreement"
- **2026-10-16** (scan-notes): "Switched the queries from keywords to org lists. A dry run of "llm" and "agent" with `good first issue` mostly returned tiny personal repos (one account had 24 of 200 results)"
- **2026-10-16** (scan-notes): "1 accept (docling-project/docling-pipelines#306) and 4 rejects, all on `not-claimed` ... Added `-- -linked:pr` to all six queries ... Tested before and after: it drops sktime#11495, openinference#3785 and rai-toolkit#108, and keeps docling#306."
- **2026-10-16** (scan-notes): "Added a `bug`-label query to every family. Scan 1 showed fresh newcomer-labelled issues get raced within hours, while confirmed bugs without that label draw fewer drive-by PRs ... Replaced "fewest comments first" with age bands from `createdAt`"
- **2026-10-23** (scan-notes): "Replaced `single-bounded-goal` with the staff `scope-feasible` row, and added a difficulty note ... an open design edge, an "etc.", or several files raise the difficulty note instead of rejecting. ... agreed 7/7 with gold."

**Evidence:**

- Before, `not-core-internals` in my Unit 1 rubric (`tools/issue-select/rubric.md`): "Fails if a maintainer states the fix touches core internals. Otherwise passes."
- After, 2026-10-16 (installed rubric): "Fails if a maintainer says, in any wording, that the change reaches into the project's core internals or into a security- or trust-critical subsystem (authentication, token or credential verification, permissions, cryptography)."
- The catch, scan-notes 2026-10-16: "Rejected codex-lb#1572 on the widened `not-core-internals` (the owner put the change in the crypto key-resolution layer); that's the patch working on a real issue."

**voice-guard**

**What it is, and the judgment hole I wrote:** Our voice-guard is a hook on Claude Code's Bash tool. Essentially, before any GitHub command like `gh issue comment`, `gh pr comment`, or `gh pr create` goes out, it reads the body file and checks it against my voice guide. If the body breaks a rule, the post is stopped, and the broken rule is quoted. Moreover, every check is logged in a log file (`guard-log.md`). My voice guide is essentially the guide that my three earlier skills also use. It covers who I am, along with some rules about how I communicate when it comes to development, as well as a section on the things I never tend to post. It also contains more information, such as how I handle claims, questions, and pings.

- Claims (voice guide): "A claim says when I'll be back in the thread with the result of my first step, either tied to finishing that step or on a short timescale I control and can keep. It never dates the fix or the PR"
- Wrong/Right pair (voice guide): "Wrong: "I'll take this and have a PR up by Friday."" / "Right: "I'd like to work on this. I'll reproduce it on current `main` first and post what I find here once I have the output.""
- Pings (voice guide): "I wait at least that long, and never less than a week. A ping adds something: the branch brought up to date, a new result, or a narrower question."
- Things I never post (voice guide): "Commands or output I didn't run myself, or any text I haven't read and checked line by line before posting, AI-assisted or not."

I've made this hook mine because, for example, I never date the fix or the PR in my communication with the maintainer. I essentially align with the plan that I'll be proposing, and I don't set deadlines so that it doesn't set any strong expectations for maintainers of specific deadlines.

**How it evolved, dated:** The guide's changes taught me that oftentimes you can get a decent hook or skill out of the gate, but what really makes it yours is aligning on the rules that define your own voice and the way you actually work. Otherwise, it can come off as very generic and robotic.

- **2026-10-23** (my Unit 6 Toolkit entry): "repro-check kept saying it couldn't find my voice guide, because Claude Code's Glob tool doesn't see the symlinks the install creates, so I replaced them with the install guide's one-line pointer files."
- **2026-10-23 to 10-24** (my Unit 6 Toolkit entry): "my new rule asking for a report-back time like "this week" clashed with my Unit 2 rubric, which failed that phrase as a deadline in one run and passed it in another, so I reworded the rule to allow a report-back point tied to finishing my first step." (rubric side: commit `790c9c3`, "Run 5 scores 20/20.")
- **2026-10-24 to 10-26** (guard-log): 8 stops quoting "A plan comment is the plan in short form, a few lines at most", from 2026-10-24 19:07 to 10-26 20:54; then "2026-10-26 21:12 [hook] pass | target: gh issue comment https://github.com/catalyst-cooperative/pudl/issues/5566 --body-file"

**Evidence:**

- Before, commit `a86f6ad` (2026-10-03), still in `tools/pr-precheck/voice-guide.md`: "A plan comment is the plan in short form, a few lines at most: the cause in one line, the change and where it goes, how I'll check it, and the one decision I'd like a maintainer's view on, if there is one."
- After, installed guide (2026-10-26): "A plan comment carries the plan in short form: the cause, the change and where it goes, how I'll check it, and the one decision I'd like a maintainer's view on, if there is one. If the repo asks for a plan in a particular shape (for example, steps with their outputs), I use that shape. Anything that serves none of these, such as my full reasoning or the evidence itself, belongs in the plan or the PR."
- Canary under the new rule, guard-log 2026-10-26 21:07: "stop: rule broken: "Anything that serves none of these, such as my full reasoning or the evidence itself, belongs in the plan or the PR.""

**pr-precheck + ship-gate**

**What it is, and the judgment hole I wrote:** The pr-precheck is a skill that grades a PR package (meaning my branch's diff, its title, and description) against my plan and the issue itself, and it gives an accept or reject status. Back in Unit 4, I wrote the SKILL.md file, a rubric of 8 required and 2 preferred checks, an evidence guide, a procedure, and a voice guide rule. The ship-gate is the pr-precheck skill's second half. It's a hook that runs on the `gh pr create` command. Before a PR can open, it runs every check in my manifest. It then runs the pr-precheck on the branch, and if anything fails, the PR doesn't open. Every run is also logged in a log Markdown file (`gate-log.md`).

- Manifest row (`ship-manifest.md`): "| Unit and integration tests | `. ~/pudl-dev/env.sh && pixi run pytest-unit && pixi run pytest-integration` | AGENTS.md "PR checklist"; CI runs both in .github/workflows/pytest.yml |"
- Excluded row, dated 2026-10-30 (`ship-manifest.md`): "It runs the fast ETL (~45-60 min per docs/dev/testing.rst), past the ship-gate's 25-minute limit. The PR template asks for it "when you think the PR is done", so I run it separately before a PR is marked ready"
- `fix-observed` (rubric): "Passes if every failure the reproduction recorded is shown re-run on the changed code, naming what was run ... and showing a specific result a reviewer can compare with the plan's expected-after"
- Gate log, 2026-10-27 21:03: "fix-observed [pass]: The repro trigger is re-run after the fix: the script shows 0 line-14 gaps and 8,483 line-6 rows, and dbt shows PASS=8, versus FAIL 1782 before."

I wrote the manifest's rows from PUDL's `AGENTS.md` PR checklist, and it lists excluded checks with a dated reason for each. My rubric ensures that a fix is actually observed.

**How it evolved, dated:** Running my own rubric on real PUDL PRs allowed me to see how my rubric would fare against real PUDL PRs and issues that I'd be working on within this repository. It also allowed me to gut-check my rubric for its correctness and efficacy.

- **2026-10-09** (my Unit 4 `pull-request.md`): "The first version required every item in the test plan to be shown re-run, which held pkg-05 over a new warning that an added test already pinned." (full run 1: 19/20; confirming run: 20/20)
- **2026-10-27 and 10-30** (my Unit 7 Toolkit entry): "On both of my PRs the first run stopped because my local `main` was behind upstream, so pr-precheck saw upstream's changes mixed into mine; fast-forwarding `main` fixed that"
- **2026-10-30** (commit `01fa5ef`): "The check required naming an issue "whatever the repo states", a Path Review house rule. PUDL asks for the closing issue only "if applicable", and its maintainer said my RUS 12 follow-up needed no issue. ... Full run 4 scores 20/20."

**Evidence:**

- Gate log, 2026-10-30 14:41, BLOCK: "repo-asks-met [fail]: template wants 'Closes #XXXX'; description names no issue it resolves ('No issue', cites #5729)"
- Before (`tools/pr-precheck/rubric.md`, before commit `01fa5ef`): "Passes if each ask is met with real content or marked not applicable with a reason, and, whatever the repo states, the description names the issue it resolves."
- After (commit `01fa5ef`, 2026-10-30): "Passes if each ask is met with real content or marked not applicable with a reason, and the description names the issue it resolves or, when there is no issue, says so and points to where a maintainer asked for or agreed to the change."
- Gate log, 2026-10-30 15:04: "verdict: PASS (honored the manual pass at 2026-10-30 15:03)"; PR #5737 opened that minute.

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

- My repro-check skill grades a claim comment plus a repro report against the issue, and it says whether they're ready to post (last changed 2026-10-24, commit `790c9c3`, eval 20/20; it accepted my #5566 repro report).
- My plan-check skill grades a plan plus a plan comment against the issue and repro evidence, and it says whether they're ready to post and build from (no rubric change since 2026-10-04, commit `e7557b3`; it accepted my #5566 plan with 9/9 required and 2/2 preferred checks).
- My pr-monitor skill is a headless runner that reads a PR's live state with read-only GitHub calls and drafts my next move from my playbook, and it posts nothing (one run on #5737 on 2026-11-02 drafted a wait note; #5737 merged on 2026-11-09, before my 14-day threshold, so the nudge path never ran live).

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
Date: 2026-10-16
Friction: "A dry run of "llm" and "agent" with `good first issue` mostly returned tiny personal repos (one account had 24 of 200 results), and search results carry no star count that ranking could use to screen them out." (scan-notes)
Change: "Switched the queries from keywords to org lists." ... "Split the queries into project families (eval/observability, agent/LLM-app, wider ML) and ran each once per label." (scan-notes)
Result: "the 6 queries returned 434 unique issues. 313 had no assignee, 229 of those were updated in the last 60 days, 224 passed the title filter and 138 had 5 comments or fewer." (scan 1, scan-output.md)

**Beat 2**
Date: 2026-10-16
Friction: "1 accept (docling-project/docling-pipelines#306) and 4 rejects, all on `not-claimed` ... Ranking can't see PRs, and "fewest comments first" favours the freshest issues, which are exactly the ones drive-by PRs reach within hours." (scan-notes)
Change: "Added `-- -linked:pr` to all six queries" ... "Added a `bug`-label query to every family" ... "Replaced "fewest comments first" with age bands from `createdAt`" ... "`not-claimed` now also fails on a commit referencing the issue that someone other than a maintainer pushed in the last 30 days, including on a fork" (scan-notes)
Result: "Regression: full Unit 1 eval 20/20, every category matched." ... "Scan 1 accepted 1 of 5 and lost 4 to open PRs; scan 2 accepted 5 of 5, every one a `bug` in the 30–365-day band with 0–1 comments, so the PR races are gone." (scan-notes)

**Beat 3**
Date: 2026-10-16
Friction: "But each accept had a blocker the rubric can't see: inspect_evals#2370 and #2388 sit behind a contributor gate ... vllm#48689 needs 8 Blackwell GPUs ... phoenix#13002 is stuck waiting on its reporter." (scan-notes)
Change: "Candidate fixes, not yet made: drop `stale`/`needs information`/`needs repro` labels in ranking ...; add NVFP4/FP8/Blackwell to the title rule; consider rubric checks for contributor gates, open prerequisites ..., and fixes that live outside the repo." (scan-notes)
Result: none

**Beat 4**
Date: 2026-10-23
Friction: "repro-check kept saying it couldn't find my voice guide, because Claude Code's Glob tool doesn't see the symlinks the install creates" (Unit 6 Toolkit entry)
Change: "so I replaced them with the install guide's one-line pointer files" ... "moved the guide to one home that the voice guard and my three earlier skills all read" (Unit 6 Toolkit entry)
Result: "for the report, a reject on the first draft and an accept on the final one, with all 9 required checks and both preferred checks passing" (Reproduction Process Toolkit block)

**Beat 5**
Date: 2026-10-23 to 10-24
Friction: "my new rule asking for a report-back time like "this week" clashed with my Unit 2 rubric, which failed that phrase as a deadline in one run and passed it in another" (Unit 6 Toolkit entry)
Change: "claim-promises-honestly no longer counts a rough report-back time for my own next step as a deadline; only a fix or PR date is." (commit `790c9c3`) ... "and reworded my voice rule to match" (Reproduction Process Toolkit block)
Result: "2026-10-23 23:27 [hook] pass | target: gh issue comment https://github.com/catalyst-cooperative/pudl/issues/5566 --body-file" (guard-log) ... "Run 5 scores 20/20." (commit `790c9c3`)

**Beat 6**
Date: 2026-10-24 to 10-27
Friction: 8 stops quoting "A plan comment is the plan in short form, a few lines at most", from 2026-10-24 19:07 to 10-26 20:54 (guard-log)
Change: "A plan comment carries the plan in short form: the cause, the change and where it goes, how I'll check it, and the one decision I'd like a maintainer's view on, if there is one." (voice guide, 2026-10-26)
Result: "Posted my plan on the issue through the voice guard" (Implementation Notes, 2026-10-26) ... "Good catch - let's consider that fix to be in scope of this PR. That plan sounds reasonable to me." (e-belfer, pudl#5566, 2026-10-27)

**Beat 7**
Date: 2026-10-27
Friction: "every row needed my `env.sh` loaded, because the gate runs each row in a fresh shell." (Testing Strategy Toolkit block)
Change: "My manifest runs the four checks in PUDL's `AGENTS.md` PR checklist, each loading my `env.sh`" (Unit 7 Toolkit entry)
Result: "Pre-commit hooks on all files: `. ~/pudl-dev/env.sh && pixi run prek-run` -> ok (9s)" ... "Unit and integration tests: `. ~/pudl-dev/env.sh && pixi run pytest-unit && pixi run pytest-integration` -> ok (116s)" (gate-log, 2026-10-27 20:49)

**Beat 8**
Date: 2026-10-27
Friction: "diff-clean [unclear]: git diff main...HEAD ran against a stale main (85 files, 6 upstream commits)" (gate-log, 2026-10-27 20:49)
Change: "The first gate run rejected because my local `main` was six commits behind upstream; after I fast-forwarded it, the gate passed." (Implementation Notes, 2026-10-27)
Result: "verdict: PASS (honored the manual pass at 2026-10-27 21:03)" (gate-log, 2026-10-27 21:11) ... "opened draft PR #5729" (Implementation Notes, 2026-10-27)

**Beat 9**
Date: 2026-10-30
Friction: "repo-asks-met [fail]: template wants 'Closes #XXXX'; description names no issue it resolves ('No issue', cites #5729)" (gate-log, 2026-10-30 14:41)
Change: "On #5737 the gate also failed `repo-asks-met` for naming no issue, which PUDL doesn't require, so I changed that check and reran my Unit 4 eval (20/20)." (Pull Request Toolkit block)
Result: "verdict: PASS (honored the manual pass at 2026-10-30 15:03)" (gate-log, 2026-10-30 15:04) ... "2026-11-09: Austen approved #5737 ("This looks great, thanks @Akhan521"), merged it through the merge queue" (Pull Request section)

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
