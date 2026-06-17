# Draft Repo Memory

This repository follows the secure Zoolanding draft release workflow.

- Run `git pull --ff-only` before work when the worktree is clean.
- For multi-repo work, also pull the hub repo and every affected `draft-*` repo when clean.
- If the worktree is dirty, report it before pulling or changing files.
- Work on `dev`, promote with PR `dev -> test`, then PR `test -> main`.
- `dev` does not deploy.
- `test` deploys the test draft only after merge to `test`.
- `main` deploys production only after merge to `main`.
- Native GitHub branch protection should protect `test` and `main` when the account plan supports it. If GitHub blocks protection for private repos, the deploy workflow still rejects push-triggered deploys unless the commit is a merge from the expected source branch, but GitHub cannot block the push itself.
- Treat this repository as public unless verified otherwise. Before making it public, before PR, and before merge, run the hub repo public-safety audit and resolve every blocking finding.
- Do not commit secrets, tokens, API keys, signed URLs, `.env*`, local logs, PDFs/CVs, private keys, certificates, local databases, credential JSON, local agent state, `ai_notes/`, `findings/`, or `errors-reports/`.
- Public contact details in draft content are allowed only when they are intentionally client-facing; personal source files, CVs, private photos, identity documents, and raw research stay local-only.
- Deployment uses GitHub OIDC to assume AWS IAM roles split by repo and environment; do not add long-lived AWS access keys.
- 2026-06-09 01:36 CT: Optional auth pilot is plan-only for now. `site-config.json` may expose only public `runtime.authRemote` with `authProfileId: "staff"` and `/auth/runtime-config`; `server/auth-profile-registry.json` keeps Cognito policy server-only with `status: "planned"` and non-secret social IdP references. No real Cognito, IdP credentials, JWT tokens, client secrets, deploy, or AWS calls were created for this pilot.
- 2026-06-09 03:39 CT: `tools/deploy-draft.mjs` must keep `server/auth-profile-registry.json` classified as `server-auth-profile-registry` and `server/integrations.json` as `server-integrations`, with no `pageId`, so GitHub Actions publishes auth/integration policy only through server-only authoring kinds.
- 2026-06-10 01:45 CT: For the Zoosite `staff` auth pilot, Google and Facebook are optional per draft/auth profile and are disabled by omission. `server/auth-profile-registry.json` currently has no `socialIdpSecretRefs`, so Cognito provisioning should not require `/zoolanding/auth/zoosite/staff/google` or `/facebook` secrets. If social login is needed later, add only non-secret secret references here and load the actual IdP client secrets manually into AWS Secrets Manager outside the repo.
- 2026-06-10 02:10 CT: Zoosite `staff` auth is now active with Cognito-native login only. Runtime auth resolves to user pool `us-east-1_Pq5OCadbK`, public app client `16jb6ml9q5jdh6blj7f668fajp`, Hosted UI `https://zoosite-staff-planned.auth.us-east-1.amazoncognito.com`, and groups `zoosite-client` / `zoosite-admin`. Google/Facebook remain disabled by omission for this draft/profile; re-enable only by adding non-secret refs here and loading real IdP secrets outside the repo.
- 2026-06-17 02:18 CT: Zoosite `staff` auth uses `environmentClaim: "custom:zoolanding_env"` with `customAuth.signup.setEnvironmentClaim: true`. The browser must not send this value; the API proxy stack assigns `test` or `prod` server-side. Console-created users need this mutable Cognito custom attribute set before passing environment-scoped auth.
