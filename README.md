# Zoosite draft

<!-- zoolanding-hub-routing:start -->
## Zoolanding Knowledge Router

Shared procedures are routed through the Zoolandingpage hub. Start with [AGENTS.md](AGENTS.md) and open only the document needed for the current task.

| Task | Read |
| --- | --- |
| Edit draft content or routes | Local `site-config.json`, page JSON, and task-specific local docs |
| Create or bootstrap a draft | [ai-notes/how-to/create-secure-draft-repo.md](https://github.com/LynxPardelle/zoolandingpage/blob/main/ai-notes/how-to/create-secure-draft-repo.md) |
| Promote, deploy, or configure branches | [Hub lifecycle guide and local `.github/workflows/`](https://github.com/LynxPardelle/zoolandingpage/blob/main/docs/11-draft-lifecycle.md) |
| Upload public assets | [docs/12-public-assets-and-file-uploads.md](https://github.com/LynxPardelle/zoolandingpage/blob/main/docs/12-public-assets-and-file-uploads.md) |
| Configure domains or aliases | [docs/13-managed-alias-front-door.md](https://github.com/LynxPardelle/zoolandingpage/blob/main/docs/13-managed-alias-front-door.md) |
| Work across repositories | [docs/repository-map.md](https://github.com/LynxPardelle/zoolandingpage/blob/main/docs/repository-map.md) |

Critical repository-specific safety, deployment, and rollback rules remain local.
<!-- zoolanding-hub-routing:end -->

Sanitized source for zoositioweb.com.mx, authored as a Zoolanding draft and promoted through dev -> test -> main.

## Start here

- [AGENTS.md](./AGENTS.md): workflow, read order, and safety rules.
- [docs/README.md](./docs/README.md): task-based current documentation.
- [qa/admin-blog-qa-checklist.md](./qa/admin-blog-qa-checklist.md): blog product-readiness QA.
- [changelog/README.md](./changelog/README.md): chronological implementation history.

The runtime contract lives in the JSON package. Local-only notes, findings, logs, environment files, private keys, databases, PDFs, and CV/photo source folders remain ignored.
