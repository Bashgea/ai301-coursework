# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/256

**Branch**

fix/61-health-probe-text

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Four runs graded, in order (earlier launch attempts crashed before grading, so they have no score):

1. Full run: `agreement: 18/19 scored items`. pkg-05 errored (a Windows encoding error, `claude exited 1`) and was not scored; pkg-08 was graded reject against gold accept, failing "Repo's own checks run".
2. `--only pkg-08,pkg-05,pkg-01,pkg-20,pkg-04`: `agreement: 4/5 scored items`. pkg-08 now agreed; pkg-05 was graded reject against gold accept, failing the same check.
3. `--only pkg-05,pkg-08,pkg-04,pkg-07,pkg-10,pkg-14`: `agreement: 6/6 scored items`.
4. Full run saved to eval-run.txt: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

pkg-05 (category clear-accept, gold label: accept). The repo's contribution guide names `cargo fmt`, `clippy` and `cargo test`, and the test evidence says: "`cargo test -p nu-protocol` passes (312 tests); fmt and clippy clean." In run 2 my rubric graded it reject, failing "Repo's own checks run", because the row then demanded each named command's "real output" and "fmt and clippy clean" has no pasted output behind it. The gold label says accept, because the PR is complete and well evidenced. My rubric now accepts it because the row asks that the evidence "names the command or suite and states a concrete result (a pass count, "clean", "no diff", the output lines, or an honest failure or "no tests ran")". "Clean" is a stated result for a named command, so the check passes.

**Check rationale**

The check, exactly as it reads in `tools/pr-precheck/rubric.md`:

```
| Repo's own checks run | The commands or suites named in the repo's PR template or contribution docs (for Path Review: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`), and the template's checklist statements, read against the test evidence | If the repo names commands or suites to run, the evidence shows each one run: it names the command or suite and states a concrete result (a pass count, "clean", "no diff", the output lines, or an honest failure or "no tests ran"). Fail if a named command or suite is never mentioned in the evidence, or is covered only by a generic claim that names no command and no result ("tests pass", "tested locally"). Template checklist items that are statements rather than commands (code formatted, text internationalised, docs updated) pass when the description states them and nothing in the diff or evidence contradicts them; fail only if the diff or evidence contradicts a claimed item. If the repo names no checks, this check passes. | required |
```

It reads this way because I revised it twice after run 1. The first version failed a PR "if the description ticks a box for a check with no output behind it". That wrongly rejected pkg-08, because its template has statement items like "code formatted" that are not commands. So I split commands from checklist statements: statements pass unless the diff or evidence contradicts them. The second version still demanded each command's "real output", which wrongly rejected pkg-05's "fmt and clippy clean". So I changed it to "names the command or suite and states a concrete result". I rejected two alternatives: dropping the check altogether, and accepting any claim that checks passed. A generic "tests pass" with no command named still fails.

**Trade-offs**

Loosening this row gives up proof of output. A PR that writes "fmt and clippy clean" without having run them now passes this check, and I accept that my rubric will miss that case, because only the author knows whether they ran it. To make sure the loosened row did not let bad PRs through, I re-ran canaries with `--only`: the four not-tested packages (pkg-04, pkg-07, pkg-10, pkg-14) still graded reject, matching gold, and pkg-01 and pkg-20 (standards-wall) held in run 2. The final full run then agreed on all 20 packages. I changed only this row in `rubric.md` and the matching bullet in `procedure.md`, and changed no other check.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
