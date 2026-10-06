# Evidence guide: where proof lives in a reproduction package

In an eval bundle, the sections are: the header (source, captured date), "Repo facts", "Issue" (title, body, thread highlights), "Candidate claim comment", "Candidate repro report". Use only these. In live mode, the issue-side sections are the live issue thread and the repo's docs (CONTRIBUTING.md, issue templates, AI policy), fetched with `gh`; the candidate side is the student's draft files.

## Environment

- Where it lives: eval: the first lines of the "Candidate repro report" (an "Environment:" line or a sentence naming versions/OS), compared with the "Repo facts" latest release and with versions in the issue body. Live: the draft's environment line, compared with the issue's body and `gh release list`.
- What good looks like: the tool version, OS/platform, and any setting the failure depends on (driver, shell, backend, build profile) are named. Observable test: could a reader tell whether this attempt is comparable to the issue's? A report with output but no environment fails even if the output looks right.

## Steps

- Where it lives: eval: the numbered or inline commands and config in the "Candidate repro report". Live: the same in the draft.
- What good looks like: exact commands and config text appear, ordered from starting state to trigger, using only things a stranger can get (public repos, shown config). A reproduction that lives in a private repo or an unshared config cannot be re-run. Compare the trigger input with the one in the issue body: same syntax and shape.

## Behavior shown

- Where it lives: the code blocks (output, logs, exit codes) in the repro report, read against the symptom the issue quotes in its body.
- What good looks like: the artifact contains the issue's specific symptom, not just any failure. Check the error text, the exit code, whether the process crashed or exited gracefully, and whether the output is wrong or garbled. Adjacent symptoms to reject: a validation error where the issue shows a crash, a compile error where the issue shows a runtime error, garbled output with the program still alive, a banner that only proves the program runs. Also confirm the input was not changed from the issue's trigger. Ignore formatting and length.

## Honesty

- Where it lives: the sentences of prose in both comments that assert a result ("confirmed", "verified", "ran it ten times", "the cause is...", "guaranteed"), set next to the artifacts in the report.
- What good looks like: each assertion has an artifact beneath it. A cannot-reproduce that shows its attempt, artifacts and what differed (environment, inputs) is honest and passes. Red flags: confidence with no artifact, a diagnosis with no evidence, repeated-run counts nothing shows, a conclusion about a platform or version that was not tested, a stated "expected" that contradicts the artifact, or a version deviation hidden instead of stated. Also check the thread highlights: a version or platform difference the report ignores.

## Comms

- Where it lives: the claim comment against the issue title/body; both comments against the "Repo facts" lines on the bug-report template and contribution policy (including any AI-use rule).
- What good looks like: the claim names this issue's details and one concrete next step, and promises investigation or a report only, never a fix, guarantee or date. If the policy line says AI use must be disclosed, the comments state it (tool and extent); if it is silent or permissive, nothing is required. Boilerplate that fits any issue, or a bare +1, is a fail. Template asks (version, platform, steps, expected vs. actual) should be answered in the report.
