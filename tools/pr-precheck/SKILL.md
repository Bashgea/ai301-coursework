---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (title,
description, commits, diff, test evidence) read against the plan it
claims to implement and the issue that plan belongs to. You do not
answer from gut feel. You answer by executing the rubric in
`rubric.md`, check by check, against evidence you gather from the
package itself. Grade one package per run, and never answer a
different question.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

One of:

- **Live mode**: the student's own PR, checked before it goes out.
  Inputs:
  - `plan.md`, including everything under `## Deviations`
  - the diff on the branch, from `git diff main...HEAD` (three dots),
    run from the working copy. Only committed changes count; say so if
    you see uncommitted ones.
  - the draft PR title and description in `pr_draft.md` (title on the
    first line, the filled-in template under it)
  - the test evidence in `test_evidence.md`
  - the issue and its thread, read from the real repo
  - the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md`, read from the real repo
  A house-chain student reads the house plan and the house repro pack
  instead of their own; the same checks grade the same things. If a
  named input is missing, grade the checks that need it `unclear` and
  name the missing file in the evidence line. Never guess its contents.
  Read the draft the way a reviewer on the pull request will read it:
  the package is what the draft and the diff contain and quote, not
  other files in the student's working directory. A deviation from
  the plan is honest only if `plan.md` records it under
  `## Deviations`; a deviation that exists only in the diff is not.
  Grade it that way.
- **Eval mode**: a package bundle is the whole world. Use ONLY the
  bundle text as evidence. Do not fetch anything and do not read
  anything else. Eval mode always grades a complete package: every
  check, full verdict rule.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` in this skill directory before anything
else. It names the repo the student's PR must target and the house
rules that apply there; apply those rules when you read the evidence.
Refuse to grade a PR for any repo other than the one on the `Repo:`
line. If the `Repo:` line still carries an unfilled placeholder, stop
without grading and tell the student to fill in the `Repo:` line in
`scope.md` with their section's Path Review repo; never guess a
scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the outgoing PR text, the title and the
description in `pr_draft.md`, against those rules. Report every rule
the draft breaks in the summary, quoting the rule and the offending
words. Keep voice notes separate from the check grades. The voice
guide never changes the verdict on its own; only a rubric check that
reads it can. In eval mode, ignore `voice-guide.md` entirely: voice
is personal and carries no gold labels.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read `rubric.md`. It defines a table of checks (each with its
evidence, pass condition, and weight: `required` checks gate the
verdict; `preferred` checks never change it) and a verdict rule that
says how check grades combine into accept or reject.
`references/evidence-guide.md` is the rubric's map: where each kind
of evidence lives in a PR package, and what good looks like there.

Then **execute `procedure.md`**. It is the skill's operating
procedure, written by the student: it decides the read order, how
each evidence family gets gathered, how a check executes against
gathered evidence, and how grades become the verdict. Follow it as
written, the same way you follow the rubric, without improvising
around gaps. Most checks compare two things side by side, such as the
diff against the plan, the test evidence against the plan's test
plan, or the description against the diff. Where the procedure is
silent, note the gap in your summary rather than silently inventing a
step or a check; a procedure gap is feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no
steps filled in, stop and say which one is empty: this skill cannot
grade without a rubric AND a procedure, and that is by design. The
rubric, the evidence guide, and the procedure are the parts the
student writes; the voice guide carries over from week 2.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict, no "accept with reservations", and no
score. Reservations belong in check evidence lines, not in the
verdict. Apply the rubric's verdict rule to the check grades to set
the verdict.

Before the JSON block you may show a short readable summary: a line
per check, plus any voice-guide notes and procedure gaps in live
mode. End your reply with the fenced JSON block below. It must be
valid and last, with nothing after it. The eval harness parses the
last fenced JSON block in your output.

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

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the PR, not the polish: a terse complete PR can be ready and
  a long confident one can hide drift or be untested. Every check
  reads the thing itself against the plan, the issue, and the stated
  standards, never the formatting.
- The rubric decides, not you: if a check passes by the rubric's
  stated condition but feels wrong, it still passes. Note the
  tension in the evidence line if you want; the fix belongs in the
  rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  does not say, treat `unclear` as `fail`: a PR you cannot verify
  from the package is a PR that is not ready to submit.
