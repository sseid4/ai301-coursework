# Contribution: Figure out what fields to show per version

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Siyam Seid
**GitHub Username:** sseid4
**Issue:** [AcademySoftwareFoundation/dna#49](https://github.com/AcademySoftwareFoundation/dna/issues/49) 
**Pull Request:** Not opened yet

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: [one line on the tool work]**

I created a fit profile describing my experience with Python,
JavaScript/TypeScript, Java, C/C++, SQL, HTML/CSS, and Dart. I identified
Python and JavaScript/TypeScript as my strongest areas and prioritized
regression testing, React/Next.js, SQL, Docker, and unfamiliar-codebase
debugging as skills I want to improve.

I then wrote four search queries for Python, TypeScript, testing, and Docker
issues. I ran the issue-scout scan and saved its results. The scan accepted
four issues and rejected one, but the accepted results clustered in only two
repositories and the broad Python query returned many issues, so I identified
those as possible improvements for a later scan.

**Unit 6: [one line on the tool work]**

I installed the voice guard, updated the real-repository repro-check and
plan-check scopes, and patched the issue-scout rubric so `scope-feasible`
replaced the stricter `scope-bounded` check. The first claim-grade attempt
still used the old Path Review scope, so I verified and replaced the
installed scope before rerunning it. The updated claim grade accepted the
draft, and I posted the claim after checking that the issue had no existing
claim or assignee.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue while using my issue-scout to search for possible contribution opportunities. The issue is about deciding which version information should appear in DNA’s version header. My scout originally rejected it because the exact fields had not been decided yet. The issue depends on user stories and conversations with production teams, and that information is not included in the issue.

I still chose it because it is one clear piece of work, even though it has some uncertainty. I would describe it as ambitious because I need to learn an unfamiliar codebase, find out what version information the backend already provides, and ask the maintainers for feedback before deciding what to change. The difficulty note does not mean the issue is impossible; it means I should clarify the open design questions early.

This issue fits me because I want more practice understanding an unfamiliar project and working with maintainers before writing code. I also want to learn how product needs affect a technical decision. My claim draft passed the claim check, and I posted my claim comment on the issue.

My scout was useful for checking whether the repository was active, whether the issue was already claimed, and whether the project allowed contributions. However, its original scope check was too strict because it rejected issues with open design questions. I changed the check from scope-bounded to scope-feasible, so an issue can still be considered if it is one clear piece of work. The remaining uncertainty is reported as a difficulty note instead of automatically rejecting the issue.

Claim comment: https://github.com/AcademySoftwareFoundation/dna/issues/49#issuecomment-6549988166

**Toolkit**

-Tool: issue-scout, using my search strategy and rubric; then repro-check
  for the claim draft.
- What it produced: a scan read-out, an original `reject` for DNA #49 under
  `scope-bounded`, and an updated claim-grade `accept` after the wild scope
  was installed. The issue is **ambitious** because its design edge is open.
- Difficulties: the scout's sandbox scope initially rejected the DNA
  repository, and its original scope check rejected an issue that is one
  piece of work but requires product input.
- What changed: I installed the Unit 6 wild repro-check scope, replaced
  `scope-bounded` with `scope-feasible` in the rubric, added the difficulty
  note section, and recorded the replacement in `scan-notes.md`.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
