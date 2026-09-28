# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

Read all of the following before grading any check, in this order.
Keep a short note per step; the checks are graded from these notes.

1. **Settle the mode.** A package bundle (one markdown or JSON file
   containing issue context, repo facts, repro evidence, candidate
   plan, candidate plan comment) is eval mode: the bundle is the whole
   world, fetch nothing, and skip `scope.md` and `voice-guide.md`
   entirely. A student's own `plan.md` plus a draft comment and an
   issue URL is live mode: read `scope.md` first and stop if the issue
   is out of scope or the repo line is still a placeholder, then read
   `voice-guide.md` and keep its rules to hand for the summary.
2. **Repo facts.** Note the bug-report template's asks, the
   contribution policy, and any AI clause, quoted in the policy's own
   words. Quote it now: reconstructing it later from memory is how a
   disclosure rule turns into a general impression about AI.
3. **Issue.** Write the reported behavior in one line: trigger, then
   symptom. Every later check is judged against this line, not against
   the plan's description of the issue.
4. **Thread highlights.** List every comment from an OWNER, MEMBER, or
   COLLABORATOR that gives direction: a named culprit or file, a
   linked or open PR, a posted build to test, a stated constraint, a
   settled approach. If there is none, write "no maintainer
   direction"; that note is what lets `thread-direction` pass cleanly
   later instead of being argued from silence.
5. **Repro evidence, before the plan.** Note three things: the
   trigger, the observed Actual, and every control or comparison run
   with what it rules out (a run with the plan's suspected culprit
   removed that still fails, a case that works in the build the plan
   calls broken, a version delta). Reading this before the plan is the
   step that makes `cause-grounded` work: read the plan first and a
   confident, thread-endorsed diagnosis reads like a finding, and the
   control that kills it turns into a detail you skim.
6. **Candidate plan.** Note, by content and not by heading: the stated
   cause, the in-scope and not-in-scope lines, the files or areas
   named, the approach chosen, the test plan, and any stated unknown
   or deferral with its reason. Plan headings vary and some plans are
   plain prose.
7. **Candidate plan comment, last.** Note what it engages from the
   thread, what it claims about the work, and whether it addresses the
   policy from step 2.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

One gathering move per family, using `references/evidence-guide.md`
for where each one lives. Record the fact as a short quote or a
pointer to the step or line it came from, never as a judgment.

- **Diagnosis and grounding** (`cause-grounded`): pull the plan's
  cause sentence from step 6, and set it beside each control from
  step 5. For every control, record one line in the same form: what
  it removed, changed, or exercised, and what the result was. Then ask
  both questions of it, because a control excludes a cause two ways:
  did the failure persist with the plan's named culprit taken out, and
  did that culprit behave correctly where the control exercised it? A
  yes to either excludes the cause. The check is decided by this
  pairing, not by how the cause is worded.
- **Scope** (`scope-bounded`): list the changes the plan commits to,
  one line each, from its change list, approach steps, or prose. Mark
  each as needed-for-the-reported-behavior or not, against the issue
  line from step 3. Record deferrals in a separate list; they are not
  committed work.
- **Executability** (`stranger-executable`): record the site of the
  change (file, function, call site, or named area) and the approach
  chosen. Where the plan offers options without choosing, or names a
  verb instead of a change, record the phrase verbatim.
- **Test plan** (`test-observable`): record what the test says will be
  done and what it says will be observed, as two separate notes. If
  the observed column is empty, that is the finding.
- **Comms** (`thread-direction`, `policy-followed`): set the step 4
  direction list beside the plan's approach and the comment, and
  record whether each direction is taken up, engaged and declined, or
  unmentioned. Separately, set the step 2 policy quote beside the
  comment and record which of its demands the comment meets.
- **Honesty** (`unknowns-stated`): record what the repro evidence and
  thread leave open (an untested platform, a deferred part, a behavior
  change needing sign-off), then record whether the plan names each
  one.

If a family's evidence is genuinely absent from the package, record
"absent" with the section searched. Do not substitute a neighbouring
section, and do not fetch anything in eval mode.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Execute the checks in the rubric's table order: `cause-grounded`,
   `scope-bounded`, `stranger-executable`, `test-observable`,
   `thread-direction`, `policy-followed`, `unknowns-stated`.
2. Grade every check, including after a required check has failed. The
   output carries all of them, and the student needs the whole
   picture, not the first stop.
3. Grade each check from its gathered notes alone. Re-read only the
   named section when a note is missing or ambiguous; never re-read
   the whole package to settle one check, and never revise an earlier
   check because a later one came out badly.
4. Apply the rubric's pass condition to the outcome, not to the
   write-up. A terse plan that names its site and its observable is a
   pass; a long confident one missing them is not.
5. When evidence is absent, separate the two cases. The package is
   missing something the plan itself should contain (no test plan, no
   named site, no cause) is a **fail**: the plan does not have the
   thing the check grades. The plan makes a claim and the package
   cannot settle it either way (no control touches the stated cause)
   is **unclear**.
6. Never let agreement stand in for evidence: a cause endorsed by the
   thread, or a scope the comment calls bounded, is graded against the
   recorded facts, not against who agreed with it.
7. Record one line of evidence per check as it is graded: the quote or
   the step reference that decided it. A check without that line is
   not finished.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Collect the grades for the six required checks. If all six are
   `pass`, the verdict is `accept`. If any is `fail` or `unclear`, the
   verdict is `reject`.
2. `unclear` on a required check counts as a fail for the verdict and
   is still reported as `unclear` in the output, so the reason stays
   visible.
3. The preferred check (`unknowns-stated`) never moves the verdict in
   either direction. Report its grade and its evidence line as
   written.
4. Name the deciding check in the summary and quote its evidence line:
   on a reject, the first required check that failed in table order,
   with the fact that decided it; on an accept, the required check
   that came closest to failing. One line, the same line recorded
   during execution.
5. If the procedure had no step for something the package raised, say
   so in the summary as a procedure gap rather than improvising a
   step. In live mode, add any voice-guide rule the draft comment
   breaks, quoting the rule; it is reported, not scored.
6. Emit the summary first, then the fenced JSON block from SKILL.md
   last, with one entry per check in table order and nothing after it.
