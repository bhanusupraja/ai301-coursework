# Evidence guide: where evidence lives in a PR package

In an eval bundle the sections are: the header (source, captured date), "Repo facts", "Issue" (title, body, thread highlights), "Plan context" (the accepted plan and its repro evidence), and "Candidate PR" with its "Title", "Description", "Commits", "Diff" and "Test evidence". Use only these. In live mode the plan is `plan.md` (with `## Deviations`), the diff is `git diff main...HEAD`, the commits are `git log main..HEAD --oneline`, the title and description are `pr_draft.md`, the test output is `test_evidence.md`, and the repo's asks are in `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.

## Plan fidelity (harness category: silent-drift)

- Where it lives: the plan's scope, files and approach in "Plan context" (live: `plan.md`, its in-scope and not-in-scope lines, and `## Deviations`); the changed files and hunks in the "Diff" (live: `git diff main...HEAD`); the claims in the description (live: `pr_draft.md`).
- What good looks like: every changed file and hunk maps to a plan step or a deviation note, and every plan step has a hunk or a disclosed reason. Silent drift runs both ways: more than the plan (an extra flag, option, rename pass, rewrite, or a file the plan never names) or less (a promised docs update or second fix that is not in the diff) with no note. A description that claims "exactly as planned" or "docs now updated" is drift when the diff contradicts it. Check the diff itself, not the claim.

## Test evidence (harness category: not-tested)

- Where it lives: the "Test evidence" section of the candidate PR (live: `test_evidence.md` and the Testing section of `pr_draft.md`), read against the plan's test plan and the repro steps in "Plan context" (live: `plan.md` and the unit 2 repro).
- What good looks like: the plan's repro is re-run on the changed path with the output before and after shown, and each failure mode the plan's test plan names is covered. The repo's own checks (tests, lint, typecheck) are listed with their results, and a failing or no-op check is reported with the reason. "Tested locally", "tests pass", or a control case that never hits the changed path is not decisive.

## Diff quality (harness category: unreviewable)

- Where it lives: the unified "Diff" and the "Commits" list (live: `git diff main...HEAD`, `git diff main...HEAD --stat`, and `git log main..HEAD --oneline`).
- What good looks like: the fix is visible by itself. Debris tells to look for in added lines: debug prints, leftover logging, commented-out blocks, dead or unused functions and imports, TODOs, formatting or re-indent only hunks, and edits to unrelated files. Commit messages like "wip", "misc cleanups" or "fmt" are a hint to look closer. In live mode the stat should list only the files the fix changes.

## Standards and comms (harness category: standards-wall)

- Where it lives: the "Repo facts" lines on the PR template and contribution policy, including any AI-use rule (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`), read against the "Description" (live: `pr_draft.md`) and the diff.
- What good looks like: every template section has real content (summary, issue reference, changes, testing with only true boxes ticked, notes), any stated checklist or required file change (a changelog or whatsnew entry in the diff, an i18n step) is met, and if the policy requires AI disclosure the description states the tool and the extent. Compliance means the ask is answered, not that a heading is present. Whether the description's claims match the diff is plan fidelity, not this family.
