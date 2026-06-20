# /docs/audit — Issue closure audit trail

> One Markdown file per closed-via-single-PR issue, mapping acceptance criterion → implementation → locked-in test. Auditors and reviewers read these instead of digging through finished PR diffs.

## Index

| Issue | Title | Closing PR | Merge commit | Closure doc |
|-------|-------|-----------|--------------|-------------|
| #14 | feat: emergency recovery module | #72 | `4634a73` | [`ISSUE_14.md`](./ISSUE_14.md) |
| #41 | feat: implement finite state machine for governance proposal states | #50 | `fbe3763` | [`ISSUE_41.md`](./ISSUE_41.md) |
| #46 | feat: governance treasury vote | #74 | `c8286c2` | [`ISSUE_46.md`](./ISSUE_46.md) |

## Source of truth for dashboard-side closures

- `dashboard/CLOSURE_VERIFICATION_ISSUES_36_37_38.md` — issues #36, #37, #38 closed by PR #70; verified by the docs PR #82.

## Adding a new audit doc

1. Copy [`TEMPLATE.md`](./TEMPLATE.md) into `ISSUE_<NN>.md` next to this `README.md`.
2. `gh issue view NN --repo Vero-protocol/vero-core-engine` — copy the acceptance-criteria block verbatim into the AC table.
3. `gh pr view <closing PR> --json number,title,mergedAt,mergeCommit,additions,deletions,changedFiles` — capture the merge commit hash for the validation-evidence section.
4. Cite file:line anchors and full test names without paraphrasing.
5. If a test count shifts in a follow-up PR, add an "as-of" caveat to the validation evidence — don't pretend the numbers are frozen forever.
6. Update this README index with the new row.

## When to file a follow-up issue

Anything you flag in **Non-blocking follow-ups** is also worth a GitHub issue so it doesn't get lost. Cross-link from the closure doc to the issue so the audit trail stays navigable in both directions.
