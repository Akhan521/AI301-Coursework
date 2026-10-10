# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/115

**Branch**

`fix/60-none-context-chunk-text`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1: **19/20** (bar PASS, every category matched). One false reject: pkg-05 (clear-accept), `failed: fix-observed`.
2. `--only pkg-05,pkg-04,pkg-07,pkg-14,calib-04 --include-calibration`, after revising `fix-observed`: **4/4** scored agree, and calib-04 still rejected. pkg-05 flipped to accept; the three not-tested canaries and calib-04 still rejected on `fix-observed`.
3. Full run 2: **18/20** (bar PASS, every category matched). Two clear accepts that passed in run 1 flipped: pkg-11 `failed: description-matches-diff` and pkg-13 `failed: fix-observed`.
4. `--only pkg-11,pkg-13,pkg-03,pkg-17,calib-03 --include-calibration`, after fixing both checks: **4/4** scored agree, and calib-03 still rejected. pkg-11 and pkg-13 accepted; the silent-drift canaries pkg-03 and pkg-17 and calib-03 still rejected on `description-matches-diff`.
5. Full run 3, the confirming run committed in `eval-run.txt`: **20/20** scored items (bar PASS), `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

**Package analysis**

**pkg-05** (nushell/nushell#18848, clear-accept). Gold label: **accept**. My rubric's verdict in full run 1: **reject**, failing `fix-observed`. In the committed run: **accept**.

The plan's test plan names two things to verify: "re-run the issue's script, expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row plus a warning". The test evidence shows full before/after tables for the first. For the second it only says "Same-key redefine prints the one-time warning." My first `fix-observed` read "Passes if every trigger or failure mode the test plan says it will re-run is shown re-run on the changed code", so a prose line for the second item failed the whole PR.

That reading was too literal. The same-key warning isn't a failure the reproduction recorded. It's a new behavior the fix adds, and the diff pins it with `same_name_same_key_replaces_with_warning`, which asserts `assert_eq!(warnings.len(), 1)`, inside a `cargo test -p nu-protocol` run the evidence reports passing. A maintainer wouldn't hold the PR for that. So I split the rule: every failure the reproduction recorded must still be shown re-run, but "any other outcome the test plan names (a new behavior or edge case the change adds) may be shown the same way, or by an added test that drives it and asserts it, reported passing." In the committed run the check passes with: "Issue repro re-run after shows both atuin rows (control/char_r and none/up); the same-key warning is pinned by added test same_name_same_key_replaces_with_warning, reported passing".

**Check rationale**

> | fix-observed | The test evidence, read against the plan's test plan and each failure the reproduction recorded. | Passes if every failure the reproduction recorded is shown re-run on the changed code, naming what was run (a command, an input, or UI steps) and showing a specific result a reviewer can compare with the plan's expected-after: captured output, a value, an exit code, exact on-screen text, or a screenshot. The before may be shown or cited from the reproduction. Any other outcome the test plan names (a new behavior or edge case the change adds) may be shown the same way, or by an added test that drives it and asserts it, reported passing. A failure or outcome left out passes only if the description says so and why. Fails if the evidence asserts success without a specific observed result, exercises only a control or a path the change does not touch, or silently skips a recorded failure or a named outcome. Control runs (cases that already worked before the change), and whether the repo's suite and checks ran, are not this check's question. | required |

It reads this way because of two revisions, each driven by a false reject.

- **"Each failure the reproduction recorded" vs "any other outcome".** The first version required every item in the test plan to be shown re-run, which held pkg-05 over a new warning that an added test already pinned. Maintainers read PRs this way: the bug that was reported has to be demonstrated fixed, and the secondary behaviors a fix adds can be pinned by tests. The line still fails calib-04, because its skipped `-j 200000 -x echo` abort is a recorded failure with no test.
- **"Control runs … are not this check's question."** My pkg-05 revision added "silently skips … a named outcome", and in full run 2 the grader read controls into that: pkg-13 failed because "the plan's no-ampersand control is only asserted ('No-ampersand control unchanged') with no command or output". That was a regression I introduced. This check is about proving the fix, not re-proving cases that already worked, so controls are out of it explicitly.

I kept "specific result a reviewer can compare" rather than accepting any statement of success, because that's what separates pkg-04's "colors work now in paging mode" (no command, nothing observed) from pkg-08's toast text quoted exactly.

**Trade-offs**

Letting an added test stand in for a shown re-run (for outcomes other than recorded failures) gives up some certainty: a test can pass while the real CLI or UI behaves differently. I accepted that only for secondary outcomes. To make sure the loosening didn't free real not-tested PRs, I re-ran the not-tested canaries pkg-04, pkg-07, and pkg-14 plus calib-04 with `--only`. All four still rejected on `fix-observed` for their own reasons, for example pkg-07: "The only re-run is the single-file ./onefile/ control; the plan's two-file repro … is never shown".

A case I accept missing: the `description-matches-diff` fix now treats "an imprecise count or a loose paraphrase of something the diff does contain" as a summary. So a description that miscounts what a present change touches ("One function touched" when it also adds a helper, as in pkg-11) no longer fails.

The other direction showed up on my own PR. `repo-checks-run` counts every check the repo names, even for code the diff doesn't touch. It held my backend-only PR until I ran CONTRIBUTING's frontend command (`npm test -- --run`, 18 passed). I kept it that way, because Path Review requires all five CI jobs to be green regardless of which files a change touches.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
