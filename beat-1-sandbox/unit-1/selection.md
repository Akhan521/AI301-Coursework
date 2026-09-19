# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
No AI disclosure checkbox or AI ban anywhere — confirms `ai-contribution-allowed` passes for all three. Now I have everything needed to grade.

## Summary

All three come from the scoped repo (`codepath/pathreview-ai301-fa26-s1`), opened by a Collaborator (course staff), each a single seeded bug with repro steps, named failing tests, and a "good first issue" label. Repo is active (pushed 2026-09-16, 3 days ago) and unarchived; `CONTRIBUTING.md`/PR template state no AI restriction. None are claimed by an assignee or open PR (issue #69 has a classmate's "I'll take this" comment, but the Path Review house rule says classmate claims never block).

**Ranked (all accepted):**

1. **#60 — Faithfulness checker crashes on `text: None`** — best fit: it's a RAG *evaluator* bug (faithfulness scoring), squarely in the eval work you want more of, and it's the most self-contained (one function, one repro, zero threads to untangle).
2. **#69 — Output parser crashes on JSON array fallback** — LLM output-parsing/generator-side bug, also good fit for LLM systems work; slightly busier (a classmate is already actively reproducing it in the thread, and you also drop an `xfail` marker as part of the fix, which is normal but one more step).
3. **#57 — Tech detector counts vendored/build files** — passes everything required, but it's generic file-classification logic under `agent/tools/`, not really eval/LLM-systems work, so it fits your stated interests least among the three.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Default-branch commit on 2026-09-16 (3 days before today 2026-09-19), well within 90 days."},
      {"name": "repo-active", "grade": "pass", "evidence": "Not archived; pushedAt 2026-09-16T21:48:27Z, within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single defect: `.get('text','')` doesn't cover a present `None` value; one repro snippet, one related test named."},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: [], comments: [] — no claim, no linked PR."},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI-use restriction or disclosure requirement."},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labeled 'good first issue' and opened by author_association COLLABORATOR (Aburke225)."},
      {"name": "actionable-issue", "grade": "pass", "evidence": "Exact repro code and observed error given; names the covering test `test_none_context_chunk_text`."},
      {"name": "ci-or-tests-present", "grade": "pass", "evidence": "Repo has `.github/workflows/ci.yml` and `eval.yml`; a specific unit test already exists for this bug."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Same repo-wide commit recency: last commit 2026-09-16."},
      {"name": "repo-active", "grade": "pass", "evidence": "Not archived; pushed 2026-09-16, within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One bug (list vs dict in `_parse_json_output`) plus removing its own xfail marker — coupled to the same fix, not separate work."},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: []; classmate jacho15 (authorAssociation NONE) commented 'I'd like to attempt this,' but the Path Review house rule says classmate claims don't block."},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "No AI restriction in CONTRIBUTING.md or PR template."},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labeled 'good first issue', opened by COLLABORATOR Aburke225."},
      {"name": "actionable-issue", "grade": "pass", "evidence": "Names exact files, gives estimated effort (2-4h), and a classmate's comment already confirms the exact reproduction and traceback."},
      {"name": "ci-or-tests-present", "grade": "pass", "evidence": "Covering xfail test `tests/unit/test_output_parser.py` already exists; repo CI runs it."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Same repo-wide commit recency: last commit 2026-09-16."},
      {"name": "repo-active", "grade": "pass", "evidence": "Not archived; pushed 2026-09-16, within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single defect: `tech_detector.py` fails to exclude `node_modules/`/`build/`; one repro, two named failing tests for the one fix."},
      {"name": "not-claimed", "grade": "pass", "evidence": "assignees: [], comments: [] — no claim, no linked PR."},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "No AI restriction in CONTRIBUTING.md or PR template."},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "Labeled 'good first issue', opened by COLLABORATOR Aburke225."},
      {"name": "actionable-issue", "grade": "pass", "evidence": "Exact repro code and observed vs. expected output given; names two failing tests."},
      {"name": "ci-or-tests-present", "grade": "pass", "evidence": "Repo has CI workflows; named unit tests already cover this bug."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `agreement: 2/3 scored items` (smoke run, `--limit 3`)
2. `agreement: 1/1 scored items` (`--only issue-01`, after fixing `bounded-scope` to stop reading a single task's implementation outline as an umbrella issue)
3. `agreement: 9/10 scored items` (smoke run, `--limit 10`)
4. `agreement: 2/2 scored items` (`--only issue-01,issue-04`, after adding the "trails off with 'etc.'" and thin-writeup carve-outs to `bounded-scope`)
5. `agreement: 7/10 scored items` (`--only issue-11` through `issue-20`)
6. `agreement: 4/5 scored items` (`--only issue-19,issue-01,issue-04,issue-15,issue-20`, after adding the abandoned-attempts, unresolved-debate, and unconfirmed-feature-request clauses to `bounded-scope`)
7. `agreement: 0/1 scored items` (`--only issue-01`, diagnostic re-run after the above edit regressed issue-01)
8. `agreement: 5/5 scored items` (`--only issue-01,issue-04,issue-15,issue-19,issue-20`, after rewriting the "independent work" test in `bounded-scope` to turn on distinct goals rather than file count)
9. `agreement: 19/20 scored items  (bar: 18/20: PASS)` (first full run)
10. `agreement: 0/1 scored items` (`--only issue-19`, diagnostic re-run after the full run above)
11. `agreement: 5/5 scored items` (`--only issue-19,issue-01,issue-04,issue-15,issue-20`, after separating "bundled work" from "one problem, several candidate approaches" in `bounded-scope`)
12. `agreement: 20/20 scored items  (bar: 18/20: PASS)` (full run)
13. `agreement: 1/1 scored items` (`--only issue-01`, sanity check after swapping a bundle-specific example for a generic one)
14. `agreement: 20/20 scored items  (bar: 18/20: PASS)` (full run, confirms the rubric was unaffected by the wording-only swap)
15. `agreement: 3/3 scored items` (`--only issue-20,issue-01,issue-04`, after softening the unconfirmed-feature-request clause)
16. `agreement: 20/20 scored items  (bar: 18/20: PASS)` (full run)
17. `agreement: 20/20 scored items  (bar: 18/20: PASS)` (final confirming run, saved to `eval-run.txt`)

**Issue analysis**

`issue-19` — "Selecting large subgraphs in proof mode freezes the UI." My rubric's final verdict: **accept**. Gold label: **accept**.

The issue is a maintainer's own bug report: two named causes ("matchers are slow for certain rewrites," "UI update waits for the matching thread") plus three additional implementation suggestions (multi-processing, category-scoped matching, running the rewrite itself in a separate thread). Early versions of my `bounded-scope` check misread this twice. First, it read the numbered list of suggestions as bundling several separate pieces of work under one issue, rather than several candidate approaches to fixing one problem. Second — after I fixed that — it independently flagged the same list as an "unresolved design debate," when it was really just a maintainer thinking out loud about how to fix their own bug, not unresolved disagreement between people. My rubric produced the correct `accept` only once `bounded-scope` explicitly distinguished "a single problem whose body lists multiple causes or candidate approaches" (one piece of work, however itemized) from both "genuinely separate pieces of work bundled together" and "unresolved disagreement between multiple people." A maintainer proposing several ways to fix their own bug is normal scoping detail, not a scope failure.

**Check rationale**

From `rubric.md`, the `not-claimed` check:

> The issue's assignees, any linked pull requests referencing it (open or closed-unmerged), and any claim comments in its thread ("I'll take this" / "working on this") together with whether a maintainer replied. | Fails if the issue has an open assignee, an open linked PR actively working it, or a claim comment dated within the last 30 days that a maintainer acknowledged with no sign of abandonment. Otherwise passes.

The 30-day window is deliberate: long enough that a claim comment from last week still blocks the issue, short enough that a claim from months ago with nothing to show for it doesn't. I picked a fixed number rather than an adjective ("recent") specifically so a different grader applying the same rubric to the same issue would land on the same verdict.

**Trade-offs**

The 30-day cutoff can't tell "abandoned" apart from "quiet but still working." Someone who claimed the issue 25 days ago and is genuinely making progress off-thread reads identically to someone who claimed it and vanished — both pass as "recently claimed" and the issue correctly stays rejected either way, but a person who claimed it 35 days ago and is still actually working on it (just slow, or not posting updates) would get incorrectly read as unclaimed, and the issue would be offered up as available when it isn't. I accept this miss: without contacting the claimer directly there's no way to distinguish silence-from-abandonment and silence-from-being-heads-down, and a fixed threshold is a more consistent, reproducible failure mode than trying to have the model guess intent from tone.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time:** Issue #60 is a bug in the RAG pipeline's faithfulness checker, the part that checks whether a generated answer is actually backed up by what it retrieved. That's real eval work, which is exactly what I want to get better at in this course. It's also small and self-contained (one function that doesn't handle a missing value correctly), so it's a reasonable size for a first issue while I'm still learning my way around this codebase.

2. **What the verdict identified vs. what I weighed:** The rubric confirmed the basics: the repo is active, nobody else has claimed the issue, there's no rule against AI-assisted contributions, and the issue gives me enough to actually start (a repro and a named test). What it can't tell me is why I'd pick this one over the other two it also accepted. It ranked #60 as the best fit for eval and RAG work, but deciding that this specific bug will teach me more about how RAG evaluation breaks than the other two lower-stakes bugs would is a judgment call about what I actually want to learn. That part's on me, not the rubric.

3. **Anticipated difficulty:** Should be low to moderate. It's a narrow bug, and there's already a test that names the exact scenario, so I don't think the fix itself will be ambiguous once I'm looking at the code. The real cost is just getting familiar with how this codebase represents context chunks and feeds them into the faithfulness checker, since I haven't worked in it before.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
