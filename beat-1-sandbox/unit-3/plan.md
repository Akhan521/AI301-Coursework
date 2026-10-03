# Plan: #60, faithfulness checker crashes when a context chunk has `text: None`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60
Builds on my reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-5840813911

## Repro evidence this plan relies on

From my posted reproduction (macOS 26.6.2 arm64, `main` @ `f89c06f`,
Python 3.11.15, `pip install -e ".[dev]"`):

The issue's snippet crashes on line 38:

```
$ .venv/bin/python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker
FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
  File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
TypeError: sequence item 0: expected str instance, NoneType found
```

The test xfailed for this issue fails the same way when forced:

```
$ .venv/bin/pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -q --runxfail
E       TypeError: sequence item 0: expected str instance, NoneType found
rag/evaluator/faithfulness_checker.py:38: TypeError
1 failed, 21 deselected in 0.07s
```

Controls from the same session: a chunk with the `text` key missing
(`[{}]`) returns `0.0`, and a chunk with a string
(`[{'text': 'Knows Python well'}]`) returns `1.0`. Of the three inputs,
only `text: None` crashes.

## Related work on the thread

No maintainer has commented on #60. PR #74 (opened 2026-09-21, "Closes
#60", still open) makes the same line-38 change and removes the same
xfail marker. It also adds `list[dict]` annotations in the test file to
clear mypy `var-annotated` errors. My plan arrives at the same one-line
change independently from my reproduction. What it adds is a
mixed-chunk regression test and the `RelevanceScorer` finding below. If
#74 merges first, I'll rebase onto it and offer only the added test,
instead of a second copy of the same fix.

## Diagnosis

`check()` builds the context string with `chunk.get("text", "")`. The
`""` default applies only when the key is missing. When the key is
present with value `None`, `.get()` returns `None`, and `" ".join(...)`
raises on the non-string item. The controls fit this exactly: a missing
key takes the default and scores `0.0`, a string joins and scores `1.0`,
and only a present-but-`None` value reaches `join` as `None`.

## Scope

In scope: the context-building line in `FaithfulnessChecker.check()`,
and the unit tests for it.

Not in scope:

- `RelevanceScorer.score()` (`rag/evaluator/relevance_scorer.py`, line
  32) uses the same `chunk.get("text", "")` pattern and also crashes on
  `text: None`, with a different error (checked on the same commit):

  ```
  $ .venv/bin/python -c "from rag.evaluator.relevance_scorer import RelevanceScorer
  print(RelevanceScorer().score('python skills', [{'text': None}]))"
  AttributeError: 'NoneType' object has no attribute 'lower'
  ```

  It is a separate component, and #60 is about the faithfulness
  checker, so I'm leaving it out of this change and raising it on the
  thread instead.
- Where `None` chunk text comes from upstream (retrieval or
  ingestion). I haven't traced it, and this fix doesn't depend on it.
- Any change to scoring, claim extraction, or `_is_supported`.

## Files

- `rag/evaluator/faithfulness_checker.py`: line 38 in `check()`.
- `tests/unit/test_faithfulness_checker.py`: remove the
  `@pytest.mark.xfail(strict=True, reason="issue #60: ...")` marker from
  `test_none_context_chunk_text`, as CONTRIBUTING requires for seeded
  bugs, and add one test for a mix of `None` and string chunks.

## Approach

1. Change line 38 to treat a `None` value the same as a missing key:
   `" ".join([chunk.get("text") or "" for chunk in context_chunks])`.
   I picked "treat as empty" over two alternatives:
   - Skipping `None` chunks gives the same score but a different code
     shape for the same result.
   - Raising a `ValueError` contradicts `test_none_context_chunk_text`,
     which asserts a float.

   "Treat as empty" also makes `None` match the missing-key behavior
   that my `[{}]` control and `test_missing_text_key_in_chunk` already
   pin.
2. Delete the #60 xfail marker on `test_none_context_chunk_text` (its
   `strict=True` would otherwise turn the now-passing test into an
   `XPASS(strict)` failure).
3. Add `test_none_text_chunk_does_not_drop_other_chunks`: feedback
   `"Has Python skills"`, chunks `[{"text": None}, {"text": "Python
   skills shown in projects"}]`, expecting `1.0`. This pins that a
   `None` chunk is treated as empty rather than discarding the rest of
   the context.
4. Run `make check && make test-unit` before pushing.

## Test plan

Re-run my Unit 2 reproduction against the change, same environment:

1. The issue's snippet, printing the result. Before: the `TypeError` at
   line 38 above. After: no traceback; it logs
   `faithfulness_checked claims_count=1 score=0.0 supported_count=0`
   and prints `0.0` (the same as the missing-key control, since the
   context is empty).
2. `pytest tests/unit/test_faithfulness_checker.py -k
   test_none_context_chunk_text -q`, without `--runxfail`. Before: `1
   xfailed`. After, with the marker removed: `1 passed`.
3. The two controls are unchanged: `[{}]` still returns `0.0` and
   `[{'text': 'Knows Python well'}]` still returns `1.0`.
4. The new mixed-chunk test passes with a score of `1.0`.
5. The full file: `pytest tests/unit/test_faithfulness_checker.py -m unit -q`
   goes from `18 passed, 4 xfailed` to `20 passed, 3 xfailed` (the other
   three are #59's). `make check` and `make test-unit` are clean.

## Risks and unknowns

- `or ""` maps any falsy `text` value to `""`. For an empty string
  that is the same result as today. A non-string, truthy `text` (for
  example a number) would still fail in `join`. Nothing in the issue or
  my reproduction shows that input, so I'm not handling it here.
- After this fix, a caller going through `EvalSuite.run()` with a
  `None` chunk still crashes, in `RelevanceScorer.score()`, because
  `run()` scores relevance before faithfulness. #60 is fixed for
  `FaithfulnessChecker.check()` itself. The suite-level crash needs the
  separate fix noted under Scope.
- I haven't checked where `None` text originates, so I can't say how
  often real retrieval produces it.
- PR #74 annotated two test variables to clear mypy `var-annotated`
  errors. I checked on `main`: `make typecheck` runs mypy only on `api/
  core/ ingestion/ rag/ agent/ safety/` ("Success: no issues found in
  76 source files") and doesn't cover `tests/`, so I'm not making that
  change. Baseline for the test file under `make test-unit`'s `-m unit`
  filter: `18 passed, 4 xfailed`.

## Deviations

Built on `fix/60-none-context-chunk-text` in two commits: `3595a17`
(fix plus marker removal) and `684fabe` (mixed-chunk test).

One change from the plan. Under Risks I said I wouldn't add #74's
`list[dict]` annotations, because `make typecheck` (and CI's typecheck
job) doesn't run mypy on `tests/`. That part holds. What I missed is
that the repo's pre-commit hook runs mypy on every touched file, and
removing the xfail marker touches the test file. The hook then rejected
the commit over two existing bare `context_chunks = []` lines (73 and
82):

```
tests/unit/test_faithfulness_checker.py:73: error: Need type annotation for "context_chunks" (hint: "context_chunks: List[<type>] = ...")  [var-annotated]
tests/unit/test_faithfulness_checker.py:82: error: Need type annotation for "context_chunks" (hint: "context_chunks: List[<type>] = ...")  [var-annotated]
```

I annotated both as `context_chunks: list[dict] = []` inside the fix
commit, and the commit body says why. This doesn't change any behavior.
It was that or bypassing the repo's own hook with `--no-verify`. My
posted plan comment never mentioned the annotations, so it is still
accurate, and I didn't add a follow-up comment on the issue.

Everything else went as planned, and each test-plan prediction held:
the snippet prints `0.0` with no traceback, `test_none_context_chunk_text`
reports `1 passed` without `--runxfail`, both controls still return
`0.0` and `1.0`, and the file goes from `18 passed, 4 xfailed` to `20
passed, 3 xfailed`. The repo-wide checks are clean (`make lint`,
`black --check .`, `make typecheck`). `make test-unit` goes from `375
passed, 53 xfailed` on `main` to `377 passed, 52 xfailed` on the
branch.
