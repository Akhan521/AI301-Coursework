# Measuring a repo's staleness threshold

How I set the number that goes in my playbook's Staleness thresholds for a new repo. The monitor never reads this file; it only reads the result.

1. Take about 15 recent PRs by outside contributors (author association not OWNER or MEMBER), whatever happened to them: merged, closed, or still open. Counting only merged PRs measures the happy path.
2. For each, measure the days from opening to the first maintainer comment or review that engages with the change. Bots (check names, not just "bot" in them), labels, auto-mentions and CI triggers don't count. Skip PRs that never got one, and if most never did, write "low odds" in the playbook instead of a number.
3. The threshold is the slow end of normal: about the second- or third-slowest, not the slowest (one backlog PR shouldn't set it), rounded up to whole weeks, never under 7 days. With fewer than 8 usable PRs from the last year, use 14 days and mark it a guess.
4. Read the contributing and maintainer docs. If they state a response time, use the longer of stated and measured. If they describe a ping path (where, when, how many steps), note it. If a stale-bot runs, keep the threshold well inside its window.
5. Write one dated line in the playbook: repo, threshold, the source of the number, and any stale-bot or ping path. Re-measure when the line is over 3 months old.

Example, catalyst-cooperative/pudl, 2026-11-02: 21 outside `community` PRs (16 merged, 5 closed or open). First maintainer reply: median about 5 days; slowest 35.3 (#5419), second-slowest 12.6 (#5506), so 14 days. `docs/dev/managing_external_contributions.rst` aims to respond within a week and check in past 2 weeks; no stale-bot workflow.
