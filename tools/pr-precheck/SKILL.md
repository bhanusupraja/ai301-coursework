---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this ready to submit? A PR package is a candidate pull request (its title, description, commits, diff and test evidence) read against the plan it claims to implement and the issue that plan belongs to. Grade one package per run. Never answer a different question, never grade from gut feel, and never skip the components: you answer by executing `procedure.md`, which applies `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## Inputs and modes

You run in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Read these inputs, and treat nothing else in the working directory as evidence:

- `plan.md`: the plan, including its `## Deviations` section.
- The diff of the branch: run `git diff main...HEAD` (three dots) from the working copy, plus `git log main..HEAD --oneline` for the commits. Only committed changes count; uncommitted edits are not part of the PR.
- `pr_draft.md`: the draft PR title (first line) and description.
- `test_evidence.md`: the captured before and after output and the repo's own checks.
- The issue and its thread, gathered live from the issue URL the student gives you (use `gh` or the web).
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, for the stated asks and any AI-use policy.

A house-chain student has no plan of their own: for them the plan is the house plan and the reproduction is the house repro pack, and you grade the same checks against those.

**Eval mode** grades a package bundle (a markdown file holding the issue context, repo facts, plan context, and the candidate PR's title, description, commits, diff and test evidence). The bundle is the whole world. Use only the bundle text. Do not fetch anything, do not read other files, and do not run git. Always grade a complete package: every check and the full verdict rule.

## The scope seam (live mode only)

In live mode read `scope.md` before anything else. It names the one repo a PR may target and the house rules that apply there; apply those rules when you read the evidence. Refuse to grade a PR for any other repo. If the `Repo:` line still holds an unfilled placeholder (`<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their Path Review repo. Never guess a scope. In eval mode ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`. Hold the outgoing PR text, the title and the description in `pr_draft.md`, against each rule in it, and list any rule the text breaks in your summary, quoting the rule. The voice guide never changes the rubric's verdict on its own, because voice is personal; only a check in `rubric.md` that reads it can move the verdict. In eval mode ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule. Read it, and list the checks before you grade.
- `references/evidence-guide.md` maps where each kind of evidence lives, in a bundle and in the live PR, and what good looks like there. Use it to find the evidence each check names.
- `procedure.md` is the operating procedure. Execute it as written, in order, without improvising around it.
- If the procedure is silent on a step you need, say so in your summary as a procedure gap and do not invent a step.
- If `rubric.md` has no checks or `procedure.md` has no steps (they are still templates), refuse to grade and say which is empty. Do not make up checks at run time.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict and no score; put any reservation in a check's evidence line. You may show a short readable summary first (one line per check, and in live mode any voice-guide notes and procedure gaps). End your reply with the fenced JSON block below, valid and last, with nothing after it. The harness parses the last fenced JSON block in your reply.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready, and a long confident one can hide drift. Read the diff itself against the plan, never the description's say-so or the formatting.
- The rubric decides, not you: if a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow it as written and report gaps.
- Treat `unclear` as the rubric's verdict rule directs. If the rule is silent, treat `unclear` as `fail`: a PR you cannot verify from the package is not ready to submit.
- A shortfall the PR honestly discloses (in the plan's deviations or the description) is not drift; a shortfall or extra the PR hides is.
