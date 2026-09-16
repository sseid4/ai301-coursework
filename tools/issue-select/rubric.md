# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

**Reference date.** Every "within N days" below is measured against the
bundle's stated capture date in eval mode, and against today's date in
live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `maintainer-alive` | Repo facts: the `last 5 default-branch commits` list, each entry's date and author. | Newest of the 5 commits falls within **90 days** of the reference date, and at least one of the 5 is human work. Human work means an author with no `[bot]` suffix, or a bot merge of a human-authored PR (source branch not `dependabot/` or `renovate/`). A log of only dependency bumps and their auto-merges fails: bots keep committing after the humans leave. | required |
| `repo-in-use` | Repo facts: `archived:` on the repo line, `latest release`, `last push to any branch`. | `archived: no`, and either `latest release` is within **365 days** of the reference date or `last push to any branch` is within **90 days**. A repo with `none published` for releases can still pass on the push clause. Archived fails outright, whatever the dates say. | required |
| `scope-bounded` | The issue body, its labels, and the full comment thread. | The issue asks for **one** coherent change that could land as a single PR. Several files, several named instances of the same small fix, several candidate causes of one symptom, or a checklist of acceptance criteria are all still **bounded**. It fails only on one of these: a body that is a tracking list of other issue numbers or that calls itself a meta, umbrella, or mega issue; the same change asked for across the whole codebase; a maintainer warning the fix is too deep for a newcomer (a maintainer's own diagnosis or suggested approach counts in favor, not against); a design question the thread leaves unsettled; a usage question rather than a change request; or a **new feature** from a non-member that no maintainer endorsed by comment or label. A maintainer-filed issue carrying a `good first issue` label is bounded unless its body is itself a tracking list, since the maintainer has already graded it for newcomers. A terse body or missing repro steps is never a fail: grade the size of the work asked for, not the polish of the writeup. | required |
| `not-claimed` | Repo facts: `this issue: assignees:` and `linked PRs:` with each state, plus the comment thread including PR numbers mentioned but not formally linked. | `assignees: none`, no linked or thread-mentioned PR in **open** state, and no claim comment ("I'll take this", "working on this") within **60 days** of the reference date or acknowledged by a maintainer. Linked PRs that are all closed do not block: those are abandoned attempts and the issue is free again. A merged PR blocks only when it already delivers what this issue asks for. Where the linked-PR list and the thread disagree, believe the thread. | required |
| `no-dead-attempts` | The issue's `opened on` date against the reference date, the `linked PRs:` states, and every claim comment in the thread. | Fails when the issue is older than **730 days** and carries either two or more closed-unmerged linked PRs or three or more claim comments that produced nothing. Age alone never fails: a quiet eight-year-old request with one stale claim is still open for business. It is the combination that matters, because a friendly label over years of abandoned attempts is telling me the work is harder than it reads. | required |
| `ai-policy-permits` | Repo facts: the `contribution policy` line and any policy file it names or quotes. | Passes on no stated policy, or on **conditions**: disclosure, human review, personally understanding and testing the change, or a ban limited to fully AI-generated or unreviewed work. Fails only on an unconditional ban with no human-review carve-out ("We do not accept AI-generated code or documentation"). My workflow is AI-assisted, so a real ban is a dead end however good the issue looks. | required |
| `maintainer-responsive` | Repo facts: the `maintainer first-response sample` list, days to first owner/member/collaborator comment per sampled issue. | At least one sampled issue drew a maintainer response within **30 days**; `unclear` when the sample is empty. Preferred on purpose: the sample is five issues wide and often maintainer-opened, so a healthy repo routinely shows four "no maintainer comment" lines. Too sparse to reject on, useful for ranking. | preferred |
| `newcomer-signposted` | The issue's labels, its opener's `author_association`, and the body. | Any one of: a `good first issue`, `help wanted`, or `documentation` label; an opener who is `OWNER`, `MEMBER`, or `COLLABORATOR`; or a body that names the file, function, or line to change. A maintainer-filed issue pointing at the code is the fastest kind to start. | preferred |

## Verdict rule

**Accept** only if all six `required` checks grade `pass`. A single
required `fail` rejects the issue: these six are independent ways a first
contribution dies, so they combine conjunctively, not as a score to
average.

`unclear` on a required check counts as **fail**. An issue whose liveness,
scope, history, claim state, or contribution policy I cannot verify from
the evidence in front of me is not worth a first PR.

`preferred` checks never change a verdict. They order accepted candidates:
`maintainer-responsive` first, since faster review beats a nicer issue,
then `newcomer-signposted`. The fit profile in `scope.md` breaks any
remaining tie, in live mode only.
