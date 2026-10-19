# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff stays inside the plan | The diff's changed files and hunks, read against the plan's stated files and scope boundary plus any `## Deviations` note | Every changed file and hunk traces to a plan step or a recorded deviation. Fail if the diff adds anything the plan did not ask for or scopes out (a new flag or config option, a rewrite or rename of unrelated code, an edit to a file the plan never touches) and no deviation note records it. Extra work that is adjacent or small still fails if unannounced. | required |
| Description matches the diff | The PR description's claims about the change, read against the diff | Every statement about what the PR does or does not do is true of the diff, including fidelity claims ("exactly as planned", "no changes outside X") and claims that something was added (docs, tests, warnings). Fail if the description says more than the diff delivers or claims fidelity the diff contradicts. | required |
| Plan steps delivered or disclosed | Each step in the plan, read against the diff and the description or deviation notes | Every plan step appears in the diff, or the description or `## Deviations` explicitly says it was deferred or dropped and why. Fail if a plan step is missing from the diff and nothing says so. A disclosed, bounded shortfall passes. | required |
| Before and after on the plan's repro | The test evidence, read against the plan's repro | The test evidence shows the plan's own repro run before the fix and after it, with visible output (transcript, diff, or output lines), and the run exercises the code path the change touches. Fail if the evidence is only a claim ("tested locally", "tests pass", "works on my machine"), if it exercises the unchanged path (a control case, or a different input than the issue's repro), or if the before or after is missing. | required |
| Plan's test plan covered | The plan's test plan items, read against the test evidence | Every item in the plan's test plan (each named failure mode, scenario, or test) has matching evidence, or the description says it was not run and why. Fail if a named item is silently missing. | required |
| Repo's own checks run | The commands or suites named in the repo's PR template or contribution docs (for Path Review: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`), and the template's checklist statements, read against the test evidence | If the repo names commands or suites to run, the evidence shows each one run: it names the command or suite and states a concrete result (a pass count, "clean", "no diff", the output lines, or an honest failure or "no tests ran"). Fail if a named command or suite is never mentioned in the evidence, or is covered only by a generic claim that names no command and no result ("tests pass", "tested locally"). Template checklist items that are statements rather than commands (code formatted, text internationalised, docs updated) pass when the description states them and nothing in the diff or evidence contradicts them; fail only if the diff or evidence contradicts a claimed item. If the repo names no checks, this check passes. | required |
| Diff is free of debris | The diff itself (added and removed lines) | No debug prints or logging, no commented-out code, no dead code or unused functions, no leftover TODO or experiment notes, no whitespace, formatting, or import-reorder churn, and no unrelated hunks. Judge the diff, not the commit messages. Fail if any such line or hunk is present, even when the fix itself is right. | required |
| Template and issue link satisfied | The repo's PR template and stated asks (checklist items, changelog or docs entry, issue link), read against the description and the diff | Every section the template or contribution docs require has real content, `Closes #<issue>` or the repo's equivalent is present, checkbox items are ticked only where true, and any file the repo says every PR must touch (a changelog or whatsnew entry) is in the diff. Fail if a required section is empty or skipped, or a required file is absent. If the repo states no template and no asks, this check passes. | required |
| AI use disclosed | The repo's stated AI policy, or the template's disclosure ask (for Path Review, the "Notes for Reviewers" section), read against the description | If the repo's facts or template require disclosure, the description contains the author's own-words statement of how AI was used. Fail if a required disclosure is absent or is only a placeholder. If the repo states no AI policy and no disclosure ask, this check passes. | required |
| Commits are readable | The commit list | Each commit message says what changed, and none is a placeholder ("wip", "fix", "misc cleanups"). | preferred |
| Description states what and why | The description's summary | The summary says what the change does and why, in terms a reviewer who has not seen the plan can follow. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Grade every check. The verdict is `accept` if and only if every `required` check is `pass`. Any `required` check graded `fail` makes the verdict `reject`. An `unclear` grade on a `required` check counts as `fail`, because a claim that cannot be verified from the package is not ready to submit. `preferred` checks are graded and reported, but never change the verdict.

A PR that honestly discloses a shortfall can be ready: a plan step that is deferred and stated as deferred (in the description or `## Deviations`) does not fail "Plan steps delivered or disclosed" or "Description matches the diff". A shortfall that is only discovered by comparing the diff with the plan, because nothing in the PR says so, fails. A short, plain PR that passes every required check is `accept`; length and polish are never a reason to reject or accept.

