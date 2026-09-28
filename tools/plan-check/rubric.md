# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `cause-grounded` | The plan's stated cause, read against the repro-evidence block's steps, timings, controls, and comparison runs (evidence guide: Diagnosis and grounding). | The stated cause explains the behavior the repro evidence actually shows, and no step or control in that evidence rules it out. A cause a control run excludes fails even when the issue thread endorses that same cause; thread agreement is not evidence. A control that exercises the plan's named component and shows it behaving correctly excludes that component just as decisively as one that removes it and still fails: the defect is in whatever differs between the passing and failing runs, not in the part they share. A cause the evidence neither supports nor excludes is unclear. | required |
| `scope-bounded` | The plan's in-scope and not-in-scope statements together with its list of changes or approach steps, read against the behavior the issue reports (evidence guide: Scope). | Every change the plan commits to is needed to fix the reported behavior. Work the issue did not ask for (refactors, migrations, redesigns, new options, framework ports, test-harness rewrites) fails the check even when a correct bounded fix sits inside it. Work the plan explicitly defers or rules out is not committed work and does not count against it, whatever its size. | required |
| `stranger-executable` | The files or areas the plan names, the approach it picks, and its order of work (evidence guide: Executability). | Someone who has not read the thread could start the first edit without asking the author to choose: the plan names where the change goes and which approach it takes. A plan that leaves the real decision for build time (which layer, which library, which of several options, "investigate and see") fails. Pinning an exact function or line by tracing inside an area the plan has already named, with the approach already chosen, is a locating step rather than a decision and passes. Terseness is not a fail: one named site and one named change is enough. | required |
| `test-observable` | The plan's test plan, read against the repro evidence's trigger and artifacts (evidence guide: Test plan). | The test names an observable outcome for this fix: the repro's own steps re-run with the specific result that must now differ, or a named fixture, case, or measurement with its expected result. Running an existing suite, "no regressions", or a felt quality ("should feel fast", "nothing else should feel broken") names no outcome for this fix and fails. No test plan at all fails. | required |
| `thread-direction` | The issue's thread highlights (maintainer, owner, or collaborator comments, isolated culprits, linked or open PRs, patched builds, requested testing), read against the plan's approach and the plan comment (evidence guide: Comms). | Where a maintainer has given explicit direction, isolated a culprit, or posted work to test, the plan takes it up, or the comment engages it and says why the plan goes another way. Taking a different route in silence fails. Where the thread carries no maintainer direction, the check passes. | required |
| `policy-followed` | The repo-facts contribution policy, including any AI clause and any stated bug-report or comment convention, read against the plan comment (evidence guide: Comms). | Match the policy's actual demand, not the topic of AI generally, and treat every package as AI-assisted work. Silence or permission passes. A policy that requires AI use to be **disclosed** passes only when the comment states the assistance; an undisclosed comment fails. A policy that requires only **human-written or human-reviewed** comments, with no disclosure clause, passes when the comment is specific, first-person prose about this plan rather than boilerplate; no disclosure sentence is needed and its absence is not a fail. An unconditional ban fails. | required |
| `unknowns-stated` | The plan's risks, open questions, and deferrals, read against what the repro evidence and the thread leave unsettled (evidence guide: Honesty). | The plan does not present as settled anything its own evidence leaves open: an untested platform or variant, a deferred part, or a behavior change that needs maintainer sign-off is named rather than glossed. A plan whose evidence leaves nothing open passes with no unknowns section. | preferred |

## Verdict rule

Accept only when every required check passes. A `fail` or an `unclear`
on any required check rejects the package: a plan that cannot be
verified from the package is not a plan that is ready to build from,
and one required failure holds the package however strong the rest of
it is. Preferred checks never change the verdict; they raise
confidence when they pass and are reported, not punished, when they
do not.
