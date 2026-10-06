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

Where it lives: the plan's cause is in the "Diagnosis" section of the Candidate plan (the "Summary" often restates it). The behavior that cause must explain is in the "Repro evidence" section: environment, numbered steps with timings or output, and the Expected/Actual statement. In live mode, the repro evidence is the student's posted repro comment on the issue (on a house issue, the repro pack as quoted in the drafts), and the diagnosis is in plan.md.

What good looks like: the stated cause is one specific mechanism, and every measurement in the repro evidence agrees with it, including the control runs (the ones where something was removed or switched off). It says how the cause was established (the plan's own testing or a named repro line). A cause that appears only as "identified in the thread" or "as noted by a commenter" is not grounded until a repro line supports it. A cause that any repro line contradicts is a fail, however confident the wording.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

Where it lives: the "Scope" section of the Candidate plan (the in-scope statement and the not-in-scope line) and the "Changes" list, which names the files or areas actually touched. In live mode, the same sections of plan.md.

What good looks like: one bounded change. Every file or area named is there because the diagnosis points to it, and the place the cause lives is on the change list, not the not-in-scope list. A not-in-scope line that rules out the diagnosed cause, or a change list full of things the diagnosis doesn't mention (a drive-by rewrite), is a sign the scope doesn't match the cause.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

Where it lives: the "Changes" section (files or areas, the approach, the order of steps) and any "Approach" or "Files to touch" section in the Candidate plan. In live mode, the same sections of plan.md.

What good looks like: a stranger could start without asking the author anything. Files or areas are named, each change says what will happen there, and the steps have an order. Phrases like "improve the handling" or "refactor as needed" with no file or action mean the plan is not executable yet. This family supports the Scope check and has no row of its own in the rubric.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

Where it lives: the "Test plan" section of the Candidate plan, read against the numbered steps in the "Repro evidence" section. In live mode, the Test plan in plan.md and the repro steps in the student's posted repro comment.

What good looks like: it re-runs the same steps the repro used (same command or action, same input) and names the result expected after the fix as a number, output, or behavior ("lands on the last line in under 1 s", "exit code 0"). Or it adds an automated test and says what it asserts and that it fails before the fix. "Verify it works" or a manual check with no expected result is vague. A test that checks something other than the diagnosed behavior (for example, only that a setting is registered) does not show the bug is fixed.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

## Honesty

Where it lives: the "Risks and unknowns" part of the Candidate plan and the "## Deviations" heading at the end of plan.md in live mode. Eval packages may have neither. If the Risks section is missing, the Risks check grades fail. It is a preferred check, so the verdict does not change. Also look in the Candidate plan comment for words that signal certainty ("I traced this to", "this fixes").

What good looks like: unknowns are stated plainly and tied to this change ("I haven't confirmed X; if it is Y, the plan changes in Z"). Confidence in the plan comment matches the evidence behind it. False confidence looks like a firm claim about a cause or fix that no repro line backs, with no risk or unknown listed. A recorded deviation says what changed and why.
## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

## Comms

Where it lives: the "Candidate plan comment" section at the end of the package, read against the "Thread highlights" and "Issue" sections (maintainer comments, existing PRs or duplicates, claims by non-maintainers with their role in parentheses) and against the "Repo facts" block (bug report template, contribution policy including any AI-use rule, latest release, archived status). In live mode, read the draft comment.md against the live thread and the repo's CONTRIBUTING file.

What good looks like, thread: the comment fits the thread as it stands. It acknowledges any existing PR, duplicate, or maintainer statement that bears on the plan, and treats a non-maintainer's claim as a lead rather than a fact unless the repro supports it. Boilerplate that would read the same on any issue, or one that ignores an open PR mentioned in the thread, is not thread-aware.

What good looks like, conventions: the comment follows each rule in the Repo facts block, such as an AI-use policy or a required template. If Repo facts state no rule that applies to a plan comment, there is nothing to fail. The comment also summarizes the plan accurately, with nothing the plan and repro don't back.
