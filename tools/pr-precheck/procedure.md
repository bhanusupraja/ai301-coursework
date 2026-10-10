# Procedure: how this tool grades a PR package

This procedure grades a pull request someone else wrote, or the student's own draft before it goes out. It never writes or fixes the PR.

## Read order

1. Read `rubric.md` first. List the checks and the verdict rule.
2. Read the repo facts: the PR template's asks and the contribution policy, including any AI-use rule. Note each required section or item.
3. Read the issue and its thread. Note the one symptom, and any maintainer direction.
4. Read the plan before the PR: the plan context in a bundle, or `plan.md` including `## Deviations` in live mode. Note the in-scope and not-in-scope lines, the files named, the approach, and the test plan with each failure mode it names. Reading the plan first is what lets you tell what the diff should contain.
5. Read the diff. In live mode run `git diff main...HEAD` and `git log main..HEAD --oneline`. List every changed file and each hunk in one line.
6. Read the test evidence.
7. Read the title and description last, so that its claims are tested against a diff you already understand rather than shaping your view of it.

## Evidence gathering

Use `references/evidence-guide.md` for where each item lives. In eval mode quote only from the bundle.

1. Plan boundary: quote the plan's in-scope and not-in-scope lines and files list, and list the approach steps. Add each deviation note.
2. Diff map: for each changed file, note whether the plan names it. For each hunk, note whether it is part of an approach step. Mark every hunk or file with no step as extra, and every approach step with no hunk as missing.
3. Description claims: list each claim in the description (fidelity phrases, the Changes list, anything said to be done) and find the hunk that backs it, or note there is none.
4. Test evidence: quote the before and after output, and compare with the plan's test plan and repro steps. Note which failure modes it covers and whether the command exercises the path the diff changes.
5. Repo checks: list the checks the template or docs ask for, and note for each whether output is shown.
6. Debris: scan the diff for added debug prints, commented-out code, dead or unused functions and imports, formatting or re-indent only hunks, and unrelated edits. Note the commit messages that signal it ("wip", "misc cleanups").
7. Standards: for each template section and policy item, quote the description or diff line that meets it, or note it as missing. Quote the AI-use rule and the disclosure.

## Check execution

1. Run the checks in the rubric's order, each against its own recorded evidence. A check may be graded without re-reading the whole package once its evidence is recorded.
2. Grade each check `pass`, `fail` or `unclear` by the rubric's pass condition only. Do not add your own standard, and do not let polish, length or a confident tone move a grade.
3. Compare the diff to the plan, not the description to the plan: a description that says "exactly as planned" proves nothing about the diff.
4. A disclosed shortfall (a deferral named in the plan's deviations or the description) passes diff-matches-plan; the same shortfall undisclosed fails.
5. Use `unclear` only when the evidence the check needs is genuinely absent from the package (for example no test evidence section at all), after you have looked where the evidence guide says.
6. For each failed check, write one line naming the exact hunk, quote or missing item that decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every check passed. A `fail` or `unclear` on any check gives `reject`.
2. Show one summary line per check: name, grade, deciding fact.
3. For every failed check quote its deciding evidence in the JSON `evidence` field.
4. Live mode only: after the checks, hold the title and description against `voice-guide.md` and list any broken rule. This never changes the verdict. Also list any procedure gap you hit.
5. End with the JSON block from `SKILL.md`, last in the reply.
