# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The environment record in the repro report (OS, runtime and tool versions, repo commit or version, how it was set up) | Someone else could rebuild the same environment from what is written. Missing or vague versions that plausibly affect the bug (e.g. "latest Node") fail. | required |
| steps-rerunnable | The commands and actions in the repro report, from clone to the point where the behavior appears | Every command or action needed to reach the behavior is present, so a stranger would not have to guess anything. Judge whether it could be re-run, not how many steps there are. | required |
| behavior-matches-issue | The commands and output excerpt in the repro report, read against the behavior and trigger the issue describes | The artifact is aimed at the issue's own behavior. Either it shows the symptom the issue reports, or, in a cannot-reproduce, it shows an attempt at the issue's described trigger and what was observed instead. It fails if it shows a different or adjacent behavior or error than the issue's, exercises a different scenario than the one the issue describes, or shows no artifact at all. | required |
| outcome-honest | The stated result in the repro report, read against the artifacts it shows | The stated outcome matches the evidence. An evidenced "cannot reproduce" passes. Claiming reproduction with no artifact, or with an artifact showing something else, fails. Claims about root cause, frequency, or how many people are affected that no artifact shows also fail. | required |
| conventions-respected | The claim comment and repro report, read against the contribution policy and bug-report template asks in the repo-facts block | First, AI disclosure: if the repo-facts block states a rule requiring disclosure of AI assistance (for example "disclose all AI usage in any form"), the comments must contain an explicit disclosure statement. Absence of a statement is the failure: do not infer that no AI was used because none is mentioned. If the rule applies to the kind of writing in the package and there is no statement, this fails regardless of anything else in the report. A policy limited to pull requests or code, with no ask for issue comments, does not trigger this. Second, template asks: the check also fails when the report omits something maintainers need, such as a required reproduction link, config or code example; logs the template requires; or confirmation on the latest release or main when required (testing an older version fails). Information given inline in the report's own words instead of pasted tool output (versions and platform instead of `conda info`) counts as supplied. If no stated policy or template ask applies, it passes. | required |
| claim-specific | The claim comment, read against the issue context | The claim names this issue's specifics and promises only investigation and a report, with no fix and no date. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept if every required check passes. Preferred checks never change the verdict. An `unclear` on a required check counts as fail.
