# Auth and protected-runtime contract

Use this document for login, account, MFA, and admin-user work. site-config.json and server/auth-profile-registry.json remain the executable contract.

## Boundaries

- Browser configuration may expose only public runtime identifiers and same-origin routes.
- Cognito policy, allowed token use, environment rules, integrations, and secrets stay server-side.
- server/auth-profile-registry.json and server/integrations.json must publish only as server-only authoring kinds, without page IDs.
- Google and Facebook are disabled by omission. Add only non-secret secret references to source control; load actual IdP secrets outside the repository.
- Never place JWTs, cookies, CSRF values, signed URLs, credentials, or real user data in draft JSON, notes, logs, or QA evidence.

## Session and routing

- Protected account and admin operations use the same-origin auth-admin BFF with HttpOnly session cookie, readable CSRF cookie plus request header, tenant/domain context, and environment checks.
- Keep auth routing path-specific. The runtime-config path belongs to the API proxy and the OAuth callback belongs to the draft app; never route a broad auth wildcard to the BFF.
- Account navigation may use only sanitized mapped account state globally. Admin-user lists and mutations remain page-scoped and server-authorized.
- Shared preview login must preserve the draftDomain parameter through OAuth without widening the allowlist of return parameters.

## Environment and roles

- The staff auth profile is Cognito-native. Google and Facebook remain optional.
- The custom environment claim is assigned by the backend, never by the browser.
- Multi-environment access through list mode is an explicit operational grant for selected accounts; normal registration uses the current environment.
- Self-registered or console-created users need an approved client/admin group and the correct environment claim before protected routes work.

## UX rules

- Login, registration, confirmation, password recovery/reset, logout, MFA, and admin mutations expose loading state and block repeat submits.
- Account MFA UI renders enrollment, disablement, or an explicit unknown-state fallback from mapped public fields.
- Challenge errors distinguish environment mismatch from expired challenge and show clear Spanish recovery instructions.
- Admin-user UI remains configuration-driven, but every mutation is rechecked by the BFF with current user state and authorization.

## Release gate

Test at least one permitted user and one authenticated user without admin permission. Verify session, CSRF, tenant/environment isolation, logout, and protected-route denial before production promotion.
