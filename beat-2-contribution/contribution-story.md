# Contribution: Validate `core_rus7__yearly_balance_sheet_assets` and corresponding output table

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Aamir Khan
**GitHub Username:** Akhan521
**Issue:** https://github.com/catalyst-cooperative/pudl/issues/5566
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my issue-select rubric into issue-scout, wrote its search strategy, and patched it after two real scans**

issue-scout is a Claude Code skill that searches GitHub with queries I wrote, ranks the results with my own filters, and grades the top five against my Unit 1 rubric; I aimed its queries at the orgs I care about (LLM evals, agents, ML tools, data tools) because keyword searches like "llm" mostly found tiny personal projects. My first scan only accepted 1 of its top 5 issues, because the other 4 already had someone's pull request open, and my rubric also missed an issue that someone was already fixing in their own fork. I fixed this by filtering out issues with linked pull requests, searching for labeled bugs and preferring issues that are a few months old, and updating two rubric checks, and my Unit 1 eval still scored 20/20 afterward. My second scan accepted all 5 with no pull request conflicts, but each one still had a problem my rubric can't see yet, like needing a GPU or only taking pull requests from approved contributors.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

None of my Unit 5 picks survived this week. My scout accepted my lead, opensearch-benchmark#714, again after this week's scope patch, with an "ambitious" difficulty note, but reading the code showed it wasn't the rename the thread suggested: the transform runners still call Elasticsearch's API and the workload sends an Elasticsearch-format request, so the fix was a port across two repos. My two backups, galaxy#16079 and mahout#1507, both got pull requests this week, and my regrade rejected both on `not-claimed`. A fresh GitHub scan accepted 4 of 5 issues, but each had a blocker my rubric can't see, so I screened the Issue Selection site itself with my rubric's rules: every AI/ML and Python issue on it (2,517), dropping anything assigned, with an open or merged PR, or with any claim comment, and then had the scout grade the best two dozen. It accepted catalyst-cooperative/pudl#5566 with a "moderate" difficulty note, passing every required check and all three preferred ones. PUDL is Catalyst Cooperative's open pipeline for US energy data, built on Dagster and dbt, which is close to the production data platform work I do at Tatari. The issue adds dbt checks that each total on a rural electric co-op's balance sheet (USDA RUS Form 7) equals the sum of its line items, which is data-quality testing on a real pipeline and the same skill as the evals work I want to get better at. I also picked it because it's the most likely to land: all four sibling issues under the same umbrella issue (#5154) were finished by outside contributors and merged within 11 to 20 days, and the work left is about three test blocks, since the schema already checks two totals and the test macro already supports subtraction. I want to learn how a real data team designs its validation tests and how to work through a maintainer's review.

My scout held up on the calls that mattered: it rejected galaxy and mahout the day their pull requests appeared, caught an open PR on zulip#33413 that my own screening script missed, and noticed that Phoenix's "good student issue" label is reserved for another course. It fell short in four ways. Its "ambitious" note on #714 couldn't tell me the fix was a two-repo port; only reading the code did. It can't tell when an issue is already fixed on `main` (the third time this has come up), or when a repo only accepts pull requests from approved contributors or after a maintainer says yes. And it grades the whole issue, so it rejected cpython#133049 even though maintainers had agreed on half of it. The scope patch helped me compare accepts instead of treating them as equal, but "ambitious" now covers both "one open question" and "a port across two repos", which are very different amounts of work. Claim comment: https://github.com/catalyst-cooperative/pudl/issues/5566#issuecomment-6380898918

**Toolkit**

- Tool: issue-scout, my search strategy, rubric, and fit profile. I ran one scan of GitHub and graded about two dozen issues in direct mode (`claude -p "issue-scout: grade <URLs>"`), most of them picks from my own screen of the Issue Selection site.
- What it produced: for pudl#5566, accept with every required check passing, 3 of 3 preferred checks, and a "moderate" difficulty note. It also noted that the maintainers assign the issue from a comment, and that PUDL's LLM policy asks for a short note in the PR on how AI was used.
- Difficulties: it accepted #714 without seeing how big the port was, and it has no check for a fix already on `main`, contribution gates (approved-contributor lists, a maintainer go-ahead, labels held for another course), or a settled half inside a disputed issue. `no-abandoned-history` also never says how many closed PRs count as "several".
- What changed: I applied the scope patch. I replaced my `single-bounded-goal` check with `scope-feasible`, which only fails an issue that isn't one piece of work, and added a difficulty note (straightforward, moderate, or ambitious) to each accept. I also changed the verdict rule to "every check weighted required" instead of a hand-copied list of names, so renaming a row can't silently drop it. My Unit 1 eval still agreed 7 of 7 with the gold labels on every scope item plus two canaries. Next, I'd add checks for "still needed on `main`" and for contribution gates, and put a number on "several" closed PRs.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
