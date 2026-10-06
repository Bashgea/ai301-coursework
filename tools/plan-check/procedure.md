# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Use only the text of the package (in live mode, the drafts and the evidence they quote). Do not fetch anything and do not use outside knowledge of the project.
2. Read the Repro evidence section first, all the way through, before looking at the plan. It holds measured results, so it is the most trustworthy part of the package. Every later check compares something against it, which is why it comes first.
3. Read the plan's Diagnosis, Scope, Changes (or files to touch), Test plan, and Risks sections, in that order. While reading, note the one cause the Diagnosis names and where the plan says that cause came from.
4. Read the candidate plan comment. Note every claim it makes and every promise about what the plan does.
5. Read the Issue and its thread, then the Repo facts (contribution policy, bug report template, latest release). Note who said each thing and their role (for example COLLABORATOR or NONE).

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. Repro evidence: write one line per measurement or observed result, with the exact numbers or output. Next to each line, write what it implies about the cause (for example, "still slow with the pager removed, so the pager is not the cause"). Also write the exact repro steps and the expected result.
2. Diagnosis: copy the plan's stated cause in one sentence. Record whether the plan says it came from its own testing, from the repro, or from a person in the thread.
3. Thread claims: for each claim the plan relies on, record who made it and their role. Check each one against your repro list. If no repro line supports it, mark it unverified.
4. Scope: list the files the plan says it will change and the things it says it will not touch. Record whether the place the cause lives appears on either list.
5. Test plan: copy the steps and the expected result. Record whether the steps re-run the repro steps and whether a number, output, or behavior is named.
6. Thread: list any existing pull request, duplicate, or maintainer statement in the thread that bears on the plan, and any claim by a non-maintainer. Record whether the comment acknowledges each one or states the claim as fact.
7. Conventions: list each rule in Repo facts that applies to a plan comment (AI-use policy, required template, other contributor asks). Record whether the comment meets each one.
8. Risks: copy each risk or unknown the plan lists.
9. If a part of the package is missing or empty, write "missing" next to it. Do not fill the gap from your own knowledge of the project.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks one at a time in the order they appear in the rubric. Use only the evidence you gathered, not your own opinion of what the best fix would be.
2. Grade each check pass (every part of the pass condition is met), fail (any part of the fail condition is met), or unclear (the evidence needed to decide is missing from the package).
3. Diagnosis: compare the plan's cause with every line in your repro list. If any line contradicts the cause, grade fail and quote that line. If the cause fits the repro but was only repeated from the thread, grade fail unless the plan itself points to a repro line that supports it.
4. Scope: grade fail if the changes would not fix the stated cause, or if the plan's out-of-scope list names the place where the cause lives.
5. Test plan: grade fail if it only says to verify the fix works without naming steps and an expected result.
6. Comment fits the thread: grade fail if the comment ignores an existing PR, duplicate, or maintainer statement you listed, or states a non-maintainer's claim as fact without support. If nothing in the thread bears on the plan, grade pass.
7. Comment follows repo conventions: grade fail if the comment breaks a rule you listed from Repo facts. If Repo facts state no applicable rule, grade pass.
8. Risks: grade pass if at least one real risk or unknown is listed. If the section is missing or empty, or only filler, grade fail. This check never changes the verdict.
9. Never grade pass because something sounds reasonable. If you cannot point to the line that proves the pass condition, grade unclear.
10. A check may be graded from your gathered notes without re-reading the whole package, but quote the line you relied on. If your notes do not contain it, go back to that section only.
11. If this procedure is silent on something you need to decide, say so in the summary instead of inventing a step.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Read the grades you wrote. Find every required check graded fail or unclear.
2. If any required check is fail or unclear, the verdict is reject. If every required check is pass, the verdict is accept. Preferred checks never change the verdict.
3. Write one sentence naming the check or checks that decided the verdict. Quote the line from the package that decided it (the repro line, the plan line, or the comment line).
4. End the output with one fenced JSON block, last in the output, containing the item id, one entry per check (name, grade of pass, fail, or unclear, and a one-line evidence quote), and the verdict accept or reject. A short readable summary may come before it.