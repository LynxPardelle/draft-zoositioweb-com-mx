# Checklist QA - Blog y autenticacion de Zoositioweb

Fecha base: 2026-06-24 CT

Usa esta lista para revisar las pantallas del blog administrativo, blog publico y autenticacion de Zoositioweb. Marca cada punto en testing antes de promover cambios a produccion.

## Acceso y permisos

- [ ] `/acceso` muestra formulario, errores entendibles y estado de carga al iniciar sesion.
- [ ] `/registro` valida contrasena, confirmacion, simbolo, longitud y estados disabled.
- [ ] `/confirmar-cuenta`, `/recuperar-contrasena`, `/cambiar-contrasena` redirigen al flujo correcto despues de exito.
- [ ] `/mi-cuenta` requiere sesion y muestra datos del usuario sin requerir rol admin.
- [ ] `/mi-cuenta` muestra activar MFA solo si el usuario no tiene MFA activo.
- [ ] `/mi-cuenta` muestra desactivar MFA solo si el usuario tiene MFA activo.
- [ ] `/admin/*` requiere sesion y rol administrador.
- [ ] Un usuario `zoosite-client` no puede abrir rutas admin.
- [ ] Los mensajes de error no filtran tokens, claims internos, ids privados o errores crudos del backend.

## Articulos - listado admin

- [ ] `/admin/blog/articulos` carga sin errores visibles ni consola con errores relevantes.
- [ ] El buscador conserva `q` en query string y no descuadra el layout.
- [ ] El filtro de estado conserva `status` en query string.
- [ ] El selector de tamano conserva `pageSize` en query string.
- [ ] La tabla muestra columnas utiles en desktop sin desbordes horizontales innecesarios.
- [ ] La vista mobile permite leer filas y acciones sin texto encimado.
- [ ] La paginacion esta visible debajo de la tabla, incluso cuando solo hay una pagina.
- [ ] La paginacion muestra resumen, anterior, siguiente y pagina activa con estados disabled claros.
- [ ] No hay paginadores duplicados.
- [ ] Las acciones de fila conservan `articleId`, `draftDomain`, `debugWorkspace` y `lang`.

## Articulos - creacion y edicion

- [ ] `/admin/blog/articulos/nuevo` separa claramente metadata, contenido, SEO y publicacion.
- [ ] Los campos requeridos bloquean guardado/publicacion cuando faltan datos.
- [ ] Los estados disabled se ven disabled y usan cursor correcto.
- [ ] El editor permite crear contenido con componentes genericos autorizados.
- [ ] El modo avanzado es opcional y no estorba al flujo basico.
- [ ] Los errores de guardado explican la accion que debe tomar el usuario.
- [ ] Los estados de carga aparecen durante guardado, publicacion, preview y navegacion.
- [ ] La programacion de publicacion valida fecha/hora y zona esperada.
- [ ] Versiones/deltas se crean cuando corresponde y no duplican contenido completo sin necesidad.

## SEO y publicacion

- [ ] El slug se valida y no permite caracteres peligrosos.
- [ ] El titulo SEO, descripcion, canonical y social image tienen validacion visible.
- [ ] La preview publica usa el dominio y contexto del draft actual.
- [ ] El articulo canonico puede adaptarse al dominio donde se renderiza.
- [ ] Categorias y tags soportan multiples tags y query strings de listado.
- [ ] Categorias/tags pueden ocultarse o renombrarse por draft cuando aplique.
- [ ] El contenido publico no muestra controles administrativos.
- [ ] SSR/render publico expone metadata suficiente para SEO.

## Medios y archivos

- [ ] El panel de medios carga listado, estados vacios y errores sin romper la pagina.
- [ ] Subida de archivos muestra progreso o estado de carga.
- [ ] Cada archivo permite revisar alt, caption, tipo, tamano y uso esperado.
- [ ] No se exponen URLs firmadas, secretos, buckets privados o datos internos.
- [ ] Imagenes publicas tienen `alt` cuando se usan en contenido.

## Moderacion e interacciones

- [ ] Comentarios requieren usuario autenticado cuando el articulo asi lo configura.
- [ ] Comentarios nuevos entran a cola de moderacion por defecto si el draft lo define.
- [ ] Likes, reacciones, CTAs y formularios publicos tienen proteccion anti-spam.
- [ ] El panel de moderacion permite aprobar, rechazar y revisar contexto sin datos sensibles.

## Analiticas

- [ ] Las paginas publicas del blog disparan eventos de analitica esperados.
- [ ] El panel de analiticas carga sin requerir datos mock si el backend responde.
- [ ] Estados vacios explican que aun no hay datos.
- [ ] Los filtros de analitica no rompen query strings ni el draft actual.

## UI/UX responsive

- [ ] Desktop 1365x900: no hay overflow horizontal, textos encimados ni botones demasiado pequenos.
- [ ] Mobile 390x844: inputs, selects, dropdowns, botones y paginacion tienen minimo tactil razonable.
- [ ] Dropdowns se ven consistentes con inputs y botones.
- [ ] Paginadores, tablas y barras de filtros mantienen alineacion en mobile.
- [ ] No aparece `[object Object]` en ningun texto visible.
- [ ] Los cambios de pantalla muestran carga cuando la transicion pueda tardar.

## Seguridad y configuracion por draft

- [ ] Todo comportamiento sensible depende de configuracion del draft o backend server-only.
- [ ] No hay material sensible ni valores de acceso en JSON publico.
- [ ] Los endpoints protegidos validan JWT en backend.
- [ ] El draft conserva `draftDomain`, idioma y entorno correcto en navegacion.
- [ ] Las opciones avanzadas de HTML/Markdown estan limitadas por allowlist del draft.
- [ ] La configuracion multi-hub solo consume hubs autorizados.

## Produccion

- [ ] Testing fue validado con usuario admin real antes de promover.
- [ ] Produccion fue validado despues del deploy con login real.
- [ ] Se hizo prueba mobile y desktop en produccion para rutas criticas.
- [ ] Se confirmo que no quedan listeners locales, archivos temporales, logs o reportes sin revisar.
