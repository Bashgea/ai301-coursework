# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
**Where it lives:**
- Eval bundle: the repro report's environment record (OS, runtime and tool versions, repo commit or release, setup method). Compare it against the issue context, which states the OS and version the reporter hit, and the repo-facts block, which names the latest release.
- Live mode: the environment section of the draft repro report. Issue side: the issue body and the repo's README or setup docs.

**What good looks like:** The OS and the versions that plausibly affect the bug are named exactly, not as "latest" or "my machine". They match what the issue targets, or the difference is called out. A stranger could rebuild the same setup from the text alone.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

**Where it lives:**
- Eval bundle: the commands and actions in the repro report, from the starting state to the point where the behavior appears.
- Live mode: the steps section of the draft report, plus the repo's setup docs for anything it assumes.

**What good looks like:** The artifact targets the issue's own scenario. It shows either the issue's symptom or, in a cannot-reproduce, an honest attempt at the issue's trigger and what happened instead. An adjacent behavior, a different scenario, or no artifact does not count.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

**Where it lives:**
- Eval bundle: output excerpts, logs, or screenshot text inside the repro report. Read them against the symptom in the issue context.
- Live mode: pasted output or screenshots in the draft report. Issue side: the behavior in the issue body and any maintainer comments in the thread.

**What good looks like:** The artifact itself exhibits the symptom the issue describes, such as the same error message or the same visible failure. A related error, or a description with no artifact, does not count as showing it. If no artifact is present at all, the behavior is not shown.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

**Where it lives:**
- Eval bundle: the stated outcome in the repro report, set next to the artifacts it shows.
- Live mode: the outcome statement in the draft report, set next to the output it pastes.

**What good looks like:** The stated outcome is exactly what the artifact supports. An evidenced "could not reproduce", with the environment and steps recorded, is honest and ready. A confident "reproduced" with no artifact, or an artifact showing something else, is not. Claims about root cause, how often it happens, or how many people are affected are unsupported unless an artifact shows them.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

**Where it lives:**
- Eval bundle: the claim comment and the repro report, read against the repo-facts block (bug-report template fields and the contribution policy, including any AI-use disclosure rule) and the issue context.
- Live mode: the draft comments, read against the repo's CONTRIBUTING.md, its issue template, and any AI policy file. Also read the issue thread for existing claims.

**What good looks like:** The comments follow every stated policy. If the repo requires disclosing AI assistance, the comments disclose it. Template asks count when something maintainers need is missing (a repro link, required config or logs, confirmation on the required version). The same versions and platform written inline instead of pasted tool output is fine. A claim names this issue's specifics and promises only investigation and a report, with no fix and no date. Boilerplate like "+1, I'll take this" is not specific.
