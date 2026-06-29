# QA checklist - Zoosite blog product readiness

Fecha base: 2026-06-29 CT

Usa esta matriz para revisar el blog administrativo, blog publico y autenticacion relacionada de Zoositioweb. Cada bloque debe validarse en testing antes de promover a produccion. Para rutas visuales, revisa desktop y mobile. Para rutas protegidas, usa al menos un usuario administrador y un usuario cliente sin permisos admin.

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
- [ ] Roles esperados: `hub-admin`, `blog-admin`, `blog-editor`, `blog-publisher`, `blog-reviewer`, `blog-moderator`, `blog-analyst`.
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

- [ ] `/admin/blog/categorias` permite crear y listar categorias con labels traducidos, slug, descripcion SEO, visibilidad y advertencias de redirects.
- [ ] `/admin/blog/tags` permite crear y listar tags con labels traducidos, slug, descripcion SEO, visibilidad y advertencias de redirects.
- [ ] Los labels y ayudas explican cada campo en espanol e ingles.
- [ ] El editor de articulo usa dropdown para categoria con categorias disponibles.
- [ ] Tags se capturan por coma, se aplica trim, se deduplican y se pueden buscar/seleccionar desde tags existentes.
- [ ] Las paginas publicas pueden filtrar por categoria o tags sin perder `draftDomain`, `debugWorkspace` ni `lang`.

## Media lifecycle

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
- [ ] Categorias/tags tienen rutas y reglas de indexacion claras; combinaciones multi-tag no indexables por defecto salvo configuracion del draft.
- [ ] Cambios de slug generan o requieren redirects en lugar de romper URLs publicadas.
- [ ] Hreflang solo se genera para idiomas publicados reales.

## Analytics productization

- [ ] Public blog emits `blog_view` only for articulos publicados reales.
- [ ] Read depth, taxonomy filter, CTA click, reaction, share, asset download, comment intent y form outcome usan payloads sin PII.
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
