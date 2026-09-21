# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

## Selected issue

### Issue link

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

### Verdict output

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
  "checks": [
    {"name": "Maintainer responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) commented on issue #43 2026-09-16, within 90 days"},
    {"name": "Repo actively maintained", "grade": "pass", "evidence": "Commit 2f4e82f dated 2026-09-16, within 60 days"},
    {"name": "Issue scoped for beginner", "grade": "pass", "evidence": "Labels include 'good first issue'"},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: [], comments: none"},
    {"name": "Clear acceptance criteria", "grade": "pass", "evidence": "Body includes exact SQLAlchemy ArgumentError text and repro steps"}
  ],
  "verdict": "accept"
}
```

## Eval iterations

### Run history

agreement: 13/20 scored items (final run)

### Issue analysis

Issue: issue-01

My rubric verdict: reject

Gold label: accept

Reasoning: My rubric requires "Issue is scoped for beginner" to pass, which checks if the issue has a "good-first-issue" label OR has a clear description under 500 characters mentioning 1-2 files. Issue-01 had no label and its description was 593 characters, so my rubric rejected it. However, the gold label accepted it. This shows my rubric was too strict on the character limit — a well-written 593-char description can still be beginner-friendly if it's clear and bounded in scope.

### Check rationale

From my rubric.md:

"| Issue is scoped for beginner | Issue body description, labels, and scope | Has a clear description of what needs to be done; task scope is bounded (not open-ended); a newcomer could understand the work without deep codebase knowledge | required |"

Reasoning: I included this check because first issues need to be manageable for newcomers. I focused on clarity and bounded scope rather than strict character counts, because what matters is whether a beginner can understand the work, not whether it fits an arbitrary length limit. This check attempts to catch issues that are too vague or open-ended.

### Trade-offs

This check still rejects issues that might actually be beginner-friendly but happen to have longer descriptions. For example, issue-01 has a detailed description over 500 characters, but the detail makes the scope clear rather than confusing. By rejecting based on length alone, my rubric misses nuance — sometimes a longer explanation is clearer than a short one. A better approach might weight the presence of "good-first-issue" label more heavily or look at whether the description has clear steps rather than just counting characters.

## Selection rationale

1. **Fit to interests and time:** Issue #61 is about fixing a SQLAlchemy database validation bug. I want to learn how databases and SQL work in Python, and this issue has clear reproduction steps and a specific error message to fix. The scope looks manageable for the time available.

2. **What the verdict identified correctly:** My rubric correctly found that the repo is actively maintained (recent commits), a maintainer has been responsive (recent comments), the issue has the "good-first-issue" label, nobody has claimed it, and it has explicit acceptance criteria (exact error text + reproduction command). What my rubric couldn't weigh is whether I have the background to understand SQLAlchemy or whether this particular bug aligns with my learning goals — that's a judgment call I made, not something the rubric can see.

3. **Anticipated difficulty:** Claiming the issue should be straightforward — just a comment on GitHub. The technical work is moderate: I'll need to understand how SQLAlchemy's `text()` function works and why raw SQL strings need wrapping, which means reading some code but probably not a huge portion of the codebase.