# Unit 1 selection

## Chosen issue

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64
"Relevance scorer 'partial overlap' test fixture actually has full query overlap"

**Skill verdict (live mode): accept**, ranked #1 of the accepted candidates.

```json
{"item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64",
 "checks": [
   {"name": "Repo active", "grade": "pass", "evidence": "Last push to main: Sep 16, 2026, 12 days before grading date (2026-09-28)"},
   {"name": "Bounded scope", "grade": "pass", "evidence": "Single wrong test fixture; issue body names the exact fix ('chunk should only partially overlap with the query terms')"},
   {"name": "Currently unclaimed", "grade": "pass", "evidence": "No assignees, no linked PRs, no comments"},
   {"name": "AI-contribution policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-related language; silence passes"},
   {"name": "Maintainer responsiveness", "grade": "unclear", "evidence": "Issue has zero comments, no response sample available"}
 ],
 "verdict": "accept"}
```

I ran the skill in live mode against three real open issues from the scoped
Path Review repo: #73, #69, and #64. #73 was rejected (an open linked PR,
#77, was already attached — an active claim, not merely a claim comment the
house rule would ignore). #69 and #64 both accepted. I picked #64 over #69
because it is the smaller, more self-contained of the two: a single wrong
test fixture with the exact fix already spelled out in the issue body,
versus #69's fallback-branching fix that touches more of the output
parser's control flow. That matches my fit profile in `scope.md`, which
says I want to build confidence with the contribution workflow itself
before taking on anything requiring deeper architectural context.

## Run history

1. Smoke run, `--limit 3` (issue-01, issue-02, issue-03): 3/3 agreement.
   This surfaced a Windows-only bug in the harness itself, not the rubric:
   `subprocess.run` was writing the prompt to the model's stdin using the
   console's default `cp1252` encoding, which crashed with
   `UnicodeEncodeError` on a non-ASCII character in one bundle. Fixed by
   setting `PYTHONUTF8=1 PYTHONIOENCODING=utf-8` before invoking Python;
   no rubric change was needed.
2. Full run, all 20 scored issues (no `--save-run`): 20/20 agreement,
   full category floor (claimed 4/4, clear-accept 8/8, dead-repo 3/3,
   policy 1/1, scope 4/4). Bar (18/20 + floor): PASS.
3. Full run again with `--save-run eval-run.txt`: identical result,
   20/20 agreement, full category floor, PASS. The harness wrote
   `eval-run.txt` since the run was complete and error-free. This is the
   file submitted as-is, never hand-edited.

No `--only` re-runs were needed: the first full run already cleared the
bar, so nothing had to be iterated on.

## Issue analysis

Scored issue **issue-15** (zulip/zulip#19589) is the one worth walking
through, because on the surface it looks like it belongs in the `claimed`
category but gold puts it in `scope`.

- **Rubric's verdict: reject**, via the "Bounded scope" check, condition
  (c): two PRs linked to the issue are closed without merging, and there
  is no maintainer comment after the most recent closure describing a
  settled new plan.
- **Gold's verdict: reject**, category `scope`, reasoning: "years of
  design debate and two abandoned PRs."

Both land on reject, but it matters *which* required check does the
rejecting, because the bundle's claim history (roughly seven different
people ran `@zulipbot claim` on this issue over the years, each auto-
unassigned after 14 days of inactivity) looks exactly like the shape of a
`claimed` rejection. If the rubric's "Currently unclaimed" check were
written to fail on *any* history of claim comments, it would reject this
issue for the wrong reason and would very likely get the *right* verdict
for the *wrong* check on other bundles too, which is the kind of rubric
that looks like it works until a slightly different bundle exposes it.
My "Currently unclaimed" check only fails on an assignee, an *open*
linked PR, or an *unresolved* claim comment (not stale, not
auto-unassigned, not superseded) — so a long history of expired claims
passes that check, and it's the "Bounded scope" check's closed-PR clause
that correctly catches this one instead, matching gold's actual
`scope`-category reasoning rather than misfiling it as `claimed`.

## Check rationale

The check that decided issue-64 (and issue-15 above): **Bounded scope**.
Current wording from the uploaded `rubric.md`:

> Fail if any of: (a) the issue body is a checklist/tracker listing 5+
> other issue numbers for others to split up, or calls itself a
> "megaissue"/tracking issue; (b) the issue body invites the solver to
> pick their own unbounded scope ("wherever it makes sense", "pick a
> file/module") instead of naming a concrete deliverable; (c) 2 or more
> PRs linked to this issue are closed without merging and no maintainer
> comment after the most recent closure gives a concrete, settled plan
> for a new attempt; (d) the issue proposes a new feature or
> visual/UI addition with no acceptance criteria or maintainer-confirmed
> approach, and leaves a design/asset/product choice explicitly
> unresolved (e.g. "TBD", an unanswered open question); (e) the issue is
> a pure usage/support question ("how do I...") with no requested code
> change. Otherwise pass.

For issue-64, none of (a)-(e) apply — the issue names one wrong test
fixture and states the exact fix — so it passes. I wrote the check as a
list of named failure shapes rather than a single vague "is this scoped
well?" prompt so that a grader (human or model) has to point at a
specific clause instead of a gut feeling, and so a scope failure like
issue-15's stays distinguishable from a claim failure like issue-13's or
issue-18's.

## Trade-offs

The "Currently unclaimed" check treats a stale/auto-unassigned claim as a
pass, which is right for zulip-style issues (issue-15) where the bot
guarantees a claim actually lapses, but it assumes evidence of staleness
is available. In live mode, a repo without an inactivity bot or a
maintainer comment marking a claim dead would leave that check genuinely
`unclear`, which the verdict rule treats as fail — the rubric would then
reject an issue that might actually be free, erring toward caution rather
than risk recommending an issue someone quietly still owns.
