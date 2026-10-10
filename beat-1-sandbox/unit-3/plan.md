# Plan for issue #64: "partial overlap" fixture actually has full overlap

## Diagnosis

The scorer is correct and the test fixture is wrong. `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py` uses the query "Python Django web framework" against the single chunk "Django is a Python web framework for rapid development". That chunk contains all four query terms, so it is a full overlap, and the scorer correctly returns 1.0.

Repro evidence I rely on (from my posted repro comment):

- With `--runxfail`: `E       assert 1.0 < 0.9` and the log line `relevance_scored avg_score=1.0 chunks_count=1 query_len=4`.
- Control, same query, three different chunks called directly on the scorer:
  - `'Django is a Python web framework for rapid development' -> 1.0`
  - `'Django is a Python framework' -> 0.75`
  - `'Rust is a systems language' -> 0.0`

Dropping one term ("web") moves the score to 0.75, inside the asserted range `0.3 < score < 0.9`, and no overlap scores 0.0. So the scorer gives sensible scores and only the fixture chunk is wrong. This also means the scorer code is not the cause.

## Scope

In scope:
- Change the chunk text in `test_query_with_partial_overlap` so it contains only some of the query terms.
- Remove the `@pytest.mark.xfail(strict=True, ...)` marker from that test, because `strict=True` would turn a passing test into a failure.

Not in scope:
- Any change to `rag/evaluator/relevance_scorer.py` or its scoring logic.
- Other tests in the file, other test files, and other xfail markers.
- Changing the asserted range `0.3 < score < 0.9`.

## Files I will touch

- `tests/unit/test_relevance_scorer.py` (only `test_query_with_partial_overlap`)

## Approach

1. In `test_query_with_partial_overlap`, replace the chunk text with "Django is a Python framework" (drops "web"; the repro control showed this scores 0.75).
2. Delete the `@pytest.mark.xfail(...)` decorator (lines 43-46) on that test.
3. Leave the query, the assertions and the docstring as they are.
4. Run the file and then the whole unit test file set to check nothing else moved.

## Test plan

Re-run my unit 2 repro steps after the fix:

1. `.venv/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q`
   - Before the fix: `18 passed, 1 xfailed`.
   - Expected after: `19 passed` with no xfailed.
2. `.venv/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail`
   - Before the fix: `1 failed, 18 passed` with `assert 1.0 < 0.9`.
   - Expected after: `19 passed`, and no `assert 1.0 < 0.9` failure (the score for the new chunk is 0.75).
3. Re-run the control script from the repro: the 0.75 and 0.0 results stay the same, so the scorer is unchanged.

## Risks and unknowns

- I have only run this on Python 3.14.0 on Windows 11; I have not tested other Python versions or operating systems.
- The 0.75 score came from one direct call in my control run; I will confirm it again in the new test run rather than assume it is stable across scorer changes.
- I don't know whether the maintainers prefer a different replacement chunk; the issue does not say. I picked the one my control run already measured.

## Deviations

Nothing changed from the plan. The build was exactly the two edits I planned in `test_query_with_partial_overlap` (chunk text changed to "Django is a Python framework", xfail marker removed), in the one file I named, and I did not touch the scorer. After the change, `pytest tests/unit/test_relevance_scorer.py -q` gave `19 passed` and `--runxfail` also gave `19 passed`, as I expected.
