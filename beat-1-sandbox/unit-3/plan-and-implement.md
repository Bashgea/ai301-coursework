# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

## Posted upstream

### GitHub username

Bashgea

### Plan comment

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-6010522500

I'd like to work on this. My repro shows the postgres probe fails with the SQLAlchemy error "Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" even though the database is reachable. The startup queries on the same database succeeded a few seconds earlier. I checked the code: api/routes/health.py line 32 runs db.execute("SELECT 1") with a plain string, which matches the error.

I see three open PRs already linked to this issue. #82 changes the DB and Redis probes together ("Fixes #61, fixes #62"), and #96 and #99 both wrap the query in text(). My plan is a separate fix on my fork. It changes only the postgres probe in api/routes/health.py, so I'm leaving the redis check (#62) and the vector_db check alone. I'll also check whether the mypy suppression for health.py in pyproject.toml can be removed. CONTRIBUTING says fixing a seeded bug removes its suppression, but that entry is tied to #62 first, and rueiliu noted in the thread that mypy still fires on the Redis code, so it may have to stay until #62 is fixed.

To test, I'll re-run my repro (start the server, curl /health) and expect "postgres":"healthy" with no postgres_health_check_failed in the log. On my machine, redis will still show unhealthy because of #62, so the overall status may stay 503. I haven't tried the fix yet.

## Your branch

### Branch

fix/61-health-probe-text

### Evidence

Issue #61, /health postgres probe.

BEFORE (from my repro comment on #61, main at 2f4e82f)

Commands (Git Bash, from the repo root):

```
.venv/Scripts/python -m uvicorn api.main:app --host 127.0.0.1 --port 8000
curl -i http://127.0.0.1:8000/health
```

Output:

```
HTTP/1.1 503 Service Unavailable, body:
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-02T03:38:17.784569"}}

[error] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
[error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
```

AFTER (branch fix/61-health-probe-text, 2026-10-06)

Commands (PowerShell, from the repo root):

```
.venv\Scripts\python -m uvicorn api.main:app --host 127.0.0.1 --port 8000
C:\Windows\System32\curl.exe -i http://127.0.0.1:8000/health
```

Output:

```
HTTP/1.1 503 Service Unavailable
date: Tue, 06 Oct 2026 06:29:32 GMT
server: uvicorn
content-length: 182
content-type: application/json
vary: Origin
x-request-id: a9c81934-b84b-47df-a3b0-c81ad2e11450

{"detail":{"status":"unhealthy","dependencies":{"postgres":"healthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-06T06:29:33.263574"}}

2026-10-06 01:28:16 [info     ] application_startup_completed 
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
2026-10-06 01:29:33,267 INFO sqlalchemy.engine.Engine BEGIN (implicit)
2026-10-06 01:29:33,268 INFO sqlalchemy.engine.Engine SELECT 1
2026-10-06 01:29:33,268 INFO sqlalchemy.engine.Engine [generated in 0.00040s] ()
2026-10-06 01:29:33 [debug    ] postgres_health_check_passed   request_id=a9c81934-b84b-47df-a3b0-c81ad2e11450
2026-10-06 01:29:34 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=a9c81934-b84b-47df-a3b0-c81ad2e11450
2026-10-06 01:29:34 [debug    ] vector_db_health_check_passed  request_id=a9c81934-b84b-47df-a3b0-c81ad2e11450
2026-10-06 01:29:34,409 INFO sqlalchemy.engine.Engine ROLLBACK
INFO:     127.0.0.1:62611 - "GET /health HTTP/1.1" 503 Service Unavailable
```

Result: postgres changed from unhealthy to healthy and the ArgumentError is gone. Redis is still unhealthy because of #62, so the overall status stays 503, as the plan expected.

New unit test, tests/unit/test_health.py. Without my fix (git stash push api/routes/health.py):

```
.venv\Scripts\python -m pytest tests/unit/test_health.py -q
FAILED tests/unit/test_health.py::TestHealthCheck::test_postgres_healthy_and_probe_uses_text_clause - AssertionError: assert False
E        +  where False = isinstance('SELECT 1', TextClause)
1 failed, 2 warnings in 1.70s
```

With my fix (after git stash pop):

```
.venv\Scripts\python -m pytest tests/unit/test_health.py -v
tests/unit/test_health.py::TestHealthCheck::test_postgres_healthy_and_probe_uses_text_clause PASSED [100%]
1 passed, 2 warnings in 1.37s
```

Repo checks (Git Bash, branch fix/61-health-probe-text):

```
make lint
All checks passed!

make typecheck
Success: no issues found in 76 source files

make test-unit
tests/unit/test_health.py::TestHealthCheck::test_postgres_healthy_and_probe_uses_text_clause PASSED [ 15%]
================ 376 passed, 53 xfailed, 2 warnings in 15.78s =================
```

## Eval iterations

### Run history

1. Run 1: 18/19 scored. pkg-02 errored (Windows UnicodeEncodeError), so the run was not saved.
2. Run 2: 17/20, saved, below the bar. Missed pkg-02, pkg-14, pkg-20.
3. Run 3: partial re-grade with --only pkg-02,pkg-14,pkg-20,pkg-04,pkg-01,pkg-06: 6/6, after splitting the comment check into two rows and syncing rubric.md, procedure.md and evidence-guide.md.
4. Run 4: full run, 19/20, every category matched. This matches the line in eval-run.txt: "agreement: 19/20 scored items  (bar: 18/20: PASS)".

### Package analysis

I picked pkg-02 (sharkdp/bat#3844, a width-1 "capacity overflow" panic). In my final full run my rubric said reject, failing only "Comment fits the thread". The gold label is accept. The other five checks passed.

Why the gold label says accept: the plan's diagnosis is a usize underflow in the background fill, and both controls in the repro agree with it. The package says "Control run, width 2 (equal to the glyph's display width)" exits 0, and "Second control: same width-1 command without `--highlight-line` (no background painted) exits 0." The thread has nothing to acknowledge: "Thread highlights (0 comments total)" followed by "(no comments)". Repo facts say "no stated AI policy".

Why my rubric rejected it: the eval-run.txt note only names the failed check, so this is my reading. The comment says "the underflow is the background-fill subtraction the issue points at". That repeats the claim of the issue's author (leeewee, NONE, a non-maintainer). The check reads "does not state a non-maintainer's claim as fact", and it has no exception for claims the repro supports. But both controls support this claim, so the comment asserts nothing unverified. My evidence-guide.md Comms section has the exception: "treats a non-maintainer's claim as a lead rather than a fact unless the repro supports it." The rubric row and the guide disagree. That also explains the instability: pkg-02 passed in Run 3 and failed in Run 4 with the same files.

### Check rationale

Quoted from the rubric.md in tools/plan-check/:

| Comment fits the thread | Candidate plan comment, compared with the Issue and Thread highlights | Acknowledges each existing PR, duplicate, or maintainer statement in the thread that bears on the plan, and does not state a non-maintainer's claim as fact. Passes if nothing in the thread bears on the plan. Fail if it ignores one of those or states such a claim as fact. | required |

Why it reads that way: it used to be one row, "Comment is honest about the thread and conventions", doing two unrelated jobs, and my Run 2 failures on pkg-02 and pkg-20 did not say which job had failed. Last week's feedback described the same problem with conventions-respected (two reasons to fail in one rule), so I split it into this thread row and a separate "Comment follows repo conventions" row. It is required because a plan comment that ignores an open PR sends maintainers wrong information. My own plan comment failed this check twice on the live thread (PRs #82 and #96, then #99), and the skill caught it each time. "Passes if nothing in the thread bears on the plan" stops a quiet thread from failing a comment for ignoring things that are not there.

### Trade-offs

The check gives up three things.

1. Judgment. "Bears on the plan" and "a non-maintainer's claim" are not mechanical, so borderline packages can flip. pkg-02 did, between Run 3 and Run 4. I accept this miss: I am at 19/20 with every category matched, and I did not edit rubric.md after the confirming run because it would make eval-run.txt no longer match the uploaded files.
2. Spec drift. The row omits "unless the repro supports it", which evidence-guide.md has. The fix would be to add that exception to the row, then re-grade pkg-02 with canaries pkg-04 and pkg-20 (the other thread-convention packages) and pkg-01 (wrong-cause), since loosening the check could flip them.
3. Quiet threads pass easily. With no comments, there is little for this check to fail.

Canaries I re-ran with --only after splitting the check: pkg-04 (thread-convention), pkg-06 (scope-creep) and pkg-01 (wrong-cause). All three stayed correct, and pkg-20 flipped from accept to reject, matching gold.

Gaps in my other files that I did not fix, for the same reason: procedure.md Read order step 1 says "Do not fetch anything", which conflicts with live mode, and no rubric row grades the ## Deviations section even though SKILL.md says a deviation is graded.
