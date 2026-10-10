# Unit 4 pull request

GitHub username: bhanusupraja

## Pull request

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/pull/121

## Branch

fix/64-partial-overlap-fixture

## Run history

1. **Full run, first draft: 17/20, below the bar.** Categories: clear-accept 4/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. All three misses were gold accepts my rubric rejected: pkg-05 (evidence-decisive and repo-checks-run), pkg-08 (repo-checks-run) and pkg-11 (description-matches-diff). The rubric was too strict, not too loose.
2. **Edit, then `--only pkg-05,pkg-08,pkg-11,pkg-04,pkg-07,pkg-10,pkg-14,pkg-17,pkg-20,pkg-12`: 10/10.** I loosened three checks (see Check rationale). pkg-05, pkg-08 and pkg-11 flipped to agree. Canaries from each category the loosening could touch (not-tested: pkg-04, 07, 10, 14; silent-drift: pkg-17; standards-wall: pkg-20; unreviewable: pkg-12) all still agreed.
3. **Confirming full run with `--save-run eval-run.txt`: 20/20, bar passed.** Categories: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. The submitted `eval-run.txt` records SKILL.md `73637ca4991a8e21`, rubric.md `37040dcf3e4b3dc5`, procedure.md `b3258eed643fa657` and evidence-guide.md `491e6dd27ab1c31a`.

## Package analysis

**pkg-11** (BurntSushi/ripgrep#3477, escape-aware trailing-space trim). Gold verdict: **accept**. My first-draft rubric said **reject**, failing description-matches-diff.

The package is terse but complete: one function fix, tests for the escape cases, a git-parity check, and an own-words AI disclosure that fits the repo's policy. My grader found two small wording problems in the description: it says tests cover "escaped, unescaped, and double-backslash" cases while the only test shows the escaped and double-backslash lines, and it says "one function touched" while a small helper rides along. My first rubric said every claim must be true of the diff, so any imprecision failed the check. That is the wrong standard for this package: nothing the description says was delivered is missing, it hides no change, and it never claims "exactly as planned" over a contradicting diff. I changed the check so only a load-bearing claim (a deliverable missing from the diff, a hidden change, a false fidelity claim) fails, and a loose count or a bit of wording does not. pkg-11 agreed in the next partial run and in the confirming full run, while pkg-17 (a description claiming docs that the diff never contains) still agreed as a reject.

## Check rationale

From `rubric.md`, the **description-matches-diff** check, pass condition:

> Every load-bearing claim the description makes is true of the diff: nothing it says was delivered (a fix, a docs update, a test, a flag) is missing from the diff, and it does not say "exactly as planned" or "nothing else changed" over a diff that contradicts that. Fail if the description claims a deliverable the diff does not contain, or hides a change the diff contains. A minor imprecision that does not change what was delivered (a loose count of test cases, "one function" when a small helper rides along, wording) does not fail.

It reads this way because of the silent-drift category: a description can claim more than the diff delivers (pkg-17, docs claimed but absent) or claim fidelity over a diff with extra work (pkg-03, pkg-06 and pkg-09). So the first part names the things that must be true: a delivered thing cannot be missing, and "exactly as planned" cannot sit over a contradicting diff. The last sentence exists because of pkg-11: without it the check punished harmless wording and rejected a gold accept. The check reads the description against the diff, never the description against itself or its polish.

## Trade-offs

- **Lenient on small wording slips.** A description with a loosely stated count or a small understatement passes. A reviewer may be mildly misled, and the grader has to decide what is "load-bearing", which it did consistently only on these packages.
- **Stated results count as evidence.** repo-checks-run now accepts "312 tests pass, fmt clean" written in prose with no pasted output. That let pkg-05 and pkg-08 through, but a PR could claim a check it never ran and pass if the claim does not contradict the evidence.
- **A secondary failure mode needs only a sentence.** evidence-decisive passes a plan's second failure mode that is reported in a sentence next to a shown primary before and after. That is weaker than a second before and after.
- **All checks are required.** One borderline check holds a PR, which keeps the verdict rule simple but makes a single check decide.
- **One confirming run.** The grader is not deterministic, and 20/20 is one run, not a guarantee. I checked the loosening only with ten packages, not all twenty, before the confirming run.
- **Cost.** About $5 for a full run, so I did one failed full run, one 10-package partial run, and one confirming full run.
