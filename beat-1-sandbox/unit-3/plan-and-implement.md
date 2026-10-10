# Unit 3 plan and implement

GitHub username: bhanusupraja

## Plan comment

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6101108922

Text as posted (paste exactly what you posted; this is the text of comment.md):

Plan for #64, building on my repro comment above (`--runxfail` gives `assert 1.0 < 0.9`, `avg_score=1.0`).

**Cause:** the scorer is fine; the fixture is wrong. The chunk "Django is a Python web framework for rapid development" contains all four query terms, so 1.0 is correct. In my control, the same query against "Django is a Python framework" scored 0.75 and a chunk with no overlap scored 0.0.

**Change:** in `tests/unit/test_relevance_scorer.py`, `test_query_with_partial_overlap` only:
1. Replace the chunk with "Django is a Python framework" (drops "web").
2. Remove the `@pytest.mark.xfail(strict=True, ...)` marker, since a strict xfail would fail once the test passes.

**Not touching:** `rag/evaluator/relevance_scorer.py`, the asserted range `0.3 < score < 0.9`, or any other test or xfail marker.

**Test plan:** re-run my repro steps. `pytest tests/unit/test_relevance_scorer.py -q` goes from `18 passed, 1 xfailed` to `19 passed`; with `--runxfail` it goes from `1 failed, 18 passed` to `19 passed`.

**Unknowns:** I tested only Python 3.14.0 on Windows 11. If you would prefer a different replacement chunk, tell me and I'll swap it.

Disclosure: I used Claude Code to help draft this comment and the plan; I ran the commands myself and reviewed the text.

## Branch

fix/64-partial-overlap-fixture

## Evidence

Run from the fork's clone with `.venv\Scripts\python.exe`.

Before the fix (branch created from main, no edits):

```
> .venv\Scripts\python.exe -m pytest tests/unit/test_relevance_scorer.py -q
..x................                                                      [100%]
18 passed, 1 xfailed in 3.38s

> .venv\Scripts\python.exe -m pytest tests/unit/test_relevance_scorer.py -q --runxfail
E       assert 1.0 < 0.9

tests\unit\test_relevance_scorer.py:58: AssertionError
---------------------------- Captured stdout call -----------------------------
2026-10-07 00:17:52 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
=========================== short test summary info ===========================
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap
1 failed, 18 passed in 1.87s
```

The change (`git diff`):

```
-    @pytest.mark.xfail(
-        strict=True,
-        reason="issue #64: relevance scorer 'partial overlap' fixture actually has full overlap",
-    )
     def test_query_with_partial_overlap(self, scorer):
...
-            {"text": "Django is a Python web framework for rapid development"},
+            {"text": "Django is a Python framework"},
```

After the fix:

```
> .venv\Scripts\python.exe -m pytest tests/unit/test_relevance_scorer.py -q
...................                                                      [100%]
19 passed in 2.11s

> .venv\Scripts\python.exe -m pytest tests/unit/test_relevance_scorer.py -q --runxfail
...................                                                      [100%]
19 passed in 1.90s
```

`git status --short` showed only `M tests/unit/test_relevance_scorer.py` as a change to tracked files; `plan.md` and `comment.md` stayed untracked and out of the commit.

## Run history

1. **Full run, first and only draft of the rubric: 19/20, bar passed.** Categories: clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. The one miss was pkg-14 (gold accept, my rubric rejected it on honest-uncertainty). The submitted `eval-run.txt` is this run; it records rubric.md `cf3ff39d6d4b65e8` and evidence-guide.md `0982acbf2a722f69`. I did not run any `--only` re-grades or a second full run.

## Package analysis

**pkg-14** (zellij-org/zellij#5174, reattach handshake). Gold verdict: **accept**. My rubric said **reject**, failing honest-uncertainty.

The grader's reason: the plan says its cause is "grounded in the repro" with a 0.44.1 control that "predates the reattach-path change in 0.44.2" and says a fresh attach "performs the same queries behind the loading screen". The grader judged that neither statement is shown in the repro evidence, and that the comment's "0.44.2 on leaking" goes beyond the repro, which ran on 0.44.3 only. The gold label accepts the package because the plan is a bounded, scoped-down fix that defers the untestable Windows variant and says so. On the other six checks my rubric agreed with the gold label (diagnosis-grounded, scope-bounded, stranger-can-start, test-observable, thread-aware and repo-conventions all passed). So the disagreement is only about how strictly honest-uncertainty reads claims that sit near the evidence but are not shown in it. I think the grader's reading is defensible against the rubric text, which is why I did not loosen it: loosening it could let through packages that really do assert things the repro does not show.

## Check rationale

From `rubric.md`, the **honest-uncertainty** check, pass condition:

> The plan and the comment claim only what the repro evidence supports; where the fix leaves a real risk or open question (a performance cost, another affected path, an untestable variant) it is said, not hidden. Fail if the plan or comment asserts as fact something the repro evidence does not show, guarantees results ("this will fix all", "guaranteed", "no risk") or hides an open question the thread or repro evidence raises. A short plan with no stated risk passes when nothing it claims goes beyond the evidence.

It reads this way because the failure family is unknowns dressed up as certainty. The first sentence sets the standard (claims stay inside the evidence). The second names what a good plan does with real risk. The "Fail if" list gives three observable triggers (an unshown claim stated as fact, a guarantee, a hidden open question) so two graders can reach the same answer. The last sentence is there so that terse, complete plans like pkg-02 and calib-01, which state no risk, are not failed just for being short.

## Trade-offs

- **Strict on claims near the evidence.** The check fails a plan that states a plausible fact the repro does not show, even when the fix is sound. That is exactly what happened to pkg-14, a gold accept. I accepted one miss rather than loosen the check, because a looser one would stop catching confident plans that go beyond their evidence.
- **It reads the plan and comment, not the code.** The check cannot tell a true unshown statement from a false one; it only sees that the package does not show it.
- **Only one grading run.** The 19/20 comes from one full run and the grader is not deterministic, so pkg-14 or another package could flip on a re-run with the same files. I did not spend on a second run to measure that.
- **All checks required, no preferred ones.** One failed check holds a plan, which keeps the rule simple but makes a single borderline check decide a package.
