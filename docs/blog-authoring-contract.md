# Blog authoring contract

Use this document for public blog and protected content-hub work. The detailed release matrix remains in qa/admin-blog-qa-checklist.md.

## API and data boundaries

- Protected reads and actions use the same-origin /features/content-hub BFF paths. Do not restore the stale /content-hub base path.
- Public discovery comes from the safe published runtime article and taxonomy index.
- Article body rendering uses the published articleContent value; summary remains metadata and introduction.
- Never expose package keys, signed URLs, grants, storage policy, credentials, or server-only fields in draft config.

## Editor and navigation

- Open editor, preview, SEO, and versions from a selected article row or an explicit article route ID.
- The admin overview contains row-scoped navigation, not global validate or publish actions.
- Mutations use interaction-scope snapshots and explicit table event fields. Required IDs must block actions before a request.
- Advanced component JSON stays within the editor scope, is parsed only in advanced mode, and is limited to allowed generic component types without events.
- The current advanced preview uses the generic component preview, not a static imitation.

## Roles and UI

- Blog editor, publisher, media, moderator, and analyst are distinct product roles in addition to admin.
- Moderation and analytics routes enforce their matching roles server-side.
- Row actions remain usable touch targets; current draft styling uses 52px inline actions.
- Media actions require article context plus a file and may display only safe returned asset identifiers and filenames.
- Category and tag tables use taxonomy IDs internally and human labels publicly.

## Public behavior

- Public article, category, tag, search, feed, sitemap, canonical, structured data, language, and 404 behavior must be verified against the published bundle.
- Public share controls perform the browser share/copy action before analytics.
- Analytics must avoid PII and cover only approved product interactions.

## Completion gate

Local tests alone do not prove product readiness. Complete the authenticated testing smoke and desktop/mobile browser QA in qa/admin-blog-qa-checklist.md before promotion.
