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
4. Full run 2: **20/20** (bar: PASS); every category matched (clear-accept 7/7, scope-creep 4/4,
   thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). This is the committed `eval-run.txt`.

**Package analysis**

pkg-04 (junegunn/fzf#4260, category thread-convention). My rubric decided **reject** and the
gold label is **reject**, so they agree.

The plan's diagnosis is fine. "fzf keeps reading console input while an `execute` child runs"
predicts all three results in the repro: keys landing in fzf's prompt, the Ubuntu control
working, and the `> /dev/tty` control working. So `diagnosis-fits-evidence` passed. What sinks
the package is that it answers a code bug with documentation, in a thread where the owner had
already gone further. The run 2 notes:

- `follows-thread` failed: "Comment ignores junegunn's 'This seems to be the culprit' pointer to
  src/tui/light_windows.go and the patched test binary from commit 8916cbc." This is the check
  the category was built for. The owner isolated the culprit and posted a patched binary for
  testing, and a comment that proposes docs without mentioning either sends the maintainer back
  to their own thread.
- `fix-acts-on-cause` failed: "Only man page, README and FAQ changes ... this documents a
  workaround. The owner named light_windows.go as the culprit rather than calling it intended."
  The plan's own stated cause lives in the input handling, and it rules that out of scope. The
  check's one exception, a maintainer calling the behavior intended, doesn't apply.
- `comment-carries-plan` failed because the comment never says how the docs will be checked. The
  plan's test plan does say (`test-decisive` passed), but the comment has to stand on its own.

One thing I don't like about this result: `scope-bounded` also failed ("None of the docs-only
items ... fixes or tests the reported defect, so each is extra by the removal test"). That is the
same underlying mistake as `fix-acts-on-cause` showing up in a second check. The verdict is
right, but a plan that only works around the bug shouldn't also count as scope creep. The cleaner
seam would scope the removal test to plans that contain a fix, and leave "no fix at all" to
`fix-acts-on-cause`. I didn't make that change, because the rubric is frozen to the committed
run's fingerprint.

**Check rationale**

From `tools/plan-check/rubric.md`:

```
| scope-bounded | The plan's list of changes and files or areas, read against the behavior the issue reports. | Passes if every change the plan commits to is needed to fix or verify the reported behavior. Apply a removal test to each item: if dropping it would still leave the reported defect fixed and tested everywhere the issue, thread, or the plan's diagnosis locates it, it is extra. Fixing the same faulty operation at another site the issue or diagnosis identifies as the same defect is part of the fix; a similar pattern found elsewhere in the code is extra unless deferred. Regression tests for the fix, removing markers or suppressions that track this bug, docs for behavior the fix changes, and reading or auditing nearby code without changing it all stay in scope. Extra work the plan explicitly defers or splits out ("not in scope", "separate issue") is fine. Fails if the plan commits to extra work, even alongside a correct core fix: a rewrite or refactor, a migration, a dependency upgrade, a new option, setting, or UI, a new framework or abstraction, a test-harness or CI overhaul, or fixing other bugs while in the area. | required |
```

This row came out of full run 1. Its original removal test asked whether dropping an item "would
still leave the reported behavior fixed and tested". On pkg-02 the grader applied that literally:
the repro is fixed by the line-934 clamp alone, so the plan's clamp at the sibling site, line
795, counted as extra. But the issue itself names line 795 as the same wide-char/tiny-width
defect, so leaving it out would ship a half fix. I widened the removal test to "the reported
defect ... everywhere the issue, thread, or the plan's diagnosis locates it". Then I added the
sentence that keeps the widening from swallowing scope creep: the same faulty operation at a site
the issue or diagnosis names is part of the fix, and "a similar pattern found elsewhere in the
code is extra unless deferred".

I rejected special-casing pkg-02, for example "sibling sites the issue lists are OK". That fixes
one package's wording, not the logic. I also rejected dropping the removal test for a list of
banned changes. The list alone can't tell a needed new mechanism from a gratuitous one: pkg-20's
generation counter is new code but necessary, while pkg-19's state machine is new code and not
necessary. The list stays as examples, but the removal test makes the call.

**Trade-offs**

`scope-bounded` gives up three things.

1. **A real fix for the same pattern elsewhere gets held.** Under "a similar pattern found
   elsewhere in the code is extra unless deferred", a plan that also fixes an identical bug in a
   neighboring component fails unless it defers it. My own plan pays for this: `RelevanceScorer`
   crashes on the same `text: None` input, and `EvalSuite.run()` calls it first, so after my fix
   the suite as a whole still crashes on a `None` chunk. The rubric pushed me to defer it to a
   separate issue rather than fold it in. I think that's the right call for reviewability, but
   it means the user-visible crash outlives this PR.
2. **Widening the removal test risked letting scope creep through.** Accepting "the same defect
   at another site" could have let pkg-06, pkg-15, or pkg-19 argue that their extra work was the
   same defect. So I re-ran those three as canaries alongside pkg-02 and pkg-03. All three still
   rejected on `scope-bounded`, citing the containerd upgrade, the undici migration and settings
   panel, and the state-machine rewrite. The confirming full run then held scope-creep at 4/4.
3. **Lines the repo's own tooling forces aren't covered.** My live plan-check after the build
   flagged a case the row doesn't decide. The pre-commit mypy hook forced two `list[dict]`
   annotations onto existing test lines. Read strictly, they fail the removal test, since the bug
   is fixed without them. The grader passed them as "required by the repo's own gate" and said
   the rubric should decide this explicitly. I recorded them under Deviations rather than edit
   the frozen rubric. A future revision should say that changes the repo's own commit gates
   require for the touched files stay in scope.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; my skill's files in
`tools/plan-check/`.
