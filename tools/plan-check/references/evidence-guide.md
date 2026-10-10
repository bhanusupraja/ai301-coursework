# Evidence guide: where evidence lives in a plan package

In an eval bundle the sections are: the header (source, captured date), "Repo facts", "Issue" (title, body, "Thread highlights"), "Repro evidence", "Candidate plan" and "Candidate plan comment". Use only these. In live mode the issue-side evidence is the live issue thread and the repo's docs (CONTRIBUTING.md, issue templates, AI policy), fetched with `gh`; the repro evidence is the student's posted repro comment on the issue; the candidate side is `plan.md` and `comment.md`.

## Diagnosis and grounding

- Where it lives: the plan states its cause in the "Cause" or "Diagnosis" part of the candidate plan. The facts that cause must explain are in the "Repro evidence" block: the numbered steps, the artifacts (output, logs, values), the control runs, and the "Actual" line. Live: the cause in `plan.md`, the facts in the posted repro comment.
- What good looks like: the cause names a component and a mechanism that every step and control in the repro evidence agrees with. Test it by asking, for each control run: does the blamed component behave badly there? If a control shows the blamed component working, or an artifact shows the bad value already present before the blamed step, the cause is ruled out. A cause lifted from the thread is grounded only if the artifacts back it.

## Scope

- Where it lives: the "In" and "Out" (or "In scope" and "Not in scope") lines of the candidate plan, plus the list of approach steps and the files named.
- What good looks like: one change that fixes the one reproduced symptom, with each approach step needed for it, and larger work named and deferred with a reason. A drive-by rewrite adds a migration, redesign, new option or setting, new framework, refactor or extra feature the symptom did not require, even if the core fix is inside it.

## Executability

- Where it lives: the "Files" line and the "Approach" or "Change" steps of the candidate plan; live, the same in `plan.md`.
- What good looks like: the plan names the file(s) or code area and commits to one approach, so a stranger could open the file and start. Open decisions at build time ("somewhere", "whichever is easier", "investigate first", "not sure which layer") mean it cannot be started. A named risk to check mid-build does not.

## Test plan

- Where it lives: the "Test" or "Test plan" part of the candidate plan, compared with the numbered steps and the "Expected" and "Actual" lines of the repro evidence.
- What good looks like: it re-runs the repro steps (or their inputs) and names what must change from the "Actual" result: a value, output, exit code or visible behaviour, and ideally the control that must stay unchanged. A test plan of "run the full suite", "nothing else should break" or "should feel fast" names no outcome for this fix.

## Honesty

- Where it lives: the confidence wording in the plan and in the plan comment ("root cause", "will fix", "guaranteed", "no risk"), and the "Risk", "Unknowns" or "Open question" statements; live, also the `## Deviations` section at the end of `plan.md`, where a mid-build change is recorded.
- What good looks like: claims stay inside the repro evidence, and real open questions (cost, other affected paths, an untestable variant) are named instead of hidden. False confidence asserts what the evidence never shows or promises results. A short plan with no risk section is fine if it claims nothing beyond what was shown.

## Comms

- Where it lives: the "Candidate plan comment", read against the issue's "Thread highlights" (live: the issue thread) and against the "Repo facts" lines on the bug-report template and the contribution policy, including any AI-use rule.
- What good looks like: thread-aware means the comment follows or answers each explicit maintainer direction (approach chosen or rejected, culprit isolated, patch to test, open PR) and does not race an existing PR. Boilerplate that would fit any issue is not thread-aware. If the policy says AI use must be disclosed, the comment states the tool and extent; a "write in your own words" rule only requires plain human voice; a silent or permissive policy requires nothing.
