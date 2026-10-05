# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of my plan, the branch I built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

Akhan521

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5966923225

Text as posted:

````markdown
## Fix plan

Following up on [my reproduction](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5840813911). The controls there narrow the cause to line 38 of `rag/evaluator/faithfulness_checker.py`: `[{}]` scores `0.0`, a string scores `1.0`, and only a present `text: None` gets past the `""` default and crashes the join.

**Change:** `chunk.get("text") or ""` on that line, so `None` behaves exactly like a missing key. Skipping the chunk would give the same score with an extra branch, and raising would break `test_none_context_chunk_text`, which expects a float back. I'll also remove that test's `xfail(strict=True)` marker, as CONTRIBUTING asks, and add one test where a `None` chunk sits next to a real one, to check the rest of the context still counts. If you'd rather reject `None` text earlier, I can switch.

**How I'll check it:** re-run the issue snippet (expect `0.0` and no traceback), run `test_none_context_chunk_text` without `--runxfail` (expect `1 passed`), confirm both controls are unchanged, then `make check && make test-unit`.

#74 makes the same line-38 change, which I reached separately from my repro. Mine adds the mixed-chunk test, and if #74 lands first I'll rebase and offer just that test.

Out of scope: `RelevanceScorer` crashes on the same input (`RelevanceScorer().score('python skills', [{'text': None}])` raises `AttributeError: 'NoneType' object has no attribute 'lower'` on the same commit), and `EvalSuite.run()` calls it first, so a `None` chunk still crashes the suite after this fix. It isn't tracked yet, so I can open a separate issue for it.
````

---

## Your branch

**Branch**

fix/60-none-context-chunk-text

**Evidence**

My Unit 2 reproduction steps (the issue snippet, the xfailed test, and the two controls),
plus the whole test file, run on macOS 26.6.2 arm64 with Python 3.11.15 in the same `.venv`
as Unit 2.

Before, on `main`:

```text
$ git rev-parse --short HEAD
f89c06f

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print(repr(FaithfulnessChecker().check('Knows Python.', [{'text': None}])))"
Traceback (most recent call last):
  File "<string>", line 2, in <module>
  File "~/Developer/pathreview-ai301-fa26-s1/rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found

$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -q
21 deselected, 1 xfailed in 0.07s

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print(repr(FaithfulnessChecker().check('Knows Python.', [{}])))
print(repr(FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python well'}])))"
2026-10-03 01:04:17 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
2026-10-03 01:04:17 [info     ] faithfulness_checked           claims_count=1 score=1.0 supported_count=1
1.0

$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -m unit -q
18 passed, 4 xfailed in 0.08s
```

After, on `fix/60-none-context-chunk-text`:

```text
$ git rev-parse --short HEAD   # fix/60-none-context-chunk-text
684fabe

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print(repr(FaithfulnessChecker().check('Knows Python.', [{'text': None}])))"
2026-10-03 01:12:41 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0

$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -q
1 passed, 22 deselected in 0.06s

$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print(repr(FaithfulnessChecker().check('Knows Python.', [{}])))
print(repr(FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python well'}])))"
2026-10-03 01:12:42 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
2026-10-03 01:12:42 [info     ] faithfulness_checked           claims_count=1 score=1.0 supported_count=1
1.0

$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -m unit -q
20 passed, 3 xfailed in 0.07s
```

Repo checks on the branch:

```text
$ make lint
All checks passed!
$ black --check .
110 files would be left unchanged.
$ make typecheck
Success: no issues found in 76 source files
$ make test-unit   # on main
375 passed, 53 xfailed, 4 warnings in 4.55s
$ make test-unit   # on fix/60-none-context-chunk-text
377 passed, 52 xfailed, 3 warnings in 10.86s
```

The crash is gone: the snippet now returns `0.0`, the same as the missing-key control.
`test_none_context_chunk_text` passes without `--runxfail`, both controls are unchanged, and
the suite gains exactly two passes (the un-xfailed test and the new mixed-chunk test) with
nothing else moving.

## Eval iterations

**Run history**

1. Calibration only (`--include-calibration --only calib-01,calib-02,calib-03,calib-04`): 4/4
   agreed with the worksheet labels (unscored).
2. Full run 1: **18/20** (bar: PASS); categories clear-accept 5/7, scope-creep 4/4,
   thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. Both misses were false rejects of
   clear accepts: pkg-02 on `scope-bounded` and pkg-03 on `claims-backed`.
3. Partial re-grade after revising those two checks (`--only pkg-02,pkg-03,pkg-06,pkg-15,pkg-19`,
   the last three as scope-creep canaries): 5/5. The canaries still rejected on `scope-bounded`.
4. Full run 2: **20/20** (bar: PASS); every category matched. This was the run I first submitted.
5. Post-grade revision. Grader feedback: `scope-bounded` and `fix-acts-on-cause` both failed
   pkg-04 for the same mistake, and I should enumerate every gate a repo enforces. I moved the
   `scope-bounded` seam and added gates to `repo-asks-planned`, then re-graded
   `--only pkg-04,pkg-06,pkg-12,pkg-15,pkg-19,pkg-02,pkg-09,pkg-20`: 8/8. pkg-04 now passes
   `scope-bounded` and still rejects. All four scope-creep canaries still fail `scope-bounded`.
6. Full run 3: **20/20** (bar: PASS); every category matched (clear-accept 7/7, scope-creep 4/4,
   thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). This is the committed `eval-run.txt`.

**Package analysis**

pkg-04 (junegunn/fzf#4260, category thread-convention). My rubric decided **reject** and the
gold label is **reject**, so they agree.

The plan's diagnosis is fine. "fzf keeps reading console input while an `execute` child runs"
predicts all three results in the repro: keys landing in fzf's prompt, the Ubuntu control
working, and the `> /dev/tty` control working. So `diagnosis-fits-evidence` passed. What sinks
the package is that it answers a code bug with documentation, in a thread where the owner had
already gone further. The run 3 notes:

- `follows-thread` failed: "Owner isolated light_windows.go lines 70-84 and posted a patched test
  binary (8916cbc); the comment mentions neither." This is the check the category was built for.
  A comment that proposes docs without mentioning either sends the maintainer back to their own
  thread.
- `fix-acts-on-cause` failed: "Docs only; 'Not in scope: any change to fzf's input handling code'
  leaves the named defective path untouched, a workaround; no maintainer said intended." The
  plan's own stated cause lives in the input handling, and the plan rules that out of scope.
- `comment-carries-plan` failed: "Comment states what and where but nothing about how the work
  will be checked."
- `scope-bounded` **passed**: "Man page note, README examples and FAQ entry all serve the single
  docs core change; nothing deferred-worthy or unrelated added."

That last line is the revision. In runs 1 and 2, `scope-bounded` also failed pkg-04, because
every docs item "fixes or tests" nothing and so failed the removal test. That was the same
mistake as `fix-acts-on-cause` firing in a second check. The verdict was right, but the failure
list told the author two things were wrong when only one was. Now `scope-bounded` measures items
against the plan's own core change, so a workaround-only plan fails once, in the check that owns
that decision.

**Check rationale**

From `tools/plan-check/rubric.md`:

```
| scope-bounded | The plan's committed changes and files or areas, including any its Deviations section records as added during the build, read against the plan's core change: the change it makes at the code path where the reported defect lives, or, if it makes none there, the change it presents as resolving the issue. | Passes if every committed change is the core change, its verification, or needed for them. Apply a removal test to each other item: if dropping it would leave the core change and its verification intact, it is extra. A core change that reaches past the faulty code path (rewriting or restructuring the surrounding module, subsystem, or other constructs to deliver the fix) counts as extra beyond the part that fixes that path. Fixing the same faulty operation at another site the issue or diagnosis identifies as the same defect is part of the core change; a similar pattern found elsewhere in the code is extra unless deferred. Regression tests, removing markers or suppressions that track this bug, docs for behavior the change alters, changes the repo's enforced gates (pre-commit hooks, CI jobs, lint, format, or type checks) require of the files the change touches, and reading or auditing nearby code without changing it all stay in scope. Extra work the plan explicitly defers or splits out ("not in scope", "separate issue") is fine. Fails if the plan commits to extra work: a rewrite or refactor, a migration, a dependency upgrade, a new option, setting, or UI, a new framework or abstraction, a test-harness or CI overhaul, or fixing other bugs while in the area. This check judges only whether the plan does more than its core change, never whether the core change fixes the defect. | required |
```

The row has been revised twice, each time to fix the seam rather than special-case a package.

1. **After full run 1.** The removal test asked whether dropping an item "would still leave the
   reported behavior fixed and tested". On pkg-02 that made the clamp at the sibling site (line
   795) look extra, even though the issue names it as the same defect. So "the same faulty
   operation at another site the issue or diagnosis identifies as the same defect" became part
   of the fix, and "a similar pattern found elsewhere in the code is extra unless deferred" kept
   that from swallowing scope creep.
2. **After grading.** The removal test was still measured against "the reported defect fixed".
   That meant a plan with no real fix failed it on every item, duplicating `fix-acts-on-cause`.
   The seam moved: items are now measured against the plan's **core change** (the change at the
   faulty code path, or failing that, the one it presents as the fix). The row ends by giving up
   the other question explicitly: "This check judges only whether the plan does more than its
   core change, never whether the core change fixes the defect."
   - **What stops a redesign from calling itself the core change:** "A core change that reaches
     past the faulty code path ... counts as extra beyond the part that fixes that path." Without
     it, pkg-12's printer rebuild and pkg-19's state machine could have passed.
   - **Gate-required lines:** I also added changes "the repo's enforced gates ... require of the
     files the change touches" to the in-scope list. That settles by rule the mypy annotations my
     own build needed, which a grader had previously passed on judgment.
   - **Deviations:** the Evidence column now includes changes a Deviations section records, so
     build-time additions get scoped too.

I rejected two alternatives. One was a pkg-04-specific carve-out ("documentation plans are in
scope"), which would fix one package's shape, not the seam. The other was removing
`scope-bounded`'s removal test entirely and relying on the list of banned changes. The list
can't tell pkg-20's necessary new counter from pkg-19's unnecessary state machine.

**Trade-offs**

`scope-bounded` gives up three things.

1. **A real fix for the same pattern elsewhere gets held.** Under "a similar pattern found
   elsewhere in the code is extra unless deferred", a plan that also fixes an identical bug in a
   neighboring component fails unless it defers it. My own plan pays for this: `RelevanceScorer`
   crashes on the same `text: None` input, and `EvalSuite.run()` calls it first, so after my fix
   the suite as a whole still crashes on a `None` chunk. I think deferring is right for
   reviewability, but the user-visible crash outlives this PR.
2. **It no longer says anything about a plan that fixes nothing.** Measuring against the plan's
   own core change means a docs-only or workaround-only plan passes `scope-bounded` as long as it
   stays small. That is deliberate, since `fix-acts-on-cause` owns that failure. But it means the
   verdict now depends on that one check to catch workarounds: if `fix-acts-on-cause` is ever
   loosened, nothing backs it up. I checked that the move didn't open a hole the other way. I
   re-ran pkg-04 as the target, all four scope-creep packages as canaries, and pkg-02, pkg-09,
   and pkg-20 as accept or in-scope guards: 8/8. The scope-creep packages still failed
   `scope-bounded` on the containerd bump, the undici migration, the printer restructure, and the
   state-machine rewrite. The confirming full run held 20/20.
3. **"Core change" is a judgment call.** The grader has to pick which change is the core one. For
   a plan that changes the faulty path it is clear-cut. For a plan that doesn't, the fallback
   ("the change it presents as resolving the issue") trusts the plan's own framing. A plan that
   presented a redesign as the fix and touched nothing at the faulty path would pass
   `scope-bounded` and rely on `fix-acts-on-cause` again. I accept that, rather than make
   `scope-bounded` re-judge the fix.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; my skill's files in
`tools/plan-check/`.
