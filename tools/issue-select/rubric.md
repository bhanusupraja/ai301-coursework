# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo active | Repo-facts block: the `archived:` flag and the `last push to any branch` date, compared against the bundle's `captured` date (live mode: compare against today's date). | Pass if `archived: no` AND the last push to any branch is within 90 days of the capture date. Fail if archived, or if the last push is more than 90 days old. |  required |
| Bounded scope | Issue body and comment thread; repo-facts `linked PRs` list for prior-attempt signals. | Fail if any of: (a) the issue body is a checklist/tracker listing 5+ other issue numbers for others to split up, or calls itself a "megaissue"/tracking issue; (b) the issue body invites the solver to pick their own unbounded scope ("wherever it makes sense", "pick a file/module") instead of naming a concrete deliverable; (c) 2 or more PRs linked to this issue are closed without merging and no maintainer comment after the most recent closure gives a concrete, settled plan for a new attempt; (d) the issue proposes a new feature or visual/UI addition with no acceptance criteria or maintainer-confirmed approach, and leaves a design/asset/product choice explicitly unresolved (e.g. "TBD", an unanswered open question); (e) the issue is a pure usage/support question ("how do I...") with no requested code change. Otherwise pass. | required |
| Currently unclaimed | Repo-facts `this issue: assignees` and `linked PRs` lines; comment thread claim language. | Fail if (a) assignees is non-empty, or (b) at least one PR linked to this issue is in the "open" state, or (c) the most recent claim-relevant activity is a claim comment ("I'll take this" / "working on this" / "can I work on this" answered yes) that is not itself marked stale or abandoned (by a stale-bot notice, an auto-unassignment, or a long silence that produced no PR). A claim that is stale, auto-unassigned, or superseded passes this check. Otherwise pass. | required |
| AI-contribution policy allows this workflow | Repo-facts `contribution policy` line. | Fail only if the policy states an outright ban on AI-assisted or AI-generated contributions (e.g. "we do not accept AI-generated code"). Conditions such as disclosure, human review, testing, or personal understanding requirements pass. No stated policy passes. | required |
| Maintainer responsiveness | Repo-facts `maintainer first-response sample`. | Pass if at least one sampled issue got an owner/member/collaborator first response within 45 days of that issue being opened. Used only to rank accepted issues against each other; never changes the verdict. | preferred |

## Verdict rule

Accept if and only if every `required` check passes. A `required` check graded `unclear` counts as a fail (a first issue you cannot verify is not one you should take). `preferred` checks never change the verdict; use them only to order the accepted issues, most promising first, and say in the summary what made the top-ranked one fit.


