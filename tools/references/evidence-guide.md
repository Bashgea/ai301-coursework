# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives, in a practice package:** the plan is in the
"Plan context" section: its numbered steps, the "Files" line, the
"Not in scope" line, and the repro evidence it built on. What the PR
actually does is in the "Candidate PR" section: the "Diff" (changed
files and hunks) and the "Description" (what the PR says it does).
Practice packages carry no deviation notes unless the plan context
states one.

**Where it lives, in my own PR:** the plan and its `## Deviations`
notes are in `plan.md`. The change is the output of
`git diff main...HEAD` (committed changes only). The claims about it
are in `pr_draft.md` (the Summary and Changes sections).

**What good looks like:** every changed file and hunk falls inside
the plan's stated files and steps or a recorded deviation note;
every plan step is in the diff or is stated as deferred; and every
claim in the description ("exactly as planned", "docs updated", "no
changes outside X") is true of the diff. Silent drift is anything
the diff adds that the plan scopes out or never mentions (a new flag,
a config option, a rewrite of unrelated code), or a description that
claims more than the diff delivers. A disclosed shortfall (a step
named as deferred in the description or `## Deviations`) is honest,
not drift.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives, in a practice package:** the proof is the "Test
evidence" section of the "Candidate PR". Read it against the
plan's repro (in "Plan context") and the plan's test plan. The
repo's own checks are named in the "Repo facts" block (the PR
template asks).

**Where it lives, in my own PR:** `test_evidence.md` (the
before/after repro output and the results of `make test-unit`,
`make test-integration`, `make lint`, `make typecheck`), read against
the test plan in `plan.md`. The ticked boxes under "Testing" in
`pr_draft.md` must match that output.

**What good looks like:** the plan's own repro appears with the same
command or input, shows a before output and an after output, and goes
through the code the diff changes. Each item in the plan's test plan
has evidence or a stated reason it was not run. Each repo check
shows real output; an honest failure or "no tests ran" counts as
shown. "Tested locally", "tests pass", or a run of a control case or
a different input proves nothing.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives, in a practice package:** the "Diff" and "Commits"
subsections of the "Candidate PR". Judge the diff's added and
removed lines, not the commit messages.

**Where it lives, in my own PR:** `git diff main...HEAD` for the
lines, and `git diff main...HEAD --stat` for the list of files
changed, which should list only the files my fix needs. The commit
messages come from `git log main..HEAD --oneline`.

**What good looks like:** the fix is visible and nothing unrelated
rides along. Debris tells: debug prints or logging, commented-out
code, dead or unused functions, leftover TODO or experiment notes,
whitespace, formatting, or import-reorder churn, and hunks for files
the plan never mentions. Placeholder commit messages ("wip", "fix",
"misc cleanups") are worth noting but are not debris in the diff.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives, in a practice package:** the asks are in the "Repo
facts" block (the PR template sections, the contribution policy, any
stated AI policy) and in the "Thread highlights" when a maintainer
gives direction. The PR's answer is in the "Description" and in
the "Diff" (for a required file such as a changelog entry).

**Where it lives, in my own PR:** the repo's
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` hold
the asks. My answer is `pr_draft.md`: every template section filled,
`Closes #<issue>` present, and my own-words AI disclosure under
"Notes for Reviewers".

**What good looks like:** every section the template or contribution
docs require has real content, the issue link is present, boxes are
ticked only where true, any required file is in the diff, and a
required AI disclosure is present in the author's own words. When
the repo states no template, no asks, and no AI policy, nothing is
required and this family passes. Boilerplate left unfilled, a skipped
section, or a missing disclosure the repo requires is a standards
wall. (Whether the description's claims match the diff belongs under
Plan fidelity, not here.)
