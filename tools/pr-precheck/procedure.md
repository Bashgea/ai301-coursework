# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. **Live mode only:** read `scope.md` first and confirm the `Repo:` line is filled in. Then read `voice-guide.md`. Skip both in eval mode.
2. Read the **plan** before anything else in the package. In an eval bundle this is the plan-context block; live, it is `plan.md`. Write down four things: the files and boundary the plan names, each numbered step, anything the plan says is out of scope, and the test plan (every repro, failure mode, or test it names). Also write down every note under `## Deviations`. The plan goes first because every later check compares something against it. If you read the diff first, you will grade the change on its own merits and miss what it quietly adds or leaves out.
3. Read the **repo facts**: the PR template's sections and checkbox items, any required file (changelog, whatsnew), the contribution policy, and any AI-use policy. Live, read `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. Write down each ask as a short list, so you can tick them off later.
4. Read the **issue and its thread** and note the original repro and any maintainer direction. Treat it as context for the plan; it does not replace the plan.
5. Read the **PR title and description** and write down every claim it makes about what was done, what was not done, and how closely it follows the plan.
6. Read the **diff** (eval: the diff section; live: `git diff main...HEAD`, committed changes only). List every changed file, and for each hunk write one line saying what it does.
7. Read the **commit list**, then the **test evidence**. Write down what was run, the before and after output, and the result of each repo check.
8. Only now start grading. Do not grade any check while reading.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

Gather the evidence for every check before grading any of them. Use `references/evidence-guide.md` for where each item lives. Record each as a pair: what the plan, repo, or template says, and what the PR shows.

- **Diff stays inside the plan:** pair the list of changed files and hunks (step 6) with the plan's files, steps, and scope-out list plus the deviation notes. Mark each hunk "in plan", "in deviation note", or "not in plan".
- **Description matches the diff:** pair each claim from the description (step 5) with the hunk or file that backs it. Mark each claim "backed", "contradicted", or "no matching hunk". Pay special attention to fidelity claims ("exactly as planned", "no changes outside X") and to claims that something was added.
- **Plan steps delivered or disclosed:** pair each numbered plan step with the hunk that implements it. If none does, search the description and deviation notes for a statement that the step was deferred or dropped, and note its wording.
- **Before and after on the plan's repro:** pair the plan's repro (command, input, expected failure) with the test evidence. Record whether the same command or input appears, whether both a before and an after output are shown, and whether the run goes through the code the diff changes. Mark a control case, a different input, or a bare claim ("tested locally") as not the repro.
- **Plan's test plan covered:** pair each test-plan item with a piece of evidence. Record each item as "shown", "disclosed as not run, with a reason", or "missing".
- **Repo's own checks run:** list the commands or suites the repo or template names to run, and for each find where the test evidence names it and what concrete result it states (a count, "clean", "no diff", output lines, or an honest failure). Mark each "shown", "named with no result", or "not mentioned". Separately list the template's checklist statements that are not commands (formatted, i18n, docs) and mark each "consistent" or "contradicted by the diff or evidence".
- **Diff is free of debris:** scan the added and removed lines of the diff itself. Record each debug print or log line, commented-out code, dead or unused function, TODO or experiment note, whitespace, format, or import-reorder churn, and hunk unrelated to the plan, with its file and line.
- **Template and issue link satisfied:** pair each template section and required file from step 3 with the description (is the section filled with real content?) and the diff (is the required file present?). Record whether `Closes #<issue>` is there and whether any ticked box is untrue.
- **AI use disclosed:** if step 3 found a disclosure requirement, find the description's statement of AI use and record whether it is in the author's own words or only a placeholder. If no requirement exists, record "none required".
- **Commits are readable:** record each commit message and mark placeholders ("wip", "fix", "misc").
- **Description states what and why:** record whether the summary says what changed and why.

If a piece of evidence does not exist in the package, record "absent" for it. Never fill the gap from memory, from the live repo in eval mode, or from what the fix probably does.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in the order of the rubric's table, one at a time, using only the evidence recorded for that check.
2. Grade each check by its pass condition and nothing else. Do not weigh the length, tone, or polish of the PR.
3. Grade `pass` only when the evidence meets the pass condition. Grade `fail` when the evidence shows the condition is not met (a hunk not in the plan, a claim the diff contradicts, a missing before-or-after, a debris line). Grade `unclear` only when the evidence the check needs is genuinely absent from the package, not when you did not look.
4. For each grade write one evidence line: the fact or short quote that decided it, with its file or section. "Looks fine" is not an evidence line.
5. A check may be graded from the recorded evidence without re-reading the package, except two: re-scan the diff for "Diff is free of debris" and "Diff stays inside the plan", because a missed line there is the failure those checks exist to catch.
6. If a check's pass condition says "if the repo states no template, no asks, no AI policy", the check passes. Write "none required" in its evidence line.
7. If the procedure does not tell you how to handle something you meet, do not invent a step. Note the gap in the summary and grade by the rubric's pass condition.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Collect the grades for all checks.
2. Apply the rubric's verdict rule: `accept` if and only if every `required` check is `pass`. A `required` check graded `fail` or `unclear` makes the verdict `reject`. `preferred` checks are reported but never change the verdict.
3. When the verdict is `reject`, quote the first failing (or unclear) `required` check in the rubric's table order as the deciding check, and give its evidence line first in the summary. Report the other failures after it.
4. When the verdict is `accept`, say in one line that every required check passed, and list any `preferred` check that failed.
5. Live mode only: after the grades, list each voice-guide rule the title or description breaks. Keep this list apart from the checks, and never let it change the verdict.
6. Print the readable per-check summary, then end the reply with the fenced JSON block from SKILL.md, with one entry per check in the rubric's order and the verdict last. Nothing may follow the block.
