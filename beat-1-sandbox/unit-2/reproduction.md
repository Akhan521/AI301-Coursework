# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Akhan521

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5840557778

Hi! I'd like to work on this one. I'll reproduce the `text: None` crash on current `main` (macOS), including the `test_none_context_chunk_text` test that's xfailed for it, and post what I find here before working on a fix.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5840813911

````markdown
## Reproduction (macOS)

Also reproduces on macOS with Python 3.11 (the version CI pins); the earlier report above was on Windows with Python 3.14. I also ran the test that's xfailed for this issue.

**Environment**
- macOS 26.6.2 (arm64)
- `main` @ `f89c06fc3ff292df2a04a39ac51319d32a76b779`
- Python 3.11.15
- Installed with `.venv/bin/pip install -e ".[dev]"` in a Python 3.11 venv (structlog 26.1.0, pytest 9.1.1); Docker, migrations, and the frontend skipped, since `check()` doesn't touch them.

**To reproduce**

1. Clone, check out `main` at the commit above, and install as above.
2. Run the snippet from the issue:

```
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
Traceback (most recent call last):
  File "<string>", line 2, in <module>
  File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
```

3. Run the xfailed test. As-is it reports `1 xfailed`; with `--runxfail` it fails the same way:

```
$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -q --runxfail
>       context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
E       TypeError: sequence item 0: expected str instance, NoneType found

rag/evaluator/faithfulness_checker.py:38: TypeError
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
1 failed, 21 deselected in 0.07s
```

**Controls (same session)**

```
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print(repr(FaithfulnessChecker().check('Knows Python.', [{}])))
print(repr(FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python well'}])))"
2026-09-25 15:35:44 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
2026-09-25 15:35:44 [info     ] faithfulness_checked           claims_count=1 score=1.0 supported_count=1
1.0
```

With the `text` key missing, `check()` returns `0.0`; with a string it returns `1.0`. Of these three inputs, only the one with `text` set to `None` crashes.

**Expected:** `check()` returns a float for a chunk whose `text` is `None`, as `test_none_context_chunk_text` asserts.

**Actual:** the `" ".join(...)` on line 38 raises on the `None` text, so `check()` never returns a score.

**Relevant files:** `rag/evaluator/faithfulness_checker.py` (line 38), `tests/unit/test_faithfulness_checker.py` (`test_none_context_chunk_text`).

I've only looked at `check()` itself, not at where `None` chunk text can come from upstream. Next I'll post a fix plan here before opening a PR.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Calibration packages only (unscored): 4/4 agreed with gold.
2. Full run 1: **18/20**. Misses: pkg-03 and pkg-05, both false rejects.
3. `--only` re-run of the two misses plus canaries, after revising two checks: **7/7** scored.
4. Full run 2, saved as `eval-run.txt`: **20/20**, every category matched.

**Package analysis**

**pkg-05** (conda/conda#16543). Gold: **accept**. Run 1: **reject**. Run 2: **accept**.

The report describes its `env.yml` ("a valid `dependencies:` list plus a `category:` section") instead of pasting it, but it shows the exact command and the `EnvironmentSectionNotValid` warning breaking the JSON output. Run 1 failed `steps-rerunnable` because "the file contents are never shown": my check only accepted inputs that were pasted or copied from the issue. That is a shape test, and it missed the real question, whether a stranger could rebuild the input. Here they can, because the one element that triggers the bug is named exactly. After I revised the check, run 2 passed it and matched gold.

**Check rationale**

| steps-rerunnable | The repro report's steps, from starting state through the trigger. | Passes if a stranger with only public resources could re-run the attempt: every command, input, file content, and setting needed is shown, taken verbatim from the issue with a pointer to it, or described precisely enough to rebuild, including the exact element that triggers the bug (for example "the default config file with one unrecognized top-level key added" is rebuildable; "our usual config" is not). Fails if the steps depend on private code, config, or data the reader cannot get, or if a step needed to reach the trigger is left out or only gestured at ("set up the project"). | required |

It originally passed only inputs that were "shown, or is taken verbatim from the issue", which false-rejected pkg-05 over a small config file that was described precisely. I added the "described precisely enough to rebuild" route so the check judges whether a stranger could re-run the steps, not whether the file was pasted. My first example was lifted from pkg-05's wording; I replaced it with a generic one so the check carries to real issues. The Fails clause is unchanged, so private inputs (pkg-18) still fail.

**Trade-offs**

Loosening the check risked flipping correct rejects, so I re-ran canaries with `--only`: pkg-20 (disclosure), pkg-18 (private repo), pkg-06, pkg-15, pkg-17, and calib-04. All still rejected on their intended checks. The case I accept it will miss is an input description that sounds precise but is quietly wrong; only actually re-running it would catch that.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
