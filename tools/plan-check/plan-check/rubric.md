# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis fits the repro | Plan's Diagnosis compared line by line with the Repro evidence section | Names one specific cause, and every measurement in the repro agrees with it. Fail if any repro result contradicts the cause, if the cause is only repeated from the thread or a commenter without being checked against the repro, or if the plan names no cause. | required |
| Scope targets the cause | Plan's Scope, Changes, and files to touch, compared with the Diagnosis | Names each file it will change, says what it won't touch, and every change addresses the diagnosed cause. Fail if the changes would not fix the cause, or if the plan's out-of-scope list names the place where the cause lives. | required |
| Test plan can show the fix worked | Plan's Test plan, compared with the Repro evidence steps | Re-runs the repro steps and states the expected result after the fix (a number, output, or behavior), or adds an automated test that would fail before the fix. Fail if it only says "verify it works." | required |
| Comment fits the thread | Candidate plan comment, compared with the Issue and Thread highlights | Acknowledges each existing PR, duplicate, or maintainer statement in the thread that bears on the plan, and does not state a non-maintainer's claim as fact. Passes if nothing in the thread bears on the plan. Fail if it ignores one of those or states such a claim as fact. | required |
| Comment follows repo conventions | Candidate plan comment, compared with the Repo facts block | Meets each rule Repo facts state for contributors, such as AI-use disclosure or a required template. Passes if Repo facts state no rule that applies to a plan comment. Fail if the comment breaks one. | required |
| Plan names risks and unknowns | Plan's risks section | Lists at least one real risk or open question, not filler. | preferred |

## Verdict rule

Accept (ready) only if every required check is graded pass. Reject (hold) if any required check is graded fail or unclear. An unclear grade (written `?`) on a required check counts as a fail, because missing evidence is a reason to ask for a revision, not to approve. Preferred checks never change the verdict.


## Verdict rule

Accept (ready) only if every required check is graded pass. Reject (hold) if any required check is graded fail or unclear. An unclear grade (written `?`) on a required check counts as a fail, because missing evidence is a reason to ask for a revision, not to approve. Preferred checks never change the verdict.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
