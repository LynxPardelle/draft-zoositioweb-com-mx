# Zoosite Agent Workflow

Use this file as the repository entrypoint for AI agents and contributors.

## Read order

1. Read [README.md](./README.md).
2. Open only the task-specific document from [docs/README.md](./docs/README.md).
3. Read [qa/admin-blog-qa-checklist.md](./qa/admin-blog-qa-checklist.md) only for blog release or authenticated QA work.
4. Read [changelog/](./changelog/) only when implementation history is relevant.

Codex.md is a compatibility pointer, not a changelog or a required context dump.

## Workflow

- Pull with "git pull --ff-only" only when the worktree is clean; report dirty worktrees before changing them.
- Work on dev, promote through PRs dev -> test -> main; dev does not deploy.
- Treat the repository as public. Before PR and merge, run the hub public-safety audit with history enabled and resolve every blocking finding.
- For rendered changes, verify every affected route in desktop and mobile on the shared test preview before production promotion.
- Runtime JSON is the executable source of truth. If documentation disagrees with it, verify behavior and update the current contract.
- Put durable current rules in docs/, chronological implementation evidence in changelog/, and local investigation in ignored ai_notes/, findings/, or errors-reports/.

## Security

- Never commit credentials, tokens, signed URLs, private source documents, local databases, logs, or raw customer/user data.
- Keep authentication policy and integration secrets server-side. Browser draft configuration may contain only explicitly public runtime identifiers and routes.
- Use GitHub OIDC and environment-scoped IAM roles; do not add long-lived AWS access keys.
