# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

In an eval bundle, the plan's cause sits in the candidate plan under a
`Diagnosis` heading, a `Cause:` line, or the first sentences of a
summary; the plan's internal headings are not fixed, so find the claim
by content and never by heading. The behavior it must explain sits in
the `## Repro evidence` block: its numbered steps, its timings or
measurements, its stated Expected and Actual, and any control run
(the same action with one variable removed, a comparison build, a
version delta). In live mode, the cause is in the student's `plan.md`
and draft comment, and the behavior is in their posted repro comment
on the issue.

Good grounding means the cause names a mechanism that would produce
the Actual the evidence recorded, and survives every control in that
evidence. Read the controls before the plan and ask what each one
rules out. Controls come in two shapes and both are decisive. One
removes the plan's named culprit and the failure persists anyway (a
timing run with no pager in the loop at all). The other exercises the
plan's named culprit and shows it working correctly in the same build
the plan calls broken (a top-level call of the operator the plan says
is defective, returning the documented result). The second shape is
the easier one to read past, because the plan and the control are
talking about the same component and the plan sounds consistent: when
a control shows the named component behaving correctly, the defect
lives in what differs between the passing and failing runs, not in the
part they share. Both shapes stay decisive when the thread and the
plan agree with each other. A cause that the evidence neither reproduces nor excludes
is unverified, not correct.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

In an eval bundle, look at the plan's `Scope` section (its in-scope
and not-in-scope lines), at the numbered changes or approach steps it
commits to, and at the issue body for what was actually reported. In
live mode, the same parts of `plan.md`, read against the issue title
and body. Read the committed work, not the adjectives: a plan that
calls itself "one bounded change" and then lists a migration is not
bounded.

One bounded change is the set of edits the reported behavior needs,
and nothing else. Treat anything the issue did not ask for as out of
bounds however sensible it looks: an upgrade, a dependency swap, a
rewrite of the surrounding module, a new user-facing option, a port to
another pattern, a CI or test-harness overhaul. Deferral is the
opposite signal: work the plan names and explicitly puts out of scope
or postpones with a reason has been bounded correctly, even when the
deferred piece is large and even when a reader would have chosen
differently.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

In an eval bundle, read the plan's change list, approach steps, and
any file or module paths it names, wherever they appear (a `Changes`
section, an `Approach` list, or a prose `Change:` line). In live mode,
read the same parts of `plan.md` plus any files the draft comment
points at.

A stranger can start when the plan has already made the decisions: it
names the site of the change (a file, a function, a call site, or a
clearly identified area) and picks one approach among the ones
available. Terse is fine and short is fine; one named site with one
named change is executable. Separate locating from deciding: a plan
that has named the area and picked the approach, and says the exact
function or line will be pinned by tracing during the build, has
decided everything a stranger needs to start, and the tracing is the
first step of the work rather than a gap in the plan. What is not
executable is a plan that hands the decision forward: an unchosen layer ("gocui or tcell, not
sure"), an unchosen location ("upstream or vendored, whichever is
easier"), a verb standing in for a plan ("investigate", "profile and
optimize", "poke around"), or an area named only as the subsystem the
issue already named.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

In an eval bundle, read the plan's `Test plan` or test section against
the repro evidence's steps and artifacts: the trigger the repro used,
and the output, timing, state, or rendering it recorded. In live mode,
read the draft plan's test section against the student's posted repro
comment.

A decisive test names what will be observed and how it will differ:
the repro's own steps re-run with the specific changed result (the
color flips without leaving the view, the command exits 0, the seek
lands immediately), or a named regression case, fixture, or
measurement with its expected result. A vague test plan names an
activity instead of an outcome: running the existing suite, checking
for regressions, confirming nothing else broke, or a feeling about
speed or correctness. The gap is the outcome, not the length: one
sentence naming the observable flip is decisive, a paragraph about
testing thoroughly is not.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

In an eval bundle, read the plan's risks, open questions, caveats, and
deferral reasons, and compare them with what the repro evidence and
thread actually settle. In live mode, read the same parts of `plan.md`
and, after a build has started, its deviation note.

Honest uncertainty is specific and named: a platform or variant the
repro could not cover, a behavior change that needs maintainer
sign-off, a second symptom split out as separate, a question left
pending a benchmark. False confidence states a mechanism, a scope, or
an outcome the evidence has not established, in the register of a
finding. Silence is not automatically false confidence: a plan whose
evidence leaves nothing open needs no unknowns section. A mid-build
deviation belongs in the plan itself, recorded with what changed and
why; a deviation that exists only in the diff is not recorded at all.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

In an eval bundle, two sources meet here. The maintainer signals are
in `## Thread highlights`: comments tagged OWNER, MEMBER, or
COLLABORATOR, an isolated culprit or named file, a linked or open PR,
a posted patched build, a request to test something, or a stated
constraint on what the repo will accept. The conventions are in the
repo-facts block: the bug-report template's asks, the contribution
policy, and any AI clause quoted there. In live mode, read the live
issue thread and the repo's CONTRIBUTING, AI policy, and templates,
against the draft plan comment.

Thread-aware means the comment shows it read the thread: it takes up
the direction a maintainer gave, or it names that direction and says
why this plan goes elsewhere, or it engages the open PR instead of
racing it. Ignoring an owner who already isolated the culprit and
asked for testing is a wall, not an oversight, however polite the
comment is. On conventions, read what the policy demands rather than
its topic, and treat the work as AI-assisted: a rule that AI use be
disclosed is met only by a statement naming the assistance, while a
rule that comments be human-written or human-reviewed is met by
specific first-person prose about this plan and needs no disclosure
sentence. Silence in the policy is not a ban; an unconditional ban
blocks the package.
