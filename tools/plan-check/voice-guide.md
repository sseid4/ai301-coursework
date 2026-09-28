# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2; paste your filled week-2 voice-guide.md here, whole. It is not
re-authored and it is not graded as new work this week.

Then reread it with the plan comment in mind. Your claim and repro
comments promised and reported; a plan comment commits you to an
approach in front of the people who maintain the code. If your rules
do not cover that register (for example: how you state an approach you
are not certain of, or how you respond when a maintainer already
suggested a direction), extend the guide with what it needs. Extending
is allowed and encouraged; starting over is not required.

Live mode reads this file before your plan comment goes out and
reports any rule your draft breaks. Eval mode ignores it entirely,
because your voice is yours and carries no gold labels.
-->

## Who I am in threads

I am a contributor investigating one concrete issue, and I am still
learning this codebase. I report what I ran and what it showed so a
maintainer can verify the claim without taking my confidence on faith.

## Rules I write by

### Rule: Name the exact behavior

Start with the symptom and trigger I actually tested. Do not turn a small
observation into a claim about the whole project.

- Wrong: "This definitely breaks the parser everywhere."
- Right: "With the command below, version 1.2.3 returns the error shown instead of parsing the quoted value."

### Rule: Show the work

A claim comment points to the steps and artifact that support it. Agreement
alone is not a reproduction.

- Wrong: "+1, I can reproduce this too and will fix it."
- Right: "I reproduced this with the configuration below; the final output is `...`, and the full steps are in the report."

### Rule: Separate results from guesses

Use observed language for what happened and label a possible cause or next
step as a hypothesis.

- Wrong: "This proves the cache invalidation code is the cause."
- Right: "The failure occurs after the cache is refreshed; I have not isolated whether invalidation is the cause yet."

### Rule: Be honest about non-reproduction

If the exact attempt does not fail, say so plainly and identify the
environmental difference or boundary that may matter.

- Wrong: "The bug is fixed on my machine."
- Right: "I followed the issue's steps on macOS 15 with version 2.4.1, but I got the expected output; the issue reports Linux, so this does not rule out the report."

### Rule: Make the next step concrete

End with the smallest useful handoff: a missing detail, a comparison to
run, or an artifact the maintainer can inspect.

- Wrong: "Hopefully this helps."
- Right: "If the driver version is relevant, I can rerun this with the issue's stated driver and compare the logs."

### Rule: Propose an approach without pretending it is settled

A plan comment commits me to a route in front of the people who maintain
the code. I say which route I picked and what the evidence behind it is,
and I name the part I have not verified instead of writing past it.

- Wrong: "The fix is to refresh the view after push; I'll have a PR up shortly."
- Right: "Step 3 of my repro shows the stale color with no pager involved, so I plan to refresh the commits context in the push callback; I have not checked whether force push takes the same path, so that is the open question in the plan."

### Rule: Answer the direction already in the thread

If a maintainer has isolated a culprit, posted a build, or named the
approach they want, my comment starts from theirs. I take it up, or I say
plainly which part I am going elsewhere on and why, before describing my
plan.

- Wrong: "I plan to document the workaround in the man page and the README."
- Right: "You isolated this in `light_windows.go` and posted a patched build; I tested it and the first-key issue remains. Rather than duplicate that work I plan to document the `> /dev/tty` workaround as an interim note, unless you would rather I pick up the input-handling fix."

## Things I never post

- I never post a bare `+1` as a reproduction.
- I never call a result guaranteed after one unisolated setup.
- I never hide a version, platform, or configuration difference.
- I never claim to have tested an artifact or step I did not test.
- I never promise a delivery date or a timeline I have not already met once.
- I never present a plan as approved when nobody has approved it.
