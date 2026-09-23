# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | The repo's recent commit history on its default branch, and how quickly a maintainer, owner, or collaborator has replied to recently opened issues. | Passes if at least one of the last 5 default-branch commits was made within the last 90 days, OR a maintainer, owner, or collaborator has replied to a recently opened issue within 30 days. | required |
| repo-active | Whether the repo is marked archived, its most recent release, its most recent push to any branch, and its star count. | Fails if the repo is archived. Otherwise passes if at least one holds: a release within the last 12 months, a push to any branch within the last 90 days, or a star count of 50 or more. | required |
| single-bounded-goal | The issue's description and its full comment thread. | Fails if the issue itself asks for its work to be split into separate tracked issues or PRs, or bundles multiple pieces of work that serve genuinely different goals under one issue — each one something that would be worth merging on its own even if the others were dropped, for example an unrelated bug fix bundled with a new feature. Whether work is "independent" turns on distinct goals, not on how many files or sections it touches: a single goal that requires coordinated edits across several files (renaming a public API and updating every call site so nothing is left broken, or fixing a bug and updating the test that should have caught it) is one piece of work, coupled by that one goal, not several bundled ones. The same holds for a single problem whose body lists multiple possible causes, candidate implementation approaches, or the steps it takes to reach one outcome: fixing one bug by trying one of several suggested approaches, or shipping one feature via an outlined sequence of steps, is one piece of work however it is itemized. A terse body, a bug report without repro steps, or a short list of concrete examples that trails off with "etc." does not by itself fail this check — that is normal shorthand for a bounded category of small, similar fixes, not open-ended scope. Weigh a maintainer/collaborator opener or a "good first issue" label toward passing when the write-up is thin. | required |
| no-unresolved-debate | The issue's full comment thread. | Fails if the thread shows unresolved disagreement between multiple people with no maintainer decision recorded. A maintainer's own issue body weighing several candidate causes or implementation approaches for the one problem it opens with is normal scoping detail, not unresolved debate, and does not fail this. | required |
| not-core-internals | The issue's description and its full comment thread. | Fails if a maintainer states the fix touches core internals. Otherwise passes. | required |
| not-support-question | The issue's description. | Fails if the issue is a pure usage/support question ("how do I get this to work?") rather than a proposed change. Otherwise passes. | required |
| no-abandoned-history | The issue's linked pull requests (open and closed-unmerged) and its comment thread. | Fails if the issue's history shows a pattern of repeated abandoned attempts — several closed, unmerged linked pull requests, or several different contributors claiming it and later going quiet — which signals the real difficulty exceeds what any label promises. Otherwise passes. | required |
| feature-request-confirmed | Whether the issue proposes a substantial new feature, and whether anyone with standing on the project has engaged with it. | Fails only if the issue proposes a substantial new feature — not a small, incremental change in the spirit of what the project already does, and not a confirmed bug or a task a maintainer proposed — and shows zero engagement from anyone with standing on the project: no maintainer or collaborator has commented on it in any way (including just a clarifying question), and whoever opened it has no established relationship to the project (not a contributor, collaborator, member, or owner). A small enhancement, a request with any maintainer engagement at all, or one opened by someone with existing standing does not fail this — the bar is a substantial, completely unvetted proposal, not the mere absence of a "yes." | required |
| not-claimed | The issue's assignees, any linked pull requests referencing it (open or closed-unmerged), and any claim comments in its thread ("I'll take this" / "working on this") together with whether a maintainer replied. | Fails if the issue has an open assignee, an open linked PR actively working it, or a claim comment dated within the last 30 days that a maintainer acknowledged with no sign of abandonment. Otherwise passes. | required |
| ai-contribution-allowed | The repo's stated policy on AI-assisted or AI-generated contributions, documented in places like a contributing guide, a dedicated AI-usage policy file, or pull request/issue templates. | Fails only if the policy states an outright ban on AI-generated or AI-assisted contributions. Conditions such as disclosure, personal understanding, testing, or human review are not a fail — they are terms to follow. No stated policy passes. | required |
| good-first-issue-signal | The issue's labels, and whether the opener has a maintainer/collaborator badge. | Passes if the issue carries a "good first issue", "help wanted", or equivalent label, OR was opened by someone with an Owner, Member, or Collaborator badge. | preferred |
| actionable-issue | The issue body and comment thread. | Passes if the issue gives enough to start work without first asking the maintainer for more information: a reproduction case or error trace, an explicit acceptance criteria list, or a maintainer-outlined implementation approach. This is about whether you can start today, not about the size of the work (that's `single-bounded-goal`); an issue can be perfectly bounded and still leave out what you'd need to begin. | preferred |
| ci-or-tests-present | The repo's CI configuration or test coverage of the area the issue touches — a CI status badge, a `.github/workflows/` directory, or a visible test suite. | Passes if a CI configuration or relevant tests are visible. | preferred |

## Verdict rule

Accept only if every required check (`maintainer-active`, `repo-active`, `single-bounded-goal`, `no-unresolved-debate`, `not-core-internals`, `not-support-question`, `no-abandoned-history`, `feature-request-confirmed`, `not-claimed`, `ai-contribution-allowed`) grades `pass`. Reject if any required check grades `fail`. `unclear` on a required check counts as `fail`: a first issue whose evidence cannot actually be verified is not a first issue to take on faith.

`good-first-issue-signal`, `actionable-issue`, and `ci-or-tests-present` never change the verdict. Among accepted issues, rank higher the ones that pass more of these three, and say in the fit summary which of them made the top-ranked issue a better bet — an accepted issue that also looks actionable and sits in a tested repo is a safer first PR than one that merely clears the required bar.
