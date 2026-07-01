# QA checklist - Zoosite blog product readiness

Fecha base: 2026-06-29 CT

Usa esta matriz para revisar el blog administrativo, blog publico y autenticacion relacionada de Zoositioweb. Cada bloque debe validarse en testing antes de promover a produccion. Para rutas visuales, revisa desktop y mobile. Para rutas protegidas, usa al menos un usuario administrador y un usuario cliente sin permisos admin.

## Estado local mas reciente

- 2026-06-30 17:08 CT: se agrego el plan operativo de cierre product-complete en 12 bloques. El ultimo corte local subido dejo `generic-file-dropzone.required`, guards de media/editor, `assetList` y `scheduleList` con `articleId` requerido. App commit `bdb406b`; draft commit `835ff21`.
- 2026-06-30 15:43 CT: se agrego cobertura directa de CLI para que el smoke de producto distinga errores locales de setup: falta `--runtime-base-url`, falta cookie autenticada o falta CSRF. Esto prepara la prueba manual sin imprimir cookies, tokens ni payloads sensibles.
- Verificacion local: `node --test tools/tests/content-hub-product-readiness-smoke.spec.mjs` retorno 21 pass; gates combinados de content-hub/admin/SSR retornaron 71 pass; `npm audit --omit=dev` retorno `found 0 vulnerabilities`; health de testing retorno PASS con bundle `main-XG4UPJHH.js` y content-hub sin sesion en `401`.
- 2026-06-29 18:45 CT: se reforzaron los guards de preview/SEO para que `articleId` y `revisionId` sean requeridos antes de ejecutar acciones de ciclo editorial. App commit `4bb03e8`; draft commit `00c065e`.
- Verificacion local: content-hub admin/schema/contract Node tests pasaron 54; focused `proxy-action.handlers.spec.ts` paso `TOTAL: 9 SUCCESS`; `npm run build` paso con el warning existente de `quill-delta`; `npm audit --omit=dev` retorno `found 0 vulnerabilities`; SSR rerun paso 23/23; public-safety audit retorno `ok:true`.
- Backend BFF: `origin/dev` de `zoolanding-content-hub` en `f9523ed` ya incluye `scheduleList`, `cancelSchedule` y auditoria persistida segura; validacion en worktree temporal paso 57 unittests, `sam validate` y `pip-audit`.
- Pendiente para cerrar producto: smoke autenticado real en testing y QA de navegador con usuario de blog.

## Product-complete closure plan - 12 blocks

El blog deja de ser MVP solo cuando estos 12 bloques tengan evidencia local, testing y produccion segun aplique. No basta que exista la configuracion; debe verse el comportamiento real.

| # | Bloque | Estado actual | Siguiente corte verificable |
|---|---|---|---|
| 1 | Smoke autenticado real | CLI listo; falta sesion real por ambiente. | Ejecutar create/edit/publish/runtime/search/feed/schedule/cancel en testing sin imprimir cookies. |
| 2 | Ciclo editorial | Crear, editar, preview, SEO, versiones y programacion existen. | Probar que crear paquete, editar rich text, guardar, validar y publicar no pierden datos. |
| 3 | Roles y permisos | Rutas admin tienen grupos por rol. | Verificar usuario admin/blog-role y usuario cliente sin permisos contra UI y BFF. |
| 4 | Rich text estable | `generic-rich-text` ya fue estabilizado para no resetear. | Browser QA con negrita, listas, encabezado, enlace, limpiar formato y recarga. |
| 5 | Modo avanzado/catalogo | Existe gating basico; catalogo visual completo sigue pendiente. | Definir corte minimo: catalogo solo visible con modo avanzado y mensaje claro si aun no esta disponible. |
| 6 | Taxonomia UX | Categorias/tags tienen pantallas y dropdowns. | Crear/listar categoria y tag reales; editor debe consumirlos sin texto tecnico. |
| 7 | Media lifecycle | Upload requiere articulo y archivo; no expone grants. | Probar upload real con fixture pequeno, alt/caption/licencia, asset list y estados vacios. |
| 8 | Programacion y versiones | Links y guards existen; `scheduleList` requiere articulo. | Programar, listar y cancelar publicacion futura; restaurar una revision con confirmacion. |
| 9 | Interacciones y moderacion | CTA/reaccion/share/comentario estan configurados. | Probar queue de comentario autenticado, moderacion y anti-spam/rate-limit sin PII. |
| 10 | SEO publico | SSR cubre sitemap/feed/search/articulo base. | Verificar SEO dinamico por articulo/categoria, canonical, JSON-LD, robots, hreflang real y 404. |
| 11 | Analytics producto | `blog_view` y panel agregado existen parcialmente. | Agregar/probar read depth, share, CTA, reaction, comment intent y asset download sin PII. |
| 12 | Operacion, release y regresion | Tests locales y public-safety estan definidos. | Ejecutar contratos, build, audit, desktop/mobile, auth regression, prod smoke y checklist final. |

## E2E route order for testing

1. Inicia sesion en `https://test.zoolandingpage.com.mx/acceso?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es` con un usuario que tenga permisos de blog.
2. Abre `/admin/blog/articulos?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; debe cargar tabla real, filtros, paginacion y acciones sin pantalla blanca.
3. Abre `/admin/blog/articulos/nuevo?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; crea un articulo con titulo unico, categoria, tags, resumen, SEO, slug y politicas.
4. Al crear, confirma que aparece `articleId`, `revisionId`, path o link de siguiente paso; no debe aparecer `Invalid id`.
5. Abre `/admin/blog/articulos/{articleId}/editor?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; edita titulo, categoria, tags, resumen y contenido enriquecido, guarda y recarga.
6. Abre `/admin/blog/articulos/{articleId}/preview?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; debe mostrar el articulo seleccionado, no la lista.
7. Abre `/admin/blog/articulos/{articleId}/seo?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; valida metadata y publica o prepara publicacion.
8. Abre `/admin/blog/articulos/{articleId}/versiones?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; debe listar versiones reales del articulo.
9. Abre `/admin/blog/programados?articleId={articleId}&draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; programa, lista y cancela una publicacion futura.
10. Abre `/blog/{categorySlug}/{articleSlug}?draftDomain=zoositioweb.com.mx&debugWorkspace=false&lang=es`; debe renderizar el articulo publicado y conservar SEO.
11. Abre `/content-hub-search.json?draftDomain=zoositioweb.com.mx&lang=es&q={articleSlug}`; debe incluir el articulo publicado.
12. Abre `/sitemap.xml?draftDomain=zoositioweb.com.mx` y `/feed.xml?draftDomain=zoositioweb.com.mx&lang=es`; deben incluir solo articulos publicos indexables.

## Redacted live smoke

- [ ] Con sesion real de testing, ejecutar desde el repo app sin guardar cookies en archivos versionados:

```powershell
$env:ZLP_CONTENT_HUB_SMOKE_COOKIE = "<cookie header temporal de testing>"
npm run content-hub:smoke -- --base-url=https://test.zoolandingpage.com.mx --runtime-base-url=https://y84vk0v44l.execute-api.us-east-1.amazonaws.com/Prod --environment=test --domain=zoositioweb.com.mx
```

- [ ] El smoke debe devolver `ok: true` y checks `createArticle`, `upsertCategory`, `upsertTag`, `taxonomyCategoryList`, `taxonomyTagList`, `uploadAsset`, `updatePackage`, `revisionList`, `publicBundlePreview`, `restoreRevision`, `assetList`, `moderationQueue`, `validate`, `submitReview`, `approveArticle`, `publish`, `recordInteractionReadProgress`, `recordInteractionCta`, `recordInteractionReaction`, `recordInteractionShare`, `recordInteractionAssetDownload`, `recordInteractionForm`, `queueComment`, `moderationQueueAfterComment`, `moderateComment`, `moderationQueueAfterModeration`, `publicInteractionAnalytics`, `runtimeBundle`, `publicSearch`, `publicArticleHtml`, `publicArticleBody`, `sitemap`, `feed`, `scheduleList`, `cancelSchedule`, `unpublishArticle`, `articleDetailAfterUnpublish`, `publicSearchAfterUnpublish`, `publicArticleAfterUnpublish`, `sitemapAfterUnpublish` y `feedAfterUnpublish`.
- [ ] No pegar cookies, CSRF, passwords, tokens ni headers completos en chats, notas, commits o PRs.

## Editorial lifecycle

- [ ] `/admin/blog/articulos/nuevo` crea un paquete editable con titulo, idioma, categoria, tags, resumen, SEO, slug, politica de comentarios, politica de contenido y visibilidad.
- [ ] Crear paquete muestra loading, exito o error entendible; nunca muestra `Invalid id` sin explicar que falta.
- [ ] Al crear un articulo, la pantalla redirige o muestra el `articleId` real y permite abrir editor, preview, SEO, versiones y programacion.
- [ ] `/admin/blog/articulos/:id/editor` carga titulo, idioma, categoria, tags, resumen, slug, contenido enriquecido, medios vinculados y estado real del articulo.
- [ ] Guardar cambios conserva el contenido enriquecido y la metadata despues de recargar.
- [ ] Validar, enviar a revision, publicar, despublicar, archivar o restaurar revision muestran estado de carga, resultado y siguiente paso.
- [ ] Las acciones no duplican submits y no dejan botones activos durante una operacion en curso.

## Blog roles and permissions

- [ ] `/admin/blog*` requiere sesion y rol administrativo.
- [ ] Un usuario `zoosite-client` no puede abrir rutas admin ni ejecutar mutaciones del content-hub.
- [ ] Roles esperados: `hub-admin`, `blog-admin`, `blog-editor`, `blog-publisher`, `blog-reviewer`, `blog-moderator`, `blog-media-manager`, `blog-analyst`.
- [ ] Controles visibles coinciden con el rol, pero la autorizacion real se verifica en backend.
- [ ] Errores de permisos muestran mensaje entendible sin filtrar grupos internos, claims, tokens ni politica server-only.

## Rich text and component builder stability

- [ ] `generic-rich-text` permite escribir sin regresar el cursor al inicio.
- [ ] Negritas, cursivas, subrayado, encabezados, listas, citas, enlaces y limpiar formato funcionan segun toolbar configurada.
- [ ] El contenido no se resetea cuando cambian valores de query, data source o estado de guardado.
- [ ] El output configurado se conserva como Delta JSON, texto o formato permitido sin tratar HTML como fuente autoritativa.
- [ ] El modo basico no muestra controles avanzados innecesarios.
- [ ] El modo avanzado se puede activar/desactivar y explica que permite editar componentes, presets y JSON seguros.

## Visual component catalog and advanced mode

- [ ] El catalogo visual de componentes aparece solo cuando el modo avanzado esta activo y el preset del draft lo permite.
- [ ] Cada componente disponible muestra nombre, descripcion, preview y campos configurables.
- [ ] El editor JSON valida sintaxis continuamente y bloquea guardado si el JSON no es valido.
- [ ] El catalogo respeta allowlist por draft y nunca permite scripts, event handlers crudos, signed URLs, secretos o politica server-only.
- [ ] Los errores de configuracion indican que campo debe corregirse.

## Taxonomy product UX

Current evidence:
- Product-readiness smoke now creates and lists a category/tag pair before article creation.

- [ ] `/admin/blog/categorias` permite crear y listar categorias con labels traducidos, slug, descripcion SEO, visibilidad y advertencias de redirects.
- [ ] `/admin/blog/tags` permite crear y listar tags con labels traducidos, slug, descripcion SEO, visibilidad y advertencias de redirects.
- [ ] Los labels y ayudas explican cada campo en espanol e ingles.
- [ ] El editor de articulo usa dropdown para categoria con categorias disponibles.
- [ ] Tags se capturan por coma, se aplica trim, se deduplican y se pueden buscar/seleccionar desde tags existentes.
- [ ] Las paginas publicas pueden filtrar por categoria o tags sin perder `draftDomain`, `debugWorkspace` ni `lang`.

## Media lifecycle

Current evidence:
- Product-readiness smoke now registers a small public-safe asset through `uploadAsset`, verifies `assetList`, and fails if asset responses expose internal storage metadata.

- [ ] `/admin/blog/medios` permite seleccionar archivos y muestra progreso o estado mientras se registra metadata.
- [ ] Cada asset permite revisar alt text, caption, licencia, focal point, tipo, tamano, uso y publicabilidad.
- [ ] Imagenes usadas en articulos tienen `alt` antes de publicar.
- [ ] El flujo no expone URLs firmadas, buckets, grants, secretos, paths privados ni datos internos.
- [ ] Estados vacios explican si no hay medios, si falta `articleId`, o si el backend no regreso registros.
- [ ] Archivar/eliminar se muestra como intencion segura y depende de backend.

## Scheduling and revision history

- [ ] `/admin/blog/programados` recibe `articleId` por query string o muestra una explicacion clara para seleccionar articulo.
- [ ] `publishAt` y `unpublishAt` usan controles de fecha/hora entendibles, con zona horaria visible.
- [ ] Programar, publicar ahora, reprogramar y cancelar programacion muestran loading, exito y errores claros.
- [ ] `/admin/blog/articulos/:id/versiones` lista revisiones, snapshots/deltas, autor, fecha y estado.
- [ ] Comparar y restaurar revision requieren confirmacion y permisos.
- [ ] Una publicacion programada apunta a una revision inmutable validada.

## Public interactions and moderation

Current evidence:
- Public article page has draft-configured CTA, reaction, share, and comment-intent wiring to content-hub actions.
- Comment UI includes a sign-in link and moderation copy.
- Product-readiness smoke now queues a moderated comment, verifies it in `moderationQueue`, approves it with `moderateComment`, verifies the moderated row, and fails if moderation responses expose internal hashes or moderator identity.
- Still requires live authenticated/API proof for backend spam/rate-limit behavior.

- [ ] Comentarios requieren login cuando el articulo lo configura.
- [ ] Comentarios nuevos entran a cola de moderacion por defecto cuando el draft lo define.
- [ ] Likes, reacciones, CTAs, shares y formularios publicos tienen proteccion anti-spam/rate-limit.
- [ ] `/admin/blog/moderacion` permite aprobar, rechazar y archivar con razon/auditoria.
- [ ] Analytics no recibe correos, nombres, telefonos, texto de comentarios ni cuerpos de formularios.
- [ ] Estados vacios explican que no hay comentarios/interacciones pendientes.

## SEO product completion

- [ ] `/blog` lista articulos publicados con previews publicas, categorias y tags seguros.
- [ ] `/blog/:categorySlug/:articleSlug` muestra el articulo correcto y devuelve 404 para slugs no publicados.
- [ ] Canonical, `BlogPosting` JSON-LD, robots, titulo, descripcion y social metadata corresponden al articulo actual.
- [ ] `/sitemap.xml`, `/feed.xml` y `/content-hub-search.json` incluyen articulos publicados y excluyen borradores, previews, admin y no-publicos.
- [ ] Despues de retirar o despublicar un articulo, `/content-hub-search.json`, la ruta publica del articulo, `/sitemap.xml` y `/feed.xml` dejan de mostrarlo.
- [ ] Categorias/tags tienen rutas y reglas de indexacion claras; combinaciones multi-tag no indexables por defecto salvo configuracion del draft.
- [ ] Cambios de slug generan o requieren redirects en lugar de romper URLs publicadas.
- [ ] Hreflang solo se genera para idiomas publicados reales.

## Analytics productization

- [ ] Public blog emits `blog_view` only for articulos publicados reales.
- [ ] Read depth, taxonomy filter, CTA click, reaction, share, asset download, comment intent y form outcome usan payloads sin PII.
- [ ] El smoke autenticado prueba aceptacion y agregacion de read depth, CTA, reaction, share, asset download, form outcome y comentario moderado; aun se debe validar que los componentes publicos del draft emitan esos eventos desde UI real donde aplique.
- [ ] `/admin/blog/analiticas` muestra metricas agregadas, estados vacios y filtros entendibles.
- [ ] El panel no muestra eventos crudos con comentarios, correos, nombres o telefonos.
- [ ] Los filtros no rompen query strings ni el draft actual.

## Operations, observability, and audit

- [ ] Cada mutacion protegida registra auditoria segura con request ID, actor, accion, target y decision.
- [ ] Errores del backend usan codigos estables y mensajes localizables: `auth_required`, `forbidden`, `validation_error`, `not_found`, `conflict`, `rate_limited`, `upstream_unavailable`, `internal_error`.
- [ ] UI muestra request ID cuando ayuda a soporte sin exponer detalles internos.
- [ ] No se exponen tokens, cookies, CSRF, signed URLs, Cognito sessions, buckets, tablas, lambdas ni policy internals.
- [ ] Existe runbook de smoke para testing y produccion.

## Full QA, release, and product readiness

- [ ] Ejecutar contratos locales: `node --test tools/tests/content-hub-contract-harness.spec.mjs tools/tests/content-hub-schema.spec.mjs tools/tests/content-hub-admin-pages.spec.mjs tools/tests/zoosite-content-hub-admin-bindings.spec.mjs`.
- [ ] Ejecutar SSR local: `node --test tools/tests/ssr-server.spec.mjs`.
- [ ] Ejecutar build: `npm run build`.
- [ ] Ejecutar auditoria de dependencias: `npm audit --omit=dev`.
- [ ] Ejecutar public-safety audit: `npm run drafts:public-safety-audit -- --repo=drafts\zoositioweb.com.mx --history=true`.
- [ ] Ejecutar smoke autenticado de producto y verificar que incluya `publicSearchAfterUnpublish`, `publicArticleAfterUnpublish`, `sitemapAfterUnpublish` y `feedAfterUnpublish`.
- [ ] Testing desktop y mobile: `/blog`, articulo publico, `/admin/blog`, articulos, nuevo, editor, preview, SEO, versiones, programados, categorias, tags, medios, moderacion, analiticas, hub y configuracion.
- [ ] Produccion desktop y mobile despues de deploy con usuario admin real y usuario cliente sin permisos.
- [ ] Confirmar que no quedan listeners locales, logs crudos, temporales, reportes sin revisar ni secretos en cambios.

## Access and authentication regression

- [ ] `/acceso` muestra formulario, errores entendibles y estado de carga al iniciar sesion.
- [ ] `/registro` valida contrasena, confirmacion, simbolo, longitud y estados disabled.
- [ ] `/confirmar-cuenta`, `/recuperar-contrasena`, `/cambiar-contrasena` redirigen al flujo correcto despues de exito.
- [ ] `/mi-cuenta` requiere sesion y muestra datos del usuario sin requerir rol admin.
- [ ] `/mi-cuenta` muestra activar MFA solo si el usuario no tiene MFA activo.
- [ ] `/mi-cuenta` muestra desactivar MFA solo si el usuario tiene MFA activo.

## Responsive and visual regression

- [ ] Desktop 1365x900: no hay overflow horizontal, textos encimados ni botones demasiado pequenos.
- [ ] Mobile 390x844: inputs, selects, dropdowns, botones y paginacion tienen minimo tactil razonable.
- [ ] Dropdowns se ven consistentes con inputs y botones.
- [ ] Paginadores, tablas y barras de filtros mantienen alineacion en mobile.
- [ ] No aparece `[object Object]`, `Invalid id`, IDs internos editables o abreviaciones sin explicar en ningun texto visible.
- [ ] Los cambios de pantalla muestran carga cuando la transicion pueda tardar.
