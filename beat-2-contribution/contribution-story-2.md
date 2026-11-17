# Contribution: Person view: move Recent Emails out of the main column

**Contribution Number:** [2]
**Student:** Siyam Seid
**Issue:** [ChurchCRM/CRM#10602](https://github.com/ChurchCRM/CRM/issues/10602)
**Pull Request:** [ChurchCRM/CRM#11686](https://github.com/ChurchCRM/CRM/pull/11686) (draft)

<!--
This is the blank template for an extra contribution. Copy this file
to contribution-story-2.md (then -3, and so on) to record another
one, and set Contribution Number to match. Extra contributions are
welcome and never graded; contribution-story.md is the graded one.
-->

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

```
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

### Unit 5: Reusing the issue-scout workflow to select a second contribution

I reused the issue-scout workflow and fit profile from my first contribution to identify another open-source issue. The selected issue is open, unassigned, labeled `good first issue`, and focused on removing duplicate email-history information from the person view. I will record any new scan result or limitation here after completing that work.

### Unit 6: Claiming and reproducing the ChurchCRM issue

I used the voice guard and repro-check to validate and post my claim, then reproduced the issue in ChurchCRM's Docker development environment. The first reproduction report was rejected because it did not include enough evidence and initially described an empty timeline filter. I reran the filters, captured both screenshots, and updated the report to show the card's five rows versus the timeline's six rows; the revised package was accepted.

### Unit 7: Installing the ship-gate and preparing the implementation plan

I installed the ship-gate for the ChurchCRM clone and created a manifest from the repository's CI checks. The manifest runs dependency installation, linting, PHP syntax validation, and packaging before a PR. I also prepared a plan that limits the change to removing the duplicate person-view card, improving timeline email details and navigation, and updating the existing Cypress coverage. On 2026-11-16 the ship-gate's first run passed all four repo checks, but my pr-precheck rejected the branch: it read a diff against my stale local `master` (about 890 files), found no before run in my evidence, and saw an empty Screenshots section. I moved local `master` to upstream, added a before/after run of the spec, and filled in the UI checklist; the second run passed and the hook let the draft PR through.

### Unit 8: Installing the PR monitor and writing my playbook for #11686

On 2026-11-16 I installed the PR monitor and checked that `--state` found #11686 from my branch (draft, no review yet, quiet for under a day). To set my staleness threshold I timed the eight most recent merged PRs from outside contributors: all merged within 0.1 to 1.7 days, and the first reply on each came from a bot, so I set 4 quiet days for a first review and added a rule that a draft PR, or one with CI waiting on approval, gets no review nudge. The first full run picked the nudge form but drafted a wait note instead, quoting my 4-day rule and naming 2026-11-21 as the earliest nudge date. I posted nothing. The note also flagged that the run report doesn't show whether the PR is a draft, so I checked on GitHub (it still is) and will start the 4-day count when I mark it ready for review.

---

## Why I Chose This Issue

I chose this issue because it is a focused UI cleanup in an unfamiliar PHP application and has a clear user-visible outcome. The issue reports that the person view shows the same email history in both a large “Recent Emails” card and the timeline, and asks to keep the timeline/full history while adding an “Email history” action-menu entry. I hope to learn how ChurchCRM connects PHP views, timeline data, navigation, and Cypress UI tests.

**Toolkit**
- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: An accepted verdict for ChurchCRM/CRM#10602 with a moderate difficulty note.
- Difficulties: The scout's maintainer-responsiveness sample was unclear, and the repository uses PHP rather than my strongest languages.
- What changed: I recorded the scope as feasible and identified the person view, email-history route, timeline, action menu, and Cypress coverage as the initial work areas.

---

## Understanding the Issue

### Problem Description

The person view displays email history twice: once in the main-column “Recent Emails” card and again in the timeline's Emails filter. The extra card takes space and duplicates information that users rarely need on every person page.

### Expected Behavior

The main-column Recent Emails card should be removed, the existing timeline email entries should remain useful, and users should be able to open the full paginated history from an “Email history” action-menu item. The family view should also be checked for the same card.

### Current Behavior

The current person view renders a five-row card titled “Recent Emails” with a “Show all” link above the tabbed timeline. The repository already has a paginated `/people/view/{id}/emails` page and an Emails timeline filter, so the same information appears in multiple places.

### Affected Components

The primary files are `src/people/views/person-view.php`, `src/people/routes/view.php`, `src/people/views/person-emails.php`, the shared timeline view, and the Cypress email-history UI test. The action menu and any family-view equivalent also need review.

**Toolkit**
- Tool: Local Docker development environment and browser inspection.
- What it produced: Confirmed the person view's duplicate email-history presentation and identified the relevant PHP views, route, timeline, and Cypress test.
- Difficulties: The family view did not contain the same Recent Emails card, so the duplicate is limited to the person view.
- What changed: I narrowed the implementation scope to the person view while preserving the existing family-view behavior.

---

## Reproduction Process

### Environment Setup

I used macOS with ChurchCRM 7.8.4 in the Docker development environment at `http://localhost`. I ran `npm run docker:dev:start` and `npm run docker:dev:build`, then used the seeded database and signed in with `admin` / `changeme`. The reproduction was run on commit `b494611e7`.

### Steps to Reproduce

1. From the repository root, run `npm run docker:dev:start` and `npm run docker:dev:build`.
2. Open `http://localhost`, sign in with `admin` / `changeme`, and open `/people/view/2` for Mathew Campbell.
3. Observe the Recent Emails card, which shows five rows and `Show all (6)`.
4. Select the Timeline's All filter and observe six email rows.
5. Select the Emails filter and observe the same six rows, including `Message not sent (failed): Potluck this Sunday` and `Oldest message`.
6. Open `/people/view/2/emails` and confirm the full paginated history.
7. Open `/people/family/13` and check for the same card.

### Reproduction Evidence

- **Commit showing reproduction:** No code commit; reproduction ran on `b494611e7`.
- **Screenshots/logs:** [`screenshot_email_above_timeline.png`](/Users/siyam/TF-AI301/fall/week6/screenshot_email_above_timeline.png) shows the Recent Emails card with five rows and `Show all (6)`. [`timeline-email-filter.png`](/Users/siyam/TF-AI301/fall/week6/timeline-email-filter.png) shows the Emails chip selected with count 6 and the six timeline email rows.
- **My findings:** The duplicate reproduces on the person view: the card shows five emails while the Emails timeline filter shows the same six emails, including the failed Potluck message and the sixth Oldest message. The timeline rows already include the email type and failed status in their text; the missing detail is the sender information. The full paginated history is available at `/people/view/2/emails`, and family ID 13 has no Recent Emails card.

**Toolkit**
- Tool: `repro-check`, Docker, and browser screenshots.
- What it produced: An accepted reproduction package with passing required and preferred checks.
- Difficulties: The first report iteration did not distinguish a failed filter observation from the final confirmed duplicate, so I reran the filters and captured both states clearly.
- What changed: I corrected the report to compare the card's five visible rows with the timeline's six rows and documented the family-view boundary.

---

## Solution Approach

### Analysis

The person route loads five recent email-log rows for a separate card, while `TimelineService::emailsForPerson()` loads the same email history for the timeline. `person-view.php` renders the card before the timeline, creating duplicate presentation of the same records.

### Proposed Solution

Remove the duplicate Recent Emails card and its separate five-row data load, while keeping the timeline and existing paginated email-history page. Add sender details to timeline email rows, link to the full history from the Emails filter, and add an Email history action-menu entry. The family view and email-log storage will not be changed.

### Implementation Plan

1. Remove the person-view card and unused route data, and add the action-menu link.
2. Preserve the timeline's type/status text and add sender details plus a full-history link.
3. Update the existing email-history Cypress test for the removed card, timeline rows, and navigation.

Deviations from the plan, all small:

- The action-menu label is "Email History", not "Email history". ChurchCRM's i18n guide says to reuse an existing msgid before adding one, and "Email History" already exists. "View all", "Sent by" and "Automatic" are also existing strings, so the PR adds no translation keys.
- Besides the first Cypress test, the "Show all opens the full, paginated history" test also had to change, because it started by clicking the removed card's link. It now starts from the Timeline's View all link.
- The View all link only renders when the caller passes `$emailHistoryUrl`. The timeline partial is shared with the family view, so this keeps the link off pages that are not about one person.

**Toolkit**
- Tool: The ChurchCRM CI documentation and the ship-gate plan template.
- What it produced: A focused implementation plan in `plan.md` covering the route, views, timeline service, and Cypress test.
- Difficulties: The issue's requested timeline details needed to be reconciled with the current UI, which already shows type and failed status as text.
- What changed: The plan narrows the new timeline data to sender information and preserves the existing type/status behavior.

---

## Testing Strategy

The main proof is the repo's own email-history spec, `cypress/e2e/ui/email/email.history.spec.js`, run before and after the change on the seeded Docker stack (`npm run docker:test:reset:db` first). The updated first test checks that `#email-history-card` is gone, that the Emails filter lists at least six email rows containing "Sent by" and "Church Admin", that the Timeline View all link opens `/people/view/2/emails`, and that the Actions menu contains "Email History". I asserted "at least six" rather than exactly six because other specs can add email rows to person 2.

| Code under test | Result |
|---|---|
| Before: upstream `master` `c23807e06`, with the spec from my branch | 3 passing, 2 failing: `#email-history-card` "was continuously found", and `#timeline-email-history-link` was never found |
| After: my branch `b980387d9` | 5 passing, 0 failing |

The two failures are exactly the issue: the duplicate card, and no path from the Timeline to the full history.

Repo checks on the rebased branch:

- `npm run lint`: all 32 checks passed.
- PHP unit tests (`tests/php/run.js unit`): 47 passed, 1 skipped.
- Ship-gate manifest: `npm ci`, lint, `npm run build:php:validate -- --all` and `npm run package` all passed.

Manual check: I re-ran my reproduction on `/people/view/2` in the browser. The card is gone, the Emails filter shows all six emails with "Sent by: Church Admin" or "Sent by: Automatic", and View all sits next to the filters. I checked it in light and dark themes at about 740px wide. I did not check phone width separately, and the PR says so.

**Toolkit**
- Tool: Cypress against the Docker test stack, ChurchCRM's `npm run lint` and PHP test runner, and the ship-gate manifest.
- What it produced: A before/after table for the spec (3/5 on master, 5/5 on my branch), lint 32/32, and PHP unit tests 47 passing.
- Difficulties: My Mac has no PHP or Composer, so the commit and push hooks failed on every PHP file. Cypress first died with `MODULE_NOT_FOUND` because VS Code sets `ELECTRON_RUN_AS_NODE=1` in its terminals. `npm ci` kept timing out downloading the Cypress app on a slow connection. One PHP unit test (`TelemetryServiceTest`) failed only because the Docker container sets `CHURCHCRM_DISABLE_POSTHOG_TELEMETRY=1`.
- What changed: I ran `php` and `composer` through small wrapper scripts that call the Docker webserver container, cleared `ELECTRON_RUN_AS_NODE` for Cypress, installed with `CYPRESS_INSTALL_BINARY=0` and reused my cached Cypress 15.21.1 app, and cleared the telemetry variable for the PHP test run. With those, every hook check ran for real instead of being skipped.

---

## Implementation Notes

### 2026-11-15

I reviewed the proposed six-step solution against the code before writing it. Most of it held, but a few points needed changing. The timeline partial `timeline-events.php` is also used by the family view, so the View all link had to depend on a variable only the person view sets. Copying the sender lookup from `EmailLogService::toArray()` into the timeline loop would run one query per row for up to 200 rows, so I collected the sender IDs and loaded the names in one query instead. The sender goes in its own `emailSentBy` key and is escaped when rendered, because slim timeline rows print `text` without escaping. I also found that the paginated-history Cypress test clicked the card's Show all link, so it needed updating too.

### 2026-11-16

I committed the change as "Fix duplicate person email history" and pushed it. A review of the branch found two cleanups. I had added the "Sent by" block to both branches of the timeline template, but email items are always slim, so the second block could never run. I had also split a one-line include into four lines for no reason. I removed both in a second commit, "Remove unreachable timeline sender block". I also did not commit a `package-lock.json` change that only dropped `libc` fields, which came from my npm version rewriting the lockfile, not from my change.

Upstream `master` had moved 174 commits past my base. Git merged the two cleanly, and none of the upstream changes touched the code I edited, so I rebased onto `c23807e06` and re-ran the spec. The pre-push hook then failed four lint checks. All four came from my setup: no PHP or Composer on my Mac, and a capture check comparing against my fork's stale `master`. After routing PHP and Composer through Docker and pointing the capture check at upstream `master`, all 32 checks passed and I force-pushed with `--force-with-lease`. The first push attempt timed out uploading 174 commits' worth of objects; a retry with a bigger upload buffer worked.

**Toolkit**
- Tool: Claude Code for reviewing the proposal against the code, implementing, and running the hooks; git for the rebase.
- What it produced: Two commits on `fix/10602-person-email-history`, rebased onto current upstream `master`, changing 5 files (46 lines added, 56 removed).
- Difficulties: The repo's pre-commit and pre-push hooks assume PHP and Composer on the host. My first wrapper only translated paths at the start of an argument, so the PHP test runner's `junit:/Users/...` option still broke, and the Composer check copied files to a temp folder Docker could not see.
- What changed: I fixed the wrapper to translate host paths anywhere in an argument, set `TMPDIR` to the repo's git-ignored `temp/` folder, and used the capture check's own `CAPTURE_BASE_REF` setting. I would install PHP locally (`brew install php`) before the next PHP contribution.

---

## Pull Request

**PR Link:** https://github.com/ChurchCRM/CRM/pull/11686 (opened as a draft on 2026-11-16, as Hazel asked)

**PR Description:** The PR follows ChurchCRM's template. Goal: the person view showed the same email history twice, and the card pushed the Timeline down. Done: the card and its five-row query are removed; Timeline email rows show "Sent by" (a name or "Automatic"); a View all link next to the Timeline filters and an "Email History" Actions menu item both open `/people/view/{id}/emails`; sender names load in one query; the family view is unchanged. It says `Fixes #10602`. Validation lists the spec's before (3/5) and after (5/5) results, lint 32/32, PHP unit tests 47 passing, and that no new translation keys were added. The UI checkbox records the light and dark check and says phone width was not checked. Risk notes that the email body preview modal no longer opens from the person view (it still does on the full history page) and offers to add it to the Timeline rows. I attached the light-theme screenshot of the Emails filter after opening, because `gh` cannot upload images.

**Maintainer Feedback:**

- 2026-11-16: none yet. GitHub is holding the two CI workflows ("CI: build, test and package (quick)" and "Security: CodeQL & Semgrep") until a maintainer approves them, which is normal for a first-time contributor. CodeRabbit skipped its review because the PR is a draft.

**Status:** Awaiting review (draft, CI awaiting maintainer approval)

**Toolkit**
- Tool: the ship-gate (manifest checks plus my pr-precheck), and `gh pr create` run through Claude Code so the hook checked it.
- What it produced: First run: all four repo checks passed, but pr-precheck rejected the branch on five rubric checks. Second run: PASS, and the hook honored that pass when I opened the PR.
- Difficulties: pr-precheck read the diff against my local `master`, which was still at my old base, so it saw about 890 files and could not find my two commits. It also failed `evidence-decisive` (after results only) and `template-asks-met` (empty Screenshots section and unchecked UI box).
- What changed: I moved local `master` to upstream `c23807e06`, ran my spec against the original code to get a real before result, and filled in the UI checkbox and Screenshots section honestly. Next time I will update local `master` right after rebasing, and record the before run as part of the plan instead of after the gate asks for it.

---

## Learnings and Reflections

### Technical Skills Gained

- How a ChurchCRM page is put together: a Slim route in `src/people/routes/view.php` builds `$pageArgs`, a PHP view renders them, and a shared partial (`timeline-events.php`) is included by more than one page, so a change to it has to be checked against every caller.
- Avoiding the N+1 query problem in Propel: collecting the sender IDs first and loading them with one `filterById(..., Criteria::IN)` query instead of one `findPk()` per row.
- ChurchCRM's i18n rules: translations are keyed by the English msgid, so reusing an existing string ("Email History") keeps every translation, while a new spelling ("Email history") would start from zero.
- Writing a Cypress assertion that proves the bug: running the new spec against the old code and getting failures that name the bug.
- Rebasing a pushed branch onto a moved upstream, checking it with `git merge-tree` first, and pushing with `--force-with-lease`.

### Challenges Overcome

- The commit and push hooks failed because my Mac has no PHP or Composer. I didn't bypass them with `--no-verify`. I ran PHP and Composer inside the Docker container I was already using, so every check really ran.
- Cypress failed with `MODULE_NOT_FOUND`, which looked like a version mismatch. The real cause was `ELECTRON_RUN_AS_NODE=1` inherited from VS Code.
- My pr-precheck rejected the first ship-gate run. Its reasons were fair: a stale base, no before run, and missing screenshots. Fixing them made the PR stronger, not just passable.

### What I'd Do Differently Next Time

I would set up the full toolchain (PHP, Composer, a working Cypress) before writing any code, because most of my time went into environment problems, not the fix. I would run the new spec against the original code as soon as I wrote it, so the before evidence exists before the gate asks for it. I would also keep my fork's `master` synced with upstream, so checks that compare against `origin/master` see the real base.

---

## Resources Used

- [ChurchCRM/CRM#10602](https://github.com/ChurchCRM/CRM/issues/10602): the issue and DawoudIO's proposal
- [My claim comment](https://github.com/ChurchCRM/CRM/issues/10602#issuecomment-6728349541)
- Hazel's reply on the issue confirming the proposal and asking for a draft PR with a person-view Cypress spec
- [My draft PR, ChurchCRM/CRM#11686](https://github.com/ChurchCRM/CRM/pull/11686)
- ChurchCRM's `CLAUDE.md`, `.agents/skills/churchcrm/git-workflow.md`, `code-standards.md` and `i18n-localization.md` (branching, commits, PR rules, string reuse)
- ChurchCRM's `.github/PULL_REQUEST_TEMPLATE.md`
- Unit 7 `INSTALL.md` (ship-gate) and Unit 8 `INSTALL.md` (PR monitor) from the course starter repos
