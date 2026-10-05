# Rubric: is this pull request ready to submit?

Every package is read as AI-assisted work. The four failure families
map onto the checks below: plan fidelity (`diff-within-plan`,
`description-matches-diff`), test evidence (`evidence-decisive`,
`repo-checks-run`), diff quality (`diff-clean`), and standards and
comms (`template-asks-met`, `ai-policy-followed`,
`maintainer-direction`). A terse PR can pass every one of them, and a
PR that honestly delivers less than everything can be ready: a
shortfall recorded in the plan's deviation or deferral note is scope,
not drift.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `diff-within-plan` | The diff's changed files and hunks, read against the plan's scope (files or areas named, approach chosen, not-in-scope line) and any deviation or deferral note in the plan (evidence guide: Plan fidelity). Read from the diff itself, never from the description. | Every hunk that changes behavior, text, configuration, dependencies, or structure is work the plan commits to or a deviation note records. Every piece of work the plan commits to is in the diff, or the plan records it as deferred or dropped. A hunk outside the plan with no note fails even when it is small, adjacent, or an improvement (an extra flag, a rename pass, a rewritten neighbouring function, a dependency bump standing in for the planned fix). Tests, a changelog entry, and fixtures the fix needs are inside the plan. | required |
| `description-matches-diff` | Every claim the description makes about what changed, including fidelity claims ("implements the plan exactly", "no changes beyond the plan", "the docs now document..."), read against the diff (evidence guide: Plan fidelity). | Each claim is true of the diff. A description that claims less than the diff delivers (says "no other changes" over an unplanned hunk) or more (claims docs, tests, or a fix that no hunk contains) fails. A description that names a deviation or deferral the diff reflects passes; one that is silent about the change's scope but claims nothing false passes. | required |
| `evidence-decisive` | The test-evidence section (and any before/after in the description), read against the plan's test plan and the repro trigger in the plan context (evidence guide: Test evidence). | The evidence re-runs the plan's repro trigger on the branch and shows the after result next to the before, and the after is the outcome the plan's test plan expected. When the test plan names more than one failure mode, repro, or input by name, every named one is shown re-run; a unit test that covers one mode does not stand in for an un-shown second. Evidence that exercises a different path than the repro (the control only, a different method or input, a neighbouring feature) fails. "Tests pass", "works on my machine", "verified for a day", or no evidence fails. | required |
| `repo-checks-run` | The test-evidence section's suite and check outcomes, read against the commands or asks in the repo-facts block (test suite, lint, format, pre-commit, generate) and the suite step in the plan's test plan (evidence guide: Test evidence). | Where the repo facts or the plan's test plan name a suite or check, the evidence shows that it ran on the branch and its outcome (a pass line, a count, "clean"). Where neither names one, the check passes on the repro evidence alone. Saying a suite "should pass", or admitting it was not run, fails. | required |
| `diff-clean` | The unified diff line by line, with the commit list as corroboration (evidence guide: Diff quality). | Every added or removed line serves the fix, its tests, or a stated repo ask. It fails on any of: a debug print or log left in, commented-out code (old attempts or experiments), dead code (an unused function, an `allow(dead_code)` helper, an unused variable), a TODO or FIXME added as debris, whitespace or re-indent churn, re-ordered or reformatted imports or statements the fix does not need, whole statements removed and re-added unchanged. One instance is enough. Unchanged lines that belong to a single statement the fix does change (the other lines of a multi-line call whose one argument changed, shown as `-`/`+` pairs because the diff grouped them) are how the diff renders that edit, not churn. Commit messages alone never fail the check, but "wip" or "cleanups while debugging" points at where to look. | required |
| `template-asks-met` | The repo-facts block's PR-template sections, checklist items, and contributing asks (issue-reference line, changelog or whatsnew entry, tests with fixes, title convention), read against the description and the diff (evidence guide: Standards and comms). | Every ask the repo states as required is met with real content in the description or a matching file in the diff, or is marked not applicable with a reason. Content counts under any heading; terseness is not a fail. It fails when a stated section, checklist, issue-reference line, or required entry is visibly absent with no word about it. Asks phrased as conditional ("screenshots if appropriate", "docs for API changes") pass when the condition does not apply. A repo with no template and no asks passes. | required |
| `ai-policy-followed` | The repo-facts contribution policy and any AI clause or AI_POLICY file it quotes, read against the description (evidence guide: Standards and comms). | Match what the policy demands, not the topic of AI. No stated policy, or a permissive one, passes. A policy requiring AI use to be **disclosed** passes only when the description states the assistance (the tool or the fact of AI help, and its extent where the policy asks). A policy requiring only **human-written or human-reviewed** text, with no disclosure clause, passes when the description is specific first-person prose about this change, and a disclosure sentence is not required. An unconditional ban on AI-assisted contributions fails. | required |
| `maintainer-direction` | The thread highlights (comments tagged OWNER, MEMBER, or COLLABORATOR, and policy a maintainer restated in the thread), read against the diff and the description (evidence guide: Standards and comms). | Where a maintainer gave explicit direction, asked a question about the approach, or stated a constraint, the diff follows it or the description engages it and says why the PR goes another way. Going another way in silence, or breaking a constraint a maintainer restated, fails. A thread with no maintainer direction passes. | required |
| `shortfall-restated` | The plan's deviation and deferral notes, read against the description (evidence guide: Plan fidelity). | Any limit, deferral, or deviation the plan records is restated in the description so a reviewer meets it without opening the plan. A plan with no such note passes. | preferred |
| `commits-legible` | The commit list. | Each commit message says what that commit changes in the code. "wip", "fix", "misc" fail. | preferred |

## Verdict rule

Accept only when all eight required checks grade `pass`. A `fail` or
an `unclear` on any required check rejects the package: the families
are independent ways a PR wastes a reviewer's time, so one required
failure holds the package however strong the rest is. `unclear` is
reported as `unclear` in the output and counts as `fail` for the
verdict, because a PR I cannot verify from the package is not ready to
submit. The two preferred checks never change the verdict in either
direction; they are reported so the author can tighten the PR.

<!--
Revision marks folded in from the Unit 4 calibration activity:
1. tighten diff-within-plan (calib-03): read the diff's hunks against
   the plan before the description; the description's "no changes
   beyond the plan" is never evidence of fidelity.
2. tighten evidence-decisive (calib-04): every failure mode the test
   plan names must be shown re-run; one mode plus a unit test is not
   enough.
3. restructure plan fidelity into two checks (calib-03, pkg-17 shape):
   diff against plan, and description against diff, so drift in the
   less-than-plan direction is caught too.
4. add explicit "terseness is not a fail" to template-asks-met and the
   honest-shortfall line to the preamble (calib-01: looks thin, is
   ready).
-->
