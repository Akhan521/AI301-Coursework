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

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/repro-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
