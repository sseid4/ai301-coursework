# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Evidence gathered live from the GitHub API (reference date: 2026-09-15). Scope confirmed: codepath/pathreview-ai301-fa26-s3 is the scoped source, and the Path Review house rule (classmates' claims don't block) applies but changes nothing here — the thread is empty.

issue-select — live mode, 1 candidate

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
"verify_password raises UnknownHashError on malformed stored hashes instead of returning False" — opened by Aburke225 (COLLABORATOR) on 2026-09-10, state open, labels: bug, good first issue, api, tier-1, 0 comments.

Check | Weight | Grade | Evidence
maintainer-alive | required | pass | Newest of the last 5 main commits is 996fabe 2026-08-24 by Andrew Burke (Aburke225) — 22 days, inside 90. All 5 are human-authored; no [bot] suffix anywhere in the list.
repo-in-use | required | pass | archived: no; latest release: none published (API 404), so it passes on the push clause — pushed_at 2026-09-10, 5 days, inside 90.
scope-bounded | required | pass | One coherent change across two named files: make verify_password in core/security.py fail closed on passlib's UnknownHashError, then drop the @pytest.mark.xfail(strict=True) on test_verify_with_wrong_hash_format. Not a tracking list, not a usage question, no unsettled design debate, and a maintainer-filed good first issue.
not-claimed | required | pass | assignees: none; the repo has 0 pull requests in any state, so no linked or thread-mentioned PR is open; 0 comments, so no claim comment exists to date. Timeline holds only 4 labeled events by the opener.
no-dead-attempts | required | pass | Opened 2026-09-10 — 5 days old against the 730-day gate, with zero closed PRs and zero claim comments. The conjunction cannot trigger.
ai-policy-permits | required | pass | No CONTRIBUTING.md at root or .github/, no AI_POLICY.md, no AGENTS.md. The linked docs/CONTRIBUTING.md (189 lines) mentions AI nowhere — its only gate is "green CI is part of the contract". Silence, not a ban.
maintainer-responsive | preferred | fail | Sample of the 6 most recently updated issues (#67–#72): every one is maintainer-opened, seeded 2026-09-10, with 0 comments. No sampled issue drew a maintainer response, so it fails by the letter. This is a preferred check and does not affect the verdict.
newcomer-signposted | preferred | pass | Hits all three clauses: a good first issue label, an opener who is COLLABORATOR, and a body naming the exact files (core/security.py, tests/unit/test_security.py) — the xfail's own reason= string points back at issue #72 and manifest id H-05.

All six required checks pass, so the verdict rule accepts.

Fit (ranking only — single candidate, so nothing to order): Python, which your profile names as your strongest language and where you've done the most real work. It's also close to the top of your "want to get better at" list — a bug fix whose acceptance criterion is a pytest regression test going green, since the fix isn't done until the strict=True xfail is removed and the test passes for real.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass",
     "evidence": "Newest of last 5 main commits: 996fabe 2026-08-24 by Andrew Burke (Aburke225), 22 days before reference date; all 5 human-authored, no [bot] suffix."},
    {"name": "repo-in-use", "grade": "pass",
     "evidence": "archived: no; no releases published, but last push to any branch 2026-09-10 = 5 days, inside the 90-day push clause."},
    {"name": "scope-bounded", "grade": "pass",
     "evidence": "One change in core/security.py (catch UnknownHashError, return False) plus removing the xfail marker in tests/unit/test_security.py; maintainer-filed with a good first issue label."},
    {"name": "not-claimed", "grade": "pass",
     "evidence": "assignees: none; repo has 0 pull requests in any state; 0 comments on the thread, so no claim comment exists."},
    {"name": "no-dead-attempts", "grade": "pass",
     "evidence": "Opened 2026-09-10, 5 days old against the 730-day gate, with zero closed-unmerged PRs and zero claim comments."},
    {"name": "ai-policy-permits", "grade": "pass",
     "evidence": "No CONTRIBUTING.md at root or .github/, no AI_POLICY.md or AGENTS.md; docs/CONTRIBUTING.md mentions AI nowhere. Silence passes."},
    {"name": "maintainer-responsive", "grade": "fail",
     "evidence": "Sampled issues #67-#72 are all maintainer-opened with 0 comments; no sampled issue drew a maintainer response within 30 days. Preferred check, does not affect verdict."},
    {"name": "newcomer-signposted", "grade": "pass",
     "evidence": "Labels include 'good first issue'; opener Aburke225 is COLLABORATOR; body names core/security.py and tests/unit/test_security.py."}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

1. Full run, 20 scored items, rubric v1: "agreement: 16/20 scored items  (bar: 18/20: below the bar)"
2. Partial re-run with `--only issue-01,issue-04,issue-19,issue-15,issue-10,issue-05,issue-20,issue-09`, after rewriting `scope-bounded` and adding `no-dead-attempts`: "agreement: 6/8 scored items"
3. Partial re-run with `--only issue-04,issue-19,issue-05,issue-10,issue-20,issue-15`, after sharpening the tracker wording in `scope-bounded`: "agreement: 6/6 scored items"
4. Confirming full run, written to `eval-run.txt`: "agreement: 20/20 scored items  (bar: 18/20: PASS)" and "categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4"

The last score matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-15` (zulip/zulip#19589). Gold label: reject. My rubric v1 decided accept — the harness printed:

```text
issue-15  reject  accept   NO     graded accept
```

The reasoning that produced my rubric's result was that my v1 rubric had five required checks, and issue-15 passed all five. Its repo facts showed:

```text
this issue: assignees: none; linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)
```

My `not-claimed` check said that linked PRs that are all closed do not block because those are abandoned attempts and the issue is free again. The claim comments in the thread had also expired, so there was no live claim inside my 60-day window. The repo was active, the scope was one field-splitting change, and the policy allowed AI use with conditions. Based on those five checks, my rubric returned accept.

The problem was that the issue had been open since 2021-08-18 and had 97 comments, 16 claim attempts, and two abandoned PRs behind a `good first issue` label. The gold note identified the years of design debate and abandoned PRs as evidence that the issue was not actually a straightforward first issue.

I had skipped the evidence guide's age-and-history signal:

```text
Age and history: an issue open for years with several abandoned
attempts (closed, unmerged PRs in its history) is telling you
something about its real difficulty.
```

I added `no-dead-attempts` to address this gap, and issue-15 then graded reject.

**Check rationale**

The check I kept as preferred is `maintainer-responsive`. In `tools/issue-select/rubric.md`, it currently reads:

```text
| `maintainer-responsive` | Repo facts: the `maintainer first-response sample` list, days to first owner/member/collaborator comment per sampled issue. | At least one sampled issue drew a maintainer response within **30 days**; `unclear` when the sample is empty. Preferred on purpose: the sample is five issues wide and often maintainer-opened, so a healthy repo routinely shows four "no maintainer comment" lines. Too sparse to reject on, useful for ranking. | preferred |
```

I kept it preferred because making it required caused problems with clear-accept issues. For example, issue-01 had only one measurable maintainer-response datapoint in its five-issue sample, and that response was 32.9 days, outside the 30-day threshold. Issue-06 also had several issues opened by maintainers, where a lack of another maintainer comment does not necessarily indicate a problem. Making this check required would therefore reject issues based on a sparse sample rather than strong evidence. Keeping it preferred lets the signal affect ranking without allowing it to determine the verdict.

**Trade-offs**

The trade-off is that my rubric cannot reject a repository solely for being unresponsive. A project with a healthy commit history but no maintainer responses could still pass the required checks and be accepted. I accepted this limitation because the response sample is small and can be especially sparse when issues are newly created or opened by maintainers.

I also found an ambiguity when I ran the skill in live mode. Issues #67-#72 were all maintainer-opened and had zero comments. The skill graded `maintainer-responsive` as `fail` on two runs and `unclear` on another. The current wording says `unclear` when the sample is empty, but it does not clearly say what to do when the sample contains issues but has no measurable response datapoints. I would tighten that wording in a future revision.

The check did not change the final verdict for issue-72 because it is preferred. The issue received `fail` on `maintainer-responsive` but was still accepted because all six required checks passed. In the evaluation, issue-01 also failed `maintainer-responsive`, but its verdict was determined by `scope-bounded`. After fixing `scope-bounded`, it was accepted even though `maintainer-responsive` still failed.

---

## Selection rationale

**1. The issue's fit to my interests and the time available**

This issue fits my interests because it is a Python bug, and I am comfortable reading Python code. The change also seems manageable within the time I have available. I like that it includes a specific regression test because I want more practice understanding and writing tests.

**2. What the verdict identified correctly, and what I weighed that the rubric could not**

The rubric correctly identified that the issue is unclaimed, the scope is reasonably bounded, and the repository is active. It also correctly found that there is no AI-use restriction that would prevent me from working on it. What I considered beyond the rubric was whether I personally understood the expected behavior. Returning `False` for a malformed stored hash instead of allowing `UnknownHashError` to escape seems like a clear behavior to investigate, but that kind of code-level understanding is not something my issue-selection rubric is designed to measure.

**3. The anticipated difficulty in claiming it**

I don't expect claiming this issue to be difficult because there are currently no comments or assignees on the issue. The main difficulty I anticipate is making sure I understand how `verify_password` is used elsewhere before changing its behavior, so that the fix does not introduce a different problem.
