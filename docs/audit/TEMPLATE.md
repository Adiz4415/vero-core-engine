# CLOSURE_VERIFICATION_TEMPLATE

> Use this template to audit any closed issue whose fix landed via a single PR. The structure is lifted from `dashboard/CLOSURE_VERIFICATION_ISSUES_36_37_38.md` (verified by the docs PR #82) and adapted to per-module contexts. **Never claim a local test run you didn't perform** — when local tooling isn't available (e.g. `cargo` in the dashboard audit environment), document the closing PR's CI status honestly.

## Required sections

1. **Header block** — verdict, branch / commit under audit, audit date, raw validation evidence (command + output line).
2. **Per-issue AC table** — at minimum three columns:
   - **Criterion** — verbatim text lifted from the issue body.
   - **Implementation** — file:line anchor + a one-line behavioural summary.
   - **Locked-in test** — the exact test `it(…)` / `#[test]` name as it exists in the repo. No paraphrasing.
3. **Verdict line** — `✅ #NN closure confirmed.` or `❌ #NN closure blocked — see gaps.`
4. **Non-blocking follow-ups** — call out anything that should exist but doesn't (tests, visual-only assertions). Treat these as evidence-quality improvements, not closure gaps.
5. **References** — closing PR + closing-PR merge commit hash + any prior commits in the chain.

## Validation evidence

Always list actual command output, not a paraphrase:

```
✓ vitest         N/N passing across F test files (X.XX s)
✓ tsc --noEmit   clean
✓ eslint         clean (--max-warnings 0)
✓ cargo test     N/N passing on merge commit <hash>
                 (or: "CI-pass on merge commit <hash>; local cargo unavailable in audit env")
```

### When local tooling can't reproduce the run

If your audit environment lacks `cargo` / `node_modules` / etc., copy the closing PR's merge commit hash and reference the CI summary line from GitHub. Example:

> *CI status: validated by remote CI run on commit `<merge commit oid>` — see PR's "Checks" pane. Local `cargo` unavailable in this audit environment.*

Never silently imply a local run. Auditors trust the trail **because** it is honest about provenance.

## Per-implementation cross-check workflow

For every row in the AC table:

1. Open the cited file and confirm the cited line range is real (don't paraphrase file paths).
2. Open the cited test file and copy the test name verbatim — wrap in backticks.
3. Note any fidelity markers — `vi.useFakeTimers()`, `vi.mock(...)`, `env.mock_all_auths()`, `#[should_panic]`. They tell reviewers what assumptions the test bakes in.
4. If a row maps to "review by inspection" rather than a unit test, flag it honestly in the **Non-blocking follow-ups** section.

## Reviewers' three bars

Every audit must satisfy:

- **Accuracy** — every test name, file path, and behaviour cited matches the source.
- **Completeness** — every acceptance criterion from the issue body maps to ≥1 implementation reference.
- **Verifiability** — the claimed validation numbers map to either a real local run or a real CI run on the cited commit hash. If an "as-of" caveat applies (e.g. test count could shift after the audit), say so.
