# Playbook: Aamir Khan

My GitHub login is Akhan521; every thread entry by it is mine. Where a rule here differs from a move form, my rule decides.

## Response rules

- My first sentence answers their point: what I changed and in which commit, or where they can see it's already handled. A first reply to a review thanks them for the specific catch inside that sentence, not on a line before it.
- The draft states only results already in the PR or thread. Anything else (a new result, who to tag) is a [slot] I fill before sending.
- I ask one question per decision that's theirs, answerable from their chair, and point to their earlier comment instead of re-explaining.
- I push the whole round, then reply once. A correction to anything I said or shipped goes out at once, even after merge. A round that will take days gets a short note saying what's coming.
- An ask that would grow the PR past its issue, I offer as a follow-up and ask whether it blocks this merge. After one evidenced no, I do it their way unless it breaks something. If two maintainers disagree, I quote both and ask them to pick.
- I change the PR only for what a maintainer asks or agrees to. A comment that asks nothing of me gets no reply.
- Branch updates (this overrides the rebase plan): the repo's guide decides. If it says nothing and anyone but me has commented, I merge the base and push, never rebase and force-push. I update an approved branch only when it conflicts.
- No "!" right after an @tag, and no excuses when I fix a mistake.

## Staleness thresholds

- The clock runs only when I spoke last: from my newest comment, or from opening if nobody has responded. My own pushes don't restart it. If a maintainer's comment is newest and asks something of me, the move is my answer, never a nudge or a wait. If it asks nothing ("LGTM, after the release"), the clock counts from it.
- Each repo gets its own number, measured the way my measuring.md says and re-measured after 3 months. A repo not listed here has no threshold yet: the move is a wait note telling me to measure first. My own deadlines never move the number.
- catalyst-cooperative/pudl (measured 2026-11-02): 14 days. Its maintainer guide aims to respond within a week and to check in when a review runs past 2 weeks. Across 21 outside PRs, the first maintainer reply took about 5 days at the median and 12.6 at the second-slowest. No stale-bot.

## Nudge etiquette

- One nudge per quiet stretch, and only with something new (the branch current with checks green, a new result, or a narrower question). Two sentences at most: what's new, and one ask for the next thing only they can do (a review, a CI approval, or the open question), whenever they have time.
- It tags one person: the maintainer who last engaged with this work and is still active. No list of @-tags, no urgency, no "any update?", no mention of my own deadlines.
- If the repo's guide describes a ping path (where, when, how many steps), it replaces these defaults.
- If the PR's future is in doubt (a stale label, a maintainer's overlapping PR), I don't wait for the threshold: one comment saying the branch is current and I still want it, or asking whether to close it in favor of theirs.
- There is never a second nudge.

## Walk-away criteria

- I close my own PR if a maintainer says they don't want the change, the same fix lands another way, they want it done in a way I can't deliver, or I can't finish it. If a bot closes it, I reopen once with something new, or walk away.
- Silence alone never closes my PR. After an unanswered nudge and one more threshold, I stop posting, leave it open, and answer if anyone comes back.
- The close note is dated, says why in one sentence, thanks whoever looked at it, and says where the work lives if anyone wants it later. If my own close note is the newest entry, the move is nothing.
