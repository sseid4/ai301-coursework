# Evidence guide: where evidence lives in a PR package

Each family below says where to look in an eval bundle, where to look
in live mode, and what good looks like there. Find every part by
content, not by heading: bundle and plan headings vary, and some plans
are plain prose.

## Plan fidelity (harness category: silent-drift)

**Where it lives.** In an eval bundle, the plan sits in `## Plan
context`: its stated cause, its scope line ("Scope, one bounded
change: ..."), its not-in-scope line, its `Files:` list, its test
plan, and any "known limit", deferral, or deviation note. What
actually changed sits in the candidate PR's `### Diff`: every
`+++ b/<path>` header and every hunk under it. The description's
claims about the change sit in `### Description`. In live mode, the
plan is the student's `plan.md` (its Scope, Files and areas, Approach,
and any deviation note added during the build); the diff is the output
of `git diff main...HEAD` in the working copy; the description is the
student's draft PR description.

**What good looks like.** Every changed file appears in the plan's
files or areas, every hunk does work the plan's approach commits to,
and every committed piece of the plan (a fix, a warning, a docs update,
a test) has a hunk. A deviation re-ties a mismatch only when it is
recorded in the plan itself, with what changed and why; a deviation
that exists only in the diff is silent drift. Drift runs both ways:
more than the plan (an unrequested option, a rename pass, a rewritten
neighbouring function, a dependency bump in place of the fix) or less
than the plan (a planned piece missing with no deferral note). The
description is read against the diff, never the other way round: a
description saying "exactly as planned" or "no other changes" over an
unplanned hunk, or claiming docs or tests no hunk contains, is silent
drift. An honest shortfall is the opposite: a deferral recorded in the
plan and restated in the description is ready as scoped.

## Test evidence (harness category: not-tested)

**Where it lives.** In an eval bundle, the PR's proof sits in `###
Test evidence` and sometimes in before/after blocks inside `###
Description`. What it must prove sits in `## Plan context`: the repro
evidence's trigger and Actual, and the test plan's expected-after for
each named repro or failure mode, plus any suite step. The repo's own
check commands sit in the repo-facts `pull requests:` and
`contribution policy:` lines (test suite, lint, fmt, pre-commit,
generate). In live mode, the proof is the student's captured terminal
output (or what the draft description pastes), the expectations are
`plan.md`'s Test plan, and the repo's checks are in the PR template,
`docs/CONTRIBUTING.md`, the `Makefile`, and `.github/workflows/`.

**What good looks like.** The plan's own trigger re-run on the branch,
with the before and the after both visible and the after matching the
test plan's expected result. Every repro or failure mode the test plan
names is shown, one by one; a control shown in place of the repro, or
a test of a neighbouring path (a GET where the issue is a POST, the
single-file case where the bug is two files), proves nothing about the
fix. The repo's named suite or check shows its outcome (a pass count,
"clean"). "Tests pass", "works now", or "verified on my machine" with
nothing named is not evidence.

## Diff quality (harness category: unreviewable)

**Where it lives.** In an eval bundle, `### Diff` (every `+` and `-`
line) and `### Commits`. In live mode, `git diff main...HEAD` and
`git log --oneline main..HEAD` in the working copy.

**What good looks like.** A reviewer can see the fix and nothing
else: every changed line serves the fix, its tests, or an entry the
repo asks for. The debris tells, any one of which makes the diff
unreviewable: a debug print, `eprintln`, `console.log`, or `print`
left in; a commented-out line or block (an old attempt or an
experiment); dead code (an unused helper, `allow(dead_code)`, an
`_unused` name); a TODO or FIXME added without purpose; whitespace or
re-indent churn; imports or statements reordered or re-split with no
behavior change; identical lines removed and re-added. Commit messages
such as "wip", "fmt + cleanup", or "misc cleanups while debugging" say
where to look; the diff decides.

## Standards and comms (harness category: standards-wall)

**Where it lives.** In an eval bundle, the repo's asks are in the
repo-facts block: the `pull requests:` line (template sections,
checklist items, issue-reference line, changelog or whatsnew entry,
title convention) and the `contribution policy:` line (CLA, AI clause,
AI_POLICY file). Maintainer direction is in `## Thread highlights`:
comments tagged OWNER, MEMBER, or COLLABORATOR, and constraints a
maintainer restated there. The PR honors them in `### Title`, `###
Description`, and (for entries like a changelog) `### Diff`. In live
mode, read `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`,
any AI policy, and the live issue thread via `gh issue view <n>
--comments`, against the draft title and description.

**What good looks like.** Every stated section and checklist item has
real content (under any heading, however brief) or a "not applicable"
with a reason; the issue reference the repo asks for is there; a
required changelog or whatsnew entry is a file in the diff. A visibly
ignored ask is a wall: no checklist where the template requires one,
no `closes #` where it is asked, a required entry missing with no
word. On AI policy, read the demand, not the topic, and treat the work
as AI-assisted: a disclosure rule is met only by a sentence stating
the assistance; a human-written rule is met by specific first-person
prose; silence is not a ban. On the thread, compliant means the PR
takes up a maintainer's direction or answers their question in the
description, and does not break a constraint they restated.
(Whether the description's claims match the diff belongs to plan
fidelity, above.)
