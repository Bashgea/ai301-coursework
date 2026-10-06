# Plan: make the /health postgres probe declare its SQL as text()

## Repro evidence (quoted from my repro comment on #61)

Environment
- Windows: Microsoft Windows [Version 10.0.26200.9457]
- Repo: my fork of codepath/pathreview-ai301-fa26-s3, main at commit 2f4e82f
- Python 3.13.14 (the project .venv was built from it), SQLAlchemy 2.1.1
- Docker 29.1.2, Compose v2.40.3-desktop.1, GNU Make 3.81, Git 2.52.0.windows.1
- Commands run in Git Bash, except copying .env, which I did in PowerShell

Steps (from the repo root)
1. Copy-Item .env.example .env   (in Git Bash: cp .env.example .env)
2. docker compose up -d
   Here the redis container could not publish port 6379 (another container on my machine held it) and vector-db exited. The db container started.
3. make setup failed at the alembic step with "password authentication failed for user pathreview". A native postgres.exe on my machine was also listening on port 5433, so my login never reached the project's database. Workaround: I created an untracked docker-compose.override.yml with
     services:
       db:
         ports: !override
           - "5434:5432"
   ran `docker compose up -d db`, and changed DATABASE_URL in .env to localhost:5434.
4. make setup again. Migrations 001 and 002 ran and the seed completed.
5. Terminal 1: .venv/Scripts/python -m uvicorn api.main:app --host 127.0.0.1 --port 8000
6. Terminal 2: curl -i http://127.0.0.1:8000/health

Observed
HTTP/1.1 503 Service Unavailable, body:
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-02T03:38:17.784569"}}

Server log for that request:
[error] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
[error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
The server's startup queries on the same database had succeeded a few seconds earlier (application_startup_completed).

Supporting check, a standalone script using an AsyncSession on settings.database_url (not the route itself):
raw string -> sqlalchemy.exc.ArgumentError : Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')
text() -> 1

Result
The postgres error in the log matches the one in the issue. I only sent one request, so I don't know how often it happens. The response also showed redis unhealthy, but that's a different error (the redis_host one in #62), not this issue. I haven't tried a fix.

## Diagnosis

The postgres probe in the /health route passes the plain string 'SELECT 1' to the database session. SQLAlchemy 2.x rejects a raw string and raises ArgumentError, the route catches the error, and it reports postgres as unhealthy even though the database is reachable.

This follows from my repro:
- The server log for the request says: "Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')".
- The database itself works: the startup queries on the same database succeeded seconds earlier (application_startup_completed).
- My standalone script, using an AsyncSession on settings.database_url, raised the same ArgumentError with the raw string and returned 1 with text().

I also read the route code: api/routes/health.py line 32 is `await db.execute("SELECT 1")`, a raw string, so the route does what my standalone script tested.

## Existing work on this issue

Three open PRs are linked to #61. #82 (by pakmultilinks-dot) changes the DB and Redis probes together and says "Fixes #61, fixes #62". #96 (by Bobaninja21) and #99 (by soccerthomas) both wrap the query in text(). My plan is a separate fix on my own fork that touches only the postgres probe. The redis failure stays out of scope because it is #62.

## Scope

In scope: the one postgres probe statement in the /health route (api/routes/health.py, line 32), and checking whether the mypy suppression for that file in pyproject.toml can be removed.

Not in scope:
- The redis failure ('Settings' object has no attribute 'redis_host'). That is a different error, tracked in #62.
- The vector_db check, which already reports healthy.
- My local setup workarounds (port 5434 override, edited .env). These are untracked and stay out of the change.

## Files to touch

- api/routes/health.py (line 32): wrap the probe's SQL in text() and import text from sqlalchemy if it isn't already imported.
- pyproject.toml (the mypy suppression for health.py, around lines 146-154): check whether it can be removed. CONTRIBUTING says that fixing a seeded bug removes its suppression, but this one is tied to #62 and "related #61", so it may have to stay until #62 is fixed.

## Approach

0. Work on a branch named fix/61-health-probe-text on my fork, as the house rules require (fix/61-<slug>).
1. Replace the raw string at api/routes/health.py:32 with text('SELECT 1') and add the import.
2. Restart the server and re-run my repro steps 5 and 6.
3. Run mypy and check whether the suppression in pyproject.toml can be removed or must stay because of #62.
4. No file under tests/ mentions health, so there is no existing test pattern for this route. I will add a test only if it can be done with a mocked session in a few lines. Otherwise I will rely on the repro re-run and say so in the PR.

## Test plan

Re-run my repro, steps 5 and 6, against the change:
- Terminal 1: .venv/Scripts/python -m uvicorn api.main:app --host 127.0.0.1 --port 8000
- Terminal 2: curl -i http://127.0.0.1:8000/health

Before the fix (observed): "postgres":"unhealthy", and the log shows postgres_health_check_failed with the 'SELECT 1' ArgumentError.

Expected after the fix:
- The response shows "postgres":"healthy".
- The log has no postgres_health_check_failed line.
- Redis still shows "unhealthy" with the redis_host error, so the overall status can still be 503 on my machine. That comes from #62 and is not a failure of this change.

I will also re-run the standalone script (raw string vs text()) to confirm the two results are unchanged.

## Risks and unknowns

- The mypy suppression may be tied to the redis error too, so I may not be able to remove it with only the postgres fix.
- The redis failure keeps the overall /health status at 503, so on my machine I can verify the postgres field but not a fully healthy response. That depends on #62.
- Three PRs already fix this issue, so mine may duplicate them. I'm building my own fix as part of the course.
- I have only reproduced this once, on Windows with SQLAlchemy 2.1.1. I haven't tested other SQLAlchemy versions.
- Whether a test is practical for this route is not yet known.

## Deviations

The core of the build matched the plan: one import and one wrapped query in api/routes/health.py. The query moved from line 32 to line 33 because the new import sits above it.

Two things differed from the plan:

- I added a unit test, tests/unit/test_health.py. My plan said I would add a test only if it could be done in a few lines. After reading docs/CONTRIBUTING.md, which says every code change should include or update relevant tests, I decided to add one. It fails against the old raw-string code (isinstance('SELECT 1', TextClause) is False) and passes with the fix.
- I did not change pyproject.toml. CONTRIBUTING says the health.py attr-defined suppression is issue #62, and mypy reports no issues with the suppression list unchanged, so there was nothing to remove for #61.

My posted plan comment said I would check whether the mypy suppression could be removed and that I would re-run my repro. Both are still true, so I did not post a follow-up comment.