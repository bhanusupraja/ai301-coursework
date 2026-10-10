# Procedure: how this skill grades a plan package

This procedure is for grading a plan someone else wrote, not for making one. Follow the steps in order. Do not skip ahead to the plan.

## Read order

1. Read `rubric.md` first. List the checks and the verdict rule so you know what you are looking for.
2. Read the package header and the "Repo facts" block (or, live, the repo's CONTRIBUTING and issue template). Note: the bug-report template asks, and whether the contribution policy requires AI-use disclosure or requires comments in the author's own words.
3. Read the issue (title, body, symptom, any version or platform). Note the one symptom the issue reports.
4. Read the "Thread highlights" (or, live, the issue thread). Note every direction from a maintainer or collaborator: an approach chosen or rejected, a culprit isolated, something they asked to be tested, an open PR.
5. Read the repro evidence before the plan. Note the steps, every control run, every artifact, and what each one rules in or out. Write down which component or step the evidence isolates and which it clears.
6. Read the candidate plan: its cause, scope, files, approach, test plan, risks.
7. Read the candidate plan comment last. Reading the evidence before the plan is what lets you catch a cause the evidence contradicts.

## Evidence gathering

Use `references/evidence-guide.md` for where each item lives. In eval mode use only the bundle text and quote from it; do not fetch anything. Record one short quote or fact for each item:

1. Diagnosis: quote the plan's stated cause. Quote the control run(s) and artifact(s) in the repro evidence that bear on it, and whether each agrees with or rules out that cause.
2. Scope: quote the plan's in-scope and not-in-scope lines and list each approach step. Mark any step the one reproduced symptom does not need.
3. Start: list the file(s) or area named and the single approach chosen. Quote any open decision ("somewhere", "whichever", "not sure", "investigate").
4. Test: quote the test plan and the expected result it names. Compare with the repro evidence's "Actual" line: is there an output, value or exit code that changes?
5. Honesty: quote each certainty word in the plan and comment ("root cause", "will fix", "guaranteed") and each stated risk or open question. Compare with what the repro evidence shows.
6. Thread: list the maintainer directions from the thread (step 4 of Read order) and quote what the comment says about each, or note that it says nothing.
7. Conventions: quote the policy line on AI use and the comment's disclosure, if any.

## Check execution

1. Run the checks in the rubric's order, one at a time, each against its own recorded evidence. You may grade a check without re-reading the whole package once its evidence is recorded.
2. Grade each check `pass`, `fail` or `unclear` using only the rubric's pass condition. Do not add your own standard, and do not let formatting, length or tone move a grade.
3. A grade of `unclear` means the evidence the check names is genuinely absent from the package (for example no test plan at all), not that you did not look. Look again in the places the evidence guide names before grading `unclear`.
4. If the rubric's pass condition says pass but the plan feels wrong, it still passes. Note the tension in the summary.
5. When a check fails, write one line naming the exact quote or fact that decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every check passed; any `fail` or `unclear` makes the verdict reject.
2. In the summary, give one line per check: name, grade, deciding quote or fact.
3. For each failing check, quote its deciding evidence in the output.
4. Live mode only: after the checks, hold the draft plan comment against `voice-guide.md` and list any rule it breaks by quoting the rule. This never changes the verdict.
5. End with the JSON block described in `SKILL.md`: every check with its grade and one-line evidence, then `verdict` as `accept` or `reject`. The JSON block must be last.
