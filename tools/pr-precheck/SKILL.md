---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this ready to submit?
A PR package is a candidate pull request (its title, description,
commit list, diff, and test evidence) read against the plan it claims
to implement (with that plan's deviation and deferral notes) and the
issue the plan belongs to, including the repo's stated PR asks and the
issue thread. Grade exactly one package per run. Do not grade the
issue, re-grade the plan, review code style, or suggest a better fix:
the plan is the agreed bar, and you decide whether this PR meets it
and is ready for a reviewer.

## Inputs and modes

Work out the mode first, from what you were handed.

- **Live mode**: the student's own submission, before it goes out.
  Read these inputs:
  - the plan: `plan.md` in the student's working copy, including any
    deviation note added during the build;
  - the diff: run `git diff main...HEAD` (three dots) from the working
    copy, which is everything the branch changes relative to the
    repo's default branch; take the commit list from
    `git log --oneline main..HEAD`. If the default branch is not
    `main`, substitute its name;
  - the draft PR title and description: the draft file the student
    names, or the text they paste;
  - the test evidence: the captured terminal output the student
    provides, or what the draft description pastes. Do not run the
    tests yourself to fill a gap: evidence the student did not capture
    is absent;
  - the issue: fetch the thread with `gh issue view <number>
    --comments` in the scoped repo, and read the repo's
    `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and
    any stated AI policy.
  A house-chain student reads the house plan and the house repro pack
  in place of their own plan and repro; the same checks grade the
  same things. Use the PR URL as the `item` once the PR exists, and
  before that the branch's compare URL
  (`https://github.com/<repo>/compare/main...<fork>:<branch>`).
- **Eval mode**: a package bundle (a markdown file, or its JSON twin,
  holding repo facts, the issue, thread highlights, plan context, and
  a candidate PR). The bundle is the whole world: every fact comes
  from its text, nothing is fetched, and nothing else is read. The
  `source:` line names a real issue, but never look it up; the bundle
  wins. Eval mode always grades the complete package: every check in
  the rubric and the full verdict rule. Use the bundle id as `item`.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything else.
If its repo line still carries an unfilled placeholder (anything in
angle brackets, such as `<ORG>/<PATH-REVIEW-REPO>`), stop without
grading and tell the student to get their cohort's scope file from the
instructor. Never guess a scope, and never infer it from the working
copy's git remote. If the PR's target repo is not the scoped repo,
refuse to grade it and say why. Apply the house rules scope.md lists
as part of how evidence is read: in Path Review that means the PR
comes from a `fix/<issue-number>-<slug>` branch on the student's fork,
delivers one posted plan for one issue, and fills every section of the
repo's PR template. A broken house rule shows up in the check that
reads that evidence. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the scope, read `voice-guide.md`: the student's own
rules for writing upstream, with wrong/right pairs. Hold the draft PR
title and description against every rule and every "never post" line,
and report each one the draft breaks in the summary, quoting the rule
and the offending line. The voice guide is reported, not scored: it
never changes a check grade or the verdict, because no rubric check
reads it, and voice is personal. In eval mode, ignore
`voice-guide.md` entirely; the universal communication checks live in
the rubric.

## Component reads

- `rubric.md` is the brain: it defines every check (name, evidence,
  pass condition, weight) and the verdict rule. Grade exactly the
  checks it lists, under exactly those names, and no others.
- `references/evidence-guide.md` is the map: for each evidence family,
  where it lives in a bundle or in live mode, and what good looks
  like. Look for evidence where it says.
- `procedure.md` is the hands: execute it as written, in its order
  (read order, evidence gathering, check execution, verdict assembly).
  Where the procedure is silent on a step the package needs, name the
  gap in your summary rather than improvising around it.

If `rubric.md` has no checks or no verdict rule, or `procedure.md` has
no steps, stop and say so without grading. This tool does not invent
checks or steps at runtime: an empty tool that grades anyway produces
verdicts that look like judgment and are noise.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; put reservations in a
check's evidence line, not in the verdict. Before the JSON you may
give a short readable summary: one line per check, the deciding check
with its evidence, any procedure gaps, and in live mode the voice-guide
notes. End your reply with this fenced JSON block, valid and last,
with nothing after it, one entry per rubric check in table order:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the hunk, quote,
  or command output that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse, complete PR can be ready,
  and a confident, well-formatted one can be hiding drift. Read the
  diff against the plan and the evidence against the test plan;
  never take the description's word for either.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule
  is silent, an unverifiable claim is a failing one: a PR you cannot
  verify from the package is not ready to submit.
