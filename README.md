# Alas de Señas — publicación por capas

Explora el proyecto con los mismos nombres y en el mismo orden que en VS Code.
Despliega una capa y selecciona **Abrir carpeta** para ver sus archivos.

<details open>
<summary><strong>alas-de-senas (Workspace)</strong></summary>

<details>
<summary>1 · Frontend (React)</summary>

[Abrir carpeta →](apps/web/) · `apps/web`

Reservada para una próxima entrega; contiene únicamente `.gitkeep`.

</details>

<details>
<summary>2 · API — rutas HTTP</summary>

[Abrir carpeta →](apps/api/app/api/) · `apps/api/app/api`

Reservada para una próxima entrega; contiene únicamente `.gitkeep`.

</details>

<details>
<summary>2 · API — formatos JSON</summary>

[Abrir carpeta →](apps/api/app/schemas/) · `apps/api/app/schemas`

Reservada para una próxima entrega; contiene únicamente `.gitkeep`.

</details>

<details>
<summary>3 · Backend — reglas de negocio</summary>

[Abrir carpeta →](apps/api/app/domain/) · `apps/api/app/domain`

Reservada para una próxima entrega; contiene únicamente `.gitkeep`.

</details>

<details>
<summary>3 · Backend — servicios</summary>

[Abrir carpeta →](apps/api/app/services/) · `apps/api/app/services`

Reservada para una próxima entrega; contiene únicamente `.gitkeep`.

</details>

<details>
<summary>4 · Base de datos — modelos y repositorios</summary>

[Abrir carpeta →](apps/api/app/infra/) · `apps/api/app/infra`

Tablas y consultas de la aplicación. [Ver entrega de la capa (PR #1)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/1).

</details>

<details>
<summary>4 · Base de datos — migraciones</summary>

[Abrir carpeta →](apps/api/alembic/) · `apps/api/alembic`

Evolución del esquema de PostgreSQL. [Ver entrega de la capa (PR #1)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/1).

</details>

<details>
<summary>Contenido (señas y lecciones)</summary>

[Abrir carpeta →](content/) · `content`

Señas, currículo y videos. [Ver entrega de la capa (PR #2)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/2).

</details>

<details>
<summary>Entrenamiento del modelo</summary>

[Abrir carpeta →](ml/) · `ml`

Scripts, notebooks y datasets. [Ver entrega de la capa (PR #3)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/3).

</details>

<details>
<summary>Documentación</summary>

[Abrir carpeta →](docs/) · `docs`

Especificaciones, decisiones y guías. [Ver entrega de la capa (PR #4)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/4).

</details>

<details>
<summary>Proyecto completo (comandos)</summary>

[Abrir carpeta →](./) · Raíz del repositorio

Configuración, comandos y soporte del proyecto. [Ver entrega de la capa (PR #5)](https://github.com/rcamilorodriguez1/Alas-De-Senas-Proyecto/pull/5).

</details>

</details>

Esta navegación corresponde a las vistas de
[`alas-de-senas.code-workspace`](alas-de-senas.code-workspace). En VS Code,
abre ese archivo para usar el explorador del workspace. En GitHub, el listado
de archivos superior muestra las rutas reales del repositorio.

Cada capa se incorpora mediante su propio pull request, que revisa y acepta
el propietario. Mientras una entrega esté pendiente, su carpeta puede contener
solo `.gitkeep`. Los pull requests de las capas pueden aceptarse en cualquier orden.

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
