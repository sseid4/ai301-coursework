# Procedure: how this tool grades a PR package

## Read order

Read all of the following before grading any check, in this order.
Keep a short note per step; the checks are graded from these notes.

1. **Settle the mode.** A package bundle (one markdown or JSON file
   with repo facts, issue, thread highlights, plan context, and a
   candidate PR) is eval mode: the bundle is the whole world, fetch
   nothing, and skip `scope.md` and `voice-guide.md` entirely. A
   student's own `plan.md`, branch, draft PR title and description,
   and test output is live mode: read `scope.md` first and stop if the
   repo is out of scope or the repo line is still a placeholder, then
   read `voice-guide.md` and keep its rules for the summary. In live
   mode, produce the diff with `git diff main...HEAD` and the commit
   list with `git log --oneline main..HEAD`, run from the working copy.
2. **Repo facts.** Quote the PR-template sections and checklist items,
   every contributing ask (issue reference, changelog or whatsnew
   entry, tests, title convention, suite or check commands), and the
   contribution policy with any AI clause, in the repo's own words.
   Write them as a numbered ask list. Quoting now is what stops a
   disclosure rule turning into a general impression later.
3. **Issue and thread.** Write the reported behavior in one line
   (trigger, then symptom). List every OWNER, MEMBER, or COLLABORATOR
   comment that gives direction, asks a question about the approach,
   or restates a constraint. If there is none, write "no maintainer
   direction".
4. **Plan, before the PR.** From the plan context (live: `plan.md`),
   write down by content: the files or areas named, the approach,
   each piece of committed work as its own line, the not-in-scope
   line, each deviation or deferral note, and the test plan as a list
   of named repros or failure modes, each with its expected-after,
   plus any suite step. The plan has to be fixed in the notes before
   the diff is opened: the diff is graded against these lines, not
   against what the PR says it did.
5. **Diff and commits.** List every changed file. For each hunk, write
   one line saying what it does (fix, test, changelog, rename, new
   option, reformat, debug line, commented-out code, dead helper). Do
   not read the description yet.
6. **Test evidence.** For each command shown, write what it ran, the
   before if shown, and the after. Note every suite or check outcome
   line.
7. **Title and description, last.** List each claim the description
   makes about what changed, each fidelity claim verbatim ("exactly as
   planned", "no changes beyond"), each template section or checklist
   it fills, any AI-use statement, and any reply to the thread. Reading
   the description last is the step that keeps a confident "no changes
   beyond the plan" from shaping how the diff gets read.

## Evidence gathering

One pairing per family, using `references/evidence-guide.md` for where
each part lives. Record each fact as a short quote or a pointer to a
hunk, command, or line, never as a judgment.

- **Plan fidelity** (`diff-within-plan`, `description-matches-diff`,
  `shortfall-restated`): set the step 5 hunk list beside the step 4
  committed-work lines. Mark each hunk as planned, recorded in a
  deviation note, or outside the plan. Then walk the committed-work
  lines the other way and mark each as present in the diff, deferred
  by a note, or missing. Then set each step 7 claim beside the hunk
  list and mark it true or false of the diff. Last, check whether each
  plan deferral or deviation note is restated in the description.
- **Test evidence** (`evidence-decisive`, `repo-checks-run`): set each
  named repro or failure mode from the step 4 test plan beside the
  step 6 commands. Mark each as shown with after, shown without after,
  shown on a different path (a control, another input, another
  method), or not shown. Separately, set the suite or check commands
  from steps 2 and 4 beside the step 6 outcome lines and mark each as
  shown with outcome or not shown.
- **Diff quality** (`diff-clean`, `commits-legible`): from the step 5
  hunk lines, copy out every line marked debug, commented-out, dead,
  TODO, reformat, reorder, or churn, with its file. Copy the commit
  messages verbatim.
- **Standards and comms** (`template-asks-met`, `ai-policy-followed`,
  `maintainer-direction`): walk the step 2 ask list and mark each ask
  as met (quote the description text or name the diff file), marked
  not applicable with a reason, conditional and not triggered, or
  absent. Set the AI clause beside the step 7 AI-use statement and
  record which demand it meets. Set each step 3 direction beside the
  diff and description and mark it followed, engaged and declined, or
  ignored.

If a family's evidence is genuinely absent from the package, record
"absent" and name the section searched. Do not borrow from a
neighbouring section, and do not fetch anything in eval mode.

## Check execution

1. Execute the checks in the rubric's table order:
   `diff-within-plan`, `description-matches-diff`,
   `evidence-decisive`, `repo-checks-run`, `diff-clean`,
   `template-asks-met`, `ai-policy-followed`, `maintainer-direction`,
   `shortfall-restated`, `commits-legible`.
2. Grade every check, including after a required check has failed. The
   output carries all of them.
3. Grade each check from its gathered notes alone. Re-read only the
   named section when a note is missing or ambiguous; never re-read
   the whole package to settle one check, and never revise an earlier
   check because a later one came out badly.
4. Apply the rubric's pass condition to the outcome, not the
   write-up. A short description and a one-commit PR that meet the
   condition pass; a polished one that misses it fails.
5. Never take the PR's word for its own fidelity or testing. A claim
   in the description is a thing to check against the diff or the
   evidence, never evidence itself.
6. When evidence is absent, separate two cases. The PR is missing
   something it should contain (no test evidence, no suite outcome,
   no required section) is **fail**. The PR makes a claim the package
   cannot settle either way (a hunk's purpose the plan neither covers
   nor excludes, a policy whose wording is ambiguous) is **unclear**.
7. Record one line of evidence per check as it is graded: the hunk,
   quote, or command that decided it. A check without that line is
   not finished.

## Verdict assembly

1. Collect the grades for the eight required checks. If all eight are
   `pass`, the verdict is `accept`. If any is `fail` or `unclear`, the
   verdict is `reject`.
2. `unclear` on a required check counts as fail for the verdict and is
   still reported as `unclear` in the output.
3. The two preferred checks (`shortfall-restated`, `commits-legible`)
   never move the verdict. Report their grades and evidence lines as
   written.
4. Name the deciding check in the summary and quote its evidence line.
   On a reject, it is the first required check in table order that
   graded `fail` or `unclear`; list any other failed required checks
   after it by name. On an accept, it is the required check that came
   closest to failing.
5. If the package raised something this procedure has no step for,
   name it in the summary as a procedure gap rather than inventing a
   step. In live mode, list each voice-guide rule the draft title or
   description breaks, quoting the rule; it is reported, not scored.
6. Emit the summary first, then the fenced JSON block from SKILL.md
   last, one entry per check in table order, and nothing after it.
