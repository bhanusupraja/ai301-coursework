# Contribution: bug(model-sources): multi-agent capability does not preserve client collaboration tools

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Supraja
**GitHub Username:** bhanusupraja
**Issue:** https://github.com/Soju06/codex-lb/issues/2499
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `1` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout with a profile-traced search strategy and rubric, then patched both after a wild scan failed its own top 5**

I built issue-scout with three Python queries traced to my fit profile, ranking by fewest comments then most recent update, and grading the top 5. The scan returned 283 unique issues and 212 survived ranking, but all top 5 came from one brand-new university team repo, with role-numbered tasks, and the frontend filter missed a role:frontend label, so I would not take any of them. I changed the strategy to at most one result per repo and substring label matching for frontend, UI, and javascript, added rubric checks for repo established (30 days) and not reserved for the author's own team, and recorded the change in scan-notes.md on 2026-10-30.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched the AI301 Issue Selection site with Language Python and tags help wanted, good first issue, enhancement, and documentation, then broadened the search beyond good first issue alone to help wanted and enhancement work, because the first scan's top results did not survive a real-world check. My original scout scan surfaced a top 5 that all came from one university team project and that I would not take, and a later broad search, verified issue by issue on GitHub, surfaced codex-lb #2499: a Python backend bug in a model gateway used by coding agents, where multi-agent capability metadata does not preserve client collaboration tools. Manual rubric grading on 2026-10-30, after the scout grade command could not run in Claude Code because the course credit balance was too low, gave this issue an ACCEPT verdict with a difficulty note of moderate: it is open, unassigned, with no live pull requests, it passes the AI-policy check with no outright ban found, and the maintainer confirmed the bug on 28 September 2026 and welcomed a focused pull request, with the remaining work limited to the namespace half after #2525 already merged base_instructions.

This issue fits my profile as an AI engineer working in Python, FastAPI, and agentic workflows, with RAG, document processing, and multi-agent systems, because it is backend logic and API tool filtering in an agentic gateway, in the catalog and proxy modules, not complex frontend UI and not a non-Python ecosystem. I hope to learn how multi-agent capability metadata is projected through a gateway, how collaboration namespaces are preserved or lost, and how an OpenSpec change is used to document a contract change. My scout fell short on a real wild search in several ways I have now patched: one repo filled the whole top 5, a substring label was missed, a brand-new repo passed as active, and an umbrella onboarding issue looked claimable when its real work was in contested or assigned child tasks. What convinced me the patched approach held up was verifying each candidate on GitHub itself — assignee, live pull requests, claim comments, maintainer replies, and AI policy — before claiming, rather than trusting the list page alone.

Claim comment link: https://github.com/Soju06/codex-lb/issues/2499#issuecomment-6528381254

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: The original scan (283 unique, 212 survived ranking, top 5 all from one university repo, none taken), followed by a broad Issue Selection search and GitHub verification that produced codex-lb #2499, manually graded ACCEPT, moderate, on 2026-10-30.
- Difficulties: The scout's top 5 failed on real-world checks (one repo, role-numbered team tasks, missed frontend substring, brand-new repo passing as active), an umbrella issue looked claimable, and the scout grade command could not run on 2026-10-30 because the Claude Code course credit balance was too low, so grading was done manually from verified checks.
- What changed: Strategy changed to at most one result per repo and substring label matching; rubric gained Repo established (30 days) and Not reserved for the author's own team; a scope-feasible patch is to be applied in Unit 6, and a true scout grade will supersede the manual grade only if course credit returns.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
