# Alas de Señas — publicación por capas

Esta es una entrega parcial del proyecto. Conserva las rutas de
`alas-de-senas.code-workspace`: al abrir ese archivo en VS Code aparecen
las capas con sus nombres originales.

`main` comienza con la estructura. Cada capa se incorpora mediante su propio
pull request, que debe revisar y aceptar el propietario. Los pull requests
son independientes y pueden aceptarse en cualquier orden.

| Vista del workspace | Ruta | Contenido de esta entrega |
|---|---|---|
| Frontend (React) | `apps/web` | Vacía, solo `.gitkeep` |
| API — rutas HTTP | `apps/api/app/api` | Vacía, solo `.gitkeep` |
| API — formatos JSON | `apps/api/app/schemas` | Vacía, solo `.gitkeep` |
| Backend — reglas de negocio | `apps/api/app/domain` | Vacía, solo `.gitkeep` |
| Backend — servicios | `apps/api/app/services` | Vacía, solo `.gitkeep` |
| Base de datos — modelos y repositorios | `apps/api/app/infra` | PR `feat/base-de-datos` |
| Base de datos — migraciones | `apps/api/alembic` | PR `feat/base-de-datos` |
| Contenido (señas y lecciones) | `content` | PR `feat/contenido` |
| Entrenamiento del modelo | `ml` | PR `feat/entrenamiento-modelo` |
| Documentación | `docs` | PR `docs/documentacion` |
| Proyecto completo (comandos) | `.` | PR `chore/proyecto-comandos` |

La capa de base de datos incluye `apps/api/alembic.ini`, `docker/` y
`docker-compose.yml`. La capa de comandos incluye configuración, herramientas,
CLI, soporte compartido y pruebas originales; respeta las cinco carpetas vacías.

Se incluyen los videos originales disponibles en `content/videos/raw` y los
datasets locales de `ml/data`. Sus atribuciones y licencias se conservan en
`content/datasets.csv` (LSC-54: CC BY-NC 4.0; LSC70: CC BY 4.0).
Se excluyen secretos `.env`, entornos virtuales, dependencias instaladas,
cachés y archivos compilados.

**Esta entrega no ejecuta la aplicación completa:** faltan intencionalmente
frontend, rutas HTTP, formatos JSON, dominio y servicios. Sus pruebas de
integración tampoco pueden ejecutarse hasta incorporar esas capas.
La CI conserva la comprobación de disponibilidad antes de ejecutar las pruebas.

El README original se conserva en `docs/README-proyecto-completo.md` como
referencia del proyecto completo; describe también funciones aún no publicadas
en esta entrega. La aplicación original permanece en
[alas-de-senas](https://github.com/rcamilorodriguez1/alas-de-senas).
