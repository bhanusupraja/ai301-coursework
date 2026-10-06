# Unit 2 reproduction

GitHub username: bhanusupraja

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64

## Claim comment

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6023692094

Text as posted:

Hello, I'd to contribute to this issue, I will start running pytest/tests/unit/test_relevance_scorer/py -q to see whether I get the same assert 1.0 < 0.9 failure in test_query_with_partial_overlap, and I will check that the chunk really contains all four terms of Python Django web framework, I will post what i find here including ,my env and the output before i change anything



## Repro comment

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6024482947

Text as posted:

>Environment: Python 3.14.0, pytest 9.1.1, Windows 11 Enterprise (10.0.26200), repo commit f89c06f (full SHA f89c06fc3ff292df2a04a39ac51319d32a76b779) on my fork of codepath/pathreview-ai301-fa26-s1. Docker, Postgres, Redis and the frontend were not started; the unit tests do not need them. The repo docs list Python 3.11 as the minimum, and I used 3.14 without problems.

Steps:

1. `git clone https://github.com/bhanusupraja/pathreview-ai301-fa26-s1.git` and `cd pathreview-ai301-fa26-s1`
2. There is no requirements.txt; dependencies are in pyproject.toml and `make setup` runs `pip install -e ".[dev]"`. I ran only that part, inside the repo's `.venv`: `.venv/Scripts/python -m pip install -e ".[dev]"`
3. `.venv/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q`
4. `.venv/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail`

Output of step 3 (a plain run does not fail, because the test is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`):

```
..x................                                                      [100%]
18 passed, 1 xfailed in 0.87s
```

Output of step 4 (`--runxfail` ignores the marker and runs the test as a normal test):

```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests\unit\test_relevance_scorer.py:58: AssertionError
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
1 failed, 18 passed in 0.99s
```

Check of the fixture: `test_query_with_partial_overlap` uses the query "Python Django web framework" and the single chunk "Django is a Python web framework for rapid development". That chunk contains all four query terms (Python, Django, web, framework), so it is a full overlap, not a partial one. The log line agrees: `avg_score=1.0`.

Control: I called the scorer directly with the same query and three different chunks (a throwaway script, deleted afterwards; `git status --short` printed nothing afterwards):

```
'Django is a Python web framework for rapid development' -> 1.0
'Django is a Python framework' -> 0.75
'Rust is a systems language' -> 0.0
```

Dropping one query term ("web") from the chunk moves the score to 0.75, which is inside the range the test asserts (0.3 < score < 0.9), and a chunk with no overlap scores 0.0. So the scorer behaves sensibly and the fixture chunk is what is wrong.

Expected (from the issue): the test named "partial overlap" should use a chunk that only partly overlaps the query, so the score lands below 0.9.

Actual: the score is 1.0 and the assertion `1.0 < 0.9` fails when the xfail marker is ignored. This matches the issue because the failing assertion and the full four-term overlap are exactly what it describes. I have not tested any other Python version or OS.

Next: I will look at which replacement chunk gives a stable partial score (for example the 0.75 chunk above) and what removing the xfail marker involves, and report back here before I open a PR. I have not changed any code yet.


## Run history

All runs used `python run_eval.py --rubric ~/.claude/skills/repro-check/rubric.md --evidence ~/.claude/skills/repro-check/references/evidence-guide.md` from the clone's `eval/` directory (with `PYTHONUTF8=1` on Windows; plain `python3` is not installed here).

1. **Full run, first draft of the rubric: 16/20, below the bar.** Categories: clear-accept 4/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. All four misses were gold-accepts that my rubric rejected: pkg-03 (honest-outcome), pkg-05 (steps-rerunnable), pkg-09 (behavior-matches-issue) and pkg-12 (steps-rerunnable). The rubric was too strict, not too loose.
2. **Edit, then `--only pkg-03,pkg-05,pkg-09,pkg-12,pkg-20,pkg-06,pkg-18,pkg-19,pkg-04`: 8/9.** I loosened steps-rerunnable (the trigger may live in the issue if the report points to it exactly, or be described precisely enough to rebuild), behavior-matches-issue (an honest cannot-reproduce passes) and honest-outcome (only a load-bearing claim without an artifact fails). pkg-03, pkg-05 and pkg-12 flipped to agree; pkg-09 still failed honest-outcome. Canaries pkg-20 (the only disclosure package), pkg-06, pkg-18, pkg-19 and pkg-04 still agreed.
3. **Edit, then `--only pkg-09,pkg-03,pkg-05,pkg-12,pkg-20,pkg-04,pkg-13,pkg-14,pkg-15`: 8/9.** I added to honest-outcome that in a cannot-reproduce a repeat-run count matching a shown run, and a clearly hedged hypothesis, are not claims. pkg-09 now agreed, but pkg-03 flipped to reject on repo-conventions (the ripgrep policy says comments must be human-written, and the grader read that as a disclosure requirement).
4. **Edit, then `--only pkg-03,pkg-09,pkg-20,pkg-05,pkg-12`: 4/5.** I made repo-conventions say that a "human-written comments" rule is not a disclosure requirement. pkg-03 agreed again; pkg-09 failed behavior-matches-issue this time.
5. **Edit, then two `--only` runs of `pkg-09,pkg-03,pkg-02,pkg-08,pkg-16,pkg-17,pkg-20`: 7/7 both times.** I widened the cannot-reproduce branch of behavior-matches-issue to cover a good-faith attempt whose shortfall is stated openly. The wrong-target packages (pkg-02, pkg-08, pkg-16, pkg-17) and pkg-20 were the canaries for this loosening, and none flipped.
6. **Confirming full run with `--save-run eval-run.txt`: 20/20, bar passed.** Categories: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. No change between run 5 and run 6. The submitted `eval-run.txt` records rubric.md `257b1d769d4845f7`, evidence-guide.md `edd51b303ddf7bcc` and SKILL.md `f1b0abceef33d418`.

## Package analysis

**pkg-09** (sharkdp/fd#2033, `--exec-batch` ordering). Gold verdict: **accept**. My first-draft rubric said **reject**, failing behavior-matches-issue (the grader also listed control-run, which is only preferred and cannot change a verdict).

The package is an honest cannot-reproduce. The author ran scenario 2 from the issue (two `--exec-batch` commands, one hitting the argument-size limit first), showed the real log (`ONE` x3 then `TWO` x3), said it ran five times and tried a padded variant, said it did not test scenario 1, and explained what probably differed from the reporter's conditions. My first rubric required the artifact to show the issue's specific symptom, which a cannot-reproduce can never do, so the check rejected exactly the kind of report the course says to reward. The gold label accepts it because every claim in it is backed by what is shown and it never claims the bug.

Fixing it took three rounds, because the grader read the package differently on different runs. First it failed on honest-outcome (the five-run count and the padded variant have no separate artifact), then, once that was loosened, it failed on behavior-matches-issue (the attempt could not force the issue's exact condition). The final rubric covers both: honest-outcome treats a repeat-run count that matches a shown run and a clearly hedged hypothesis as not a claim, and behavior-matches-issue passes a good-faith attempt whose shortfall is stated openly. pkg-09 agreed in both of the last two `--only` runs and in the confirming full run.

## Check rationale

From `rubric.md`, the **behavior-matches-issue** check, pass condition:

> The artifact itself shows the issue's specific symptom, produced by the issue's trigger (same syntax, same operator, same input shape). An honest cannot-reproduce passes this check when it ran the issue's own trigger, or a good-faith attempt to produce it whose shortfall (what it could not force or match) is stated openly, never swapping in a different symptom, and shows the real outcome of that run; it cannot be expected to show a symptom it did not see, but it must not claim one. Fail if the artifact shows an adjacent symptom (a graceful validation error instead of a crash, garbled output instead of a crash, a compile error instead of the runtime error), if the input was altered so a different thing fails, or if the artifacts only show the tool runs and nothing shows the bug.

This is the check I changed most, and it reads the way it does because of pkg-09. The first sentence is the original rule: the artifact has to show the issue's own symptom from the issue's own trigger, because a package that only shows the tool runs, or shows a neighbouring failure, is not proof. The "honest cannot-reproduce" sentence was added so that a report that really tried the issue's trigger and honestly shows a different outcome still passes. I wrote three limits into it on purpose: the attempt must be the issue's trigger or a good-faith try at it, any shortfall must be stated, and it must never swap in a different symptom or claim a symptom it did not see. Those limits keep the adjacent-symptom failures (pkg-02's graceful error narrated as a crash, for example) rejected while letting pkg-09 through. The "Fail if" list is unchanged.

## Trade-offs

- **Looser on terse proof, stricter on invented proof.** Because steps-rerunnable now accepts a trigger that lives in the issue or is described precisely, a report that says "the issue's inputs" without pasting them can pass. A reader who has to go back to the issue to rebuild the input pays that cost, and the rubric accepts it. pkg-05 and pkg-12 needed this, but it could let a lazy report through.
- **A hedge can look like honesty.** The cannot-reproduce branch in behavior-matches-issue and the hedged-hypothesis rule in honest-outcome let a report through if it states its shortfall and hedges its guesses. A package could wrap a weak attempt in careful wording and still pass. I accepted that because rejecting honest cannot-reproduces is the worse failure for this course, but the rubric cannot tell a careful attempt from a careless one that only reads carefully.
- **The grader is not deterministic.** pkg-03 and pkg-09 each flipped between runs with the same files, so the 20/20 is one confirming run, not a guarantee. Some of my rubric wording is there to steady those two packages, and I did not check how it behaves on other kinds of packages.
- **Policy reading is narrow.** repo-conventions only fails on an actual disclosure requirement. A repo whose policy is vaguer than "disclose AI use" would pass, and pkg-20 is the only disclosure package I could test against.
- **Cost of iterating.** Targeted `--only` runs cost about $0.20 per package, so I tested canaries from the categories I might disturb, not every package, before spending on the full run.
