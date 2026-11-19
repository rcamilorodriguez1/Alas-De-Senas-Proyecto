# Mejora de Cámara Beta: señas con movimiento

Fecha: 2026-11-11 (America/Bogota).

## Encargo del usuario a Claude Code

El usuario aprobó el flujo de este documento y pidió: «me gusta, pasale el flujo a claude code para que lo realize».

Implementa la mejora por etapas, con cambios pequeños y verificables. Empieza por el visor de fotogramas, el recorte editable y la evaluación de las cinco señas actuales. Continúa con las etapas que puedas realizar con los datos y herramientas disponibles. Las grabaciones de personas nuevas y la validación lingüística requieren participación humana: prepara sus herramientas y protocolo, registra lo pendiente y no inventes resultados. Este encargo es de implementación, no solamente de planificación.

Trabaja desde `C:\Proyectos\alas-de-senas`. Lee `CLAUDE.md` y sus fuentes de verdad. Respeta los hooks y permisos del repositorio. La autorización comprende este flujo; registra las decisiones técnicas necesarias en `docs/DECISIONES.md`. Si contradicen documentos protegidos, explica la diferencia sin modificar esos documentos ni eludir sus protecciones. No amplíes el alcance a otras áreas de la aplicación.

## Contexto comprobado antes de la entrega

Revalida estos datos al comenzar; pueden cambiar si hay otra sesión trabajando.

- `apps/web/src/camera/holistic/importer.ts`: procesa videos aproximadamente a 15 cuadros por segundo y agrupa secuencias de imágenes. Un video o grupo de imágenes ordenadas equivale a una muestra.
- `apps/web/src/camera/holistic/sequencer.ts`: remuestrea a 30 cuadros; el recorte actual conserva el tramo entre la primera y última presencia de la mano dominante. Esto no identifica necesariamente el principio y final de la seña.
- `apps/web/src/camera/holistic/frame.ts`: representación `holistic-v2`, 159 valores por cuadro, normalización compartida entre importación y cámara.
- `apps/web/src/camera/holistic/gesture-session.ts`: captura actual de dos segundos después de la cuenta regresiva.
- `ml/train_sequence.py`: GRU temporal; ya incluye aumentos de velocidad, rotación, escala, ruido y reflejo, además de validación por persona. Mantener `reset_after=False` por compatibilidad con TensorFlow.js.
- `apps/web/public/models/lsc-gestos/labels.json`: `NINGUNA`, `adios`, `de-nada`, `hola`, `perdon`, `por-favor`.
- `meta.json` del mismo directorio: 409 muestras de LSC-54, 18 personas, `cvAccuracy` 0.8973105134474327, `threshold` 0.7 y `faceFeatures: false`. Es un resultado registrado de validación; no es una medición nueva de cámara real.
- `docs/PROGRESS.md` registra confusión entre hola y adiós y validaciones manuales todavía necesarias.

MediaPipe extrae puntos; el GRU aprende las señas a partir de sus secuencias. Extraer más fotogramas del mismo video no produce nuevas ejecuciones independientes ni nuevas clases.

Al preparar este encargo, `git status --short` mostraba cambios previos en `.vscode/settings.json`, `.claude/skills/Fronted-Desing/`, `apps/web/.vscode/` y `requirements.txt`. Son ajenos a este encargo: no descartarlos ni incluirlos incidentalmente en cambios o commits.

## Flujo de trabajo y entregables

### 1. Establecer una referencia de evaluación

- Inspeccionar los datasets, videos y reportes disponibles localmente sin alterar originales.
- Preparar una evaluación reproducible de las cinco señas, NINGUNA y movimientos cotidianos.
- Separar por identidad de persona los conjuntos de entrenamiento, validación y prueba. Mantener todos los videos, fotogramas, recortes y aumentos derivados de una persona dentro de su partición. Evitar duplicados entre fuentes si se conoce que corresponden a la misma persona.
- Reservar la prueba final para comparar las versiones terminadas; ajustar parámetros solamente con entrenamiento y validación.
- Medir acierto y recuperación por clase, confusiones, falsas detecciones, rechazos de calidad y latencia. Evaluar la decisión real de la aplicación, incluido su umbral, no solo el argmax del modelo.
- Si no hay material independiente para una prueba real, dejar herramientas y protocolo listos y señalar qué mediciones siguen pendientes.

Entregable: informe inicial con procedencia de datos, particiones, parámetros y resultados realmente ejecutados; identificación clara de datos faltantes.

### 2. Visor de videos y fotogramas en el importador

Integrar en `/admin/importar`, aprovechando `ImportPage.tsx` y el pipeline existente:

- Reproducción normal y lenta, pausa y avance/retroceso por cuadros muestreados.
- Puntos de ambas manos y trayectoria de las muñecas sobre el video, alineados en tiempo y coordenadas.
- Línea temporal para comparar el video, los cuadros extraídos y la secuencia normalizada que llegará al modelo.
- Indicadores de manos detectadas, cuadros faltantes y calidad de seguimiento.
- Conservación de la relación con el video original, persona, toma, etiqueta, origen y tiempos de los cuadros. Para imágenes, definir explícitamente FPS o timestamps; no asumir que cualquier secuencia viene a 15 FPS.
- Procesamiento local, carga progresiva o caché limitada y cancelación al salir; no cargar todos los bitmaps de un dataset en memoria.
- Liberar cámaras, detectores, bitmaps y object URLs cuando corresponda. Mantener accesibilidad por teclado y textos en español.

Entregable: un administrador puede inspeccionar qué información del movimiento se conserva y dónde falla la extracción.

### 3. Recorte editable y revisión del dataset

- Permitir marcar inicio y final de la seña y previsualizar el intervalo seleccionado.
- Mantener las pausas internas que forman parte de la seña; no cortar por una simple ausencia momentánea de movimiento.
- Recalcular calidad y secuencia sobre el mismo intervalo seleccionado.
- Guardar metadatos del recorte en un manifiesto o formato compatible que permita reproducirlo, sin modificar videos originales.
- Permitir revisar etiquetas y excluir muestras defectuosas con motivo. Respetar currículo y mapeo de etiquetas.
- Mantener compatibilidad con datasets existentes. Si cambia el contrato de características, versionarlo explícitamente y actualizar de forma coordinada importación, entrenamiento e inferencia.

Entregable: dataset revisable y exportación reproducible de las secuencias recortadas.

**Primer hito funcional:** pasos 1–3, con visor, recorte editable, reporte inicial y pruebas pertinentes. No declarar que la precisión mejoró antes de medirlo.

### 4. Ampliar ejemplos de forma dirigida

- Preparar el colector y una guía para grabar nuevas personas desde la aplicación; priorizar hola/adiós y ejemplos de NINGUNA.
- Incluir distintas ejecuciones, velocidades, distancias, iluminación y equipos de escritorio/portátiles.
- Para NINGUNA, incluir reposo, acomodarse, entrar/salir del encuadre y movimientos cotidianos; distinguir estos ejemplos de fallos técnicos de captura.
- Mantener identificadores de persona y toma, origen y consentimiento según el flujo existente. Las grabaciones son voluntarias y no se suben automáticamente.
- Conservar e investigar los aumentos existentes; no duplicar mecanismos sin evaluar si preservan la seña.
- Preparar revisión de etiquetas y variantes con una persona competente en LSC. No inventar validaciones lingüísticas.

Entregable: herramientas de recolección, protocolo y balance de muestras por seña/persona. Las grabaciones que no existan quedan como dependencia humana explícita.

### 5. Reentrenamiento y comparación controlada

- Primero mantener el GRU y la entrada actual de 30 cuadros para medir el efecto de limpiar y recortar los datos.
- Normalización y aumentos calculados/aplicados solamente sobre entrenamiento dentro de cada partición.
- Si existen datos y entorno suficientes, entrenar un candidato separado del modelo publicado y generar comparación reproducible.
- Después probar, por separado, dirección/velocidad de muñecas, cambios de dedos y distancia entre manos. Preservar la referencia al cuerpo y usar los tiempos de captura cuando se calculen velocidades.
- Estos cambios de representación requieren un contrato versionado; no alimentar al modelo actual con dimensiones o semántica diferentes.
- Ajustar umbrales con validación y analizar falsas detecciones y rechazos. No bajar el umbral para aparentar más aciertos.
- Verificar exportación y equivalencia de predicciones Keras/TensorFlow.js.
- Si faltan datos, dependencias autorizadas o capacidad de entrenamiento, entregar el procedimiento ejecutable y dejar claro qué no se ha entrenado.

Entregable: candidato y reporte cuando sea posible; herramientas reproducibles y pendientes concretos cuando no lo sea.

### 6. Captura flexible en Cámara Beta

- Reducir la dependencia de la ventana fija de dos segundos. Primero ofrecer inicio/final controlados, duración limitada y cancelación clara.
- Después evaluar inicio/final automáticos con margen previo/posterior, estabilidad temporal y consideración de pausas internas. No convertir un gesto quieto o pérdida de seguimiento en un final automático sin validar.
- Mantener el procesamiento temporal coherente con el importador y con el contrato del modelo. Conservar el modo actual como alternativa durante la evaluación.
- Mostrar estados comprensibles: preparación, grabando, analizando, resultado y repetir.
- Distinguir «captura incompleta», «no reconocida» y «seña reconocida». Una puntuación del clasificador no equivale a certificar que la ejecución lingüística sea correcta.
- En práctica guiada, integrar referencia en video y repetición del intento cuando sea viable. Las correcciones deben apoyarse en mediciones; las indicaciones lingüísticas necesitan validación.

Entregable: captura de intentos completos con ritmos diferentes, sin regresiones del control de cámara, navegación o accesibilidad.

### 7. Validación final e incorporación

- Repetir la evaluación reservada y comparar con la referencia: resultados por clase, falsas detecciones, muestras rechazadas y latencia.
- Probar escritorio/portátil, diferentes cámaras y personas no usadas para entrenar. La comprobación humana pendiente debe quedar diferenciada de pruebas automatizadas o cámara simulada.
- Incorporar un modelo candidato solo si hay evidencia suficiente; conservar la versión anterior y un procedimiento de reversión. No publicar ni hacer push automáticamente por este encargo.
- Actualizar `docs/PROGRESS.md` y las decisiones aplicables con cambios, comandos ejecutados, resultados y tareas manuales pendientes.

Entregable: implementación comprobada, comparación verificable y pasos de validación manual para el usuario.

## Archivos de entrada para la implementación

- `apps/web/src/features/camera/ImportPage.tsx`
- `apps/web/src/features/camera/GestureDatasetPage.tsx`
- `apps/web/src/features/camera/GestureCamera.tsx`
- `apps/web/src/features/camera/GesturePractice.tsx`
- `apps/web/src/camera/holistic/importer.ts`
- `apps/web/src/camera/holistic/sequencer.ts`
- `apps/web/src/camera/holistic/frame.ts`
- `apps/web/src/camera/holistic/dataset-v2.ts`
- `apps/web/src/camera/holistic/gesture-session.ts`
- `apps/web/src/camera/holistic/sequence-classifier.ts`
- `apps/web/src/camera/holistic/feedback.ts`
- `ml/train_sequence.py` y `ml/tests/test_train_sequence.py`
- `content/dataset-mapping.csv`, `content/signs.csv`

## Verificación y criterios de aceptación

- Seguir la política de pruebas del repositorio para cambios con lógica. Cubrir orden/tiempos de cuadros, límites del recorte, pausas y huecos de detección, calidad dentro del intervalo, compatibilidad de exportación, particiones sin filtración y cancelación/liberación de recursos según lo modificado.
- Ejecutar comprobaciones pertinentes: desde la raíz, `npm --prefix apps/web run typecheck`, `npm --prefix apps/web run lint`, `npm --prefix apps/web run test -- --run`; para entrenamiento, `uv run --project ml pytest ml/tests/test_train_sequence.py` si el entorno está disponible.
- Al cerrar un grupo seguir `CLAUDE.md`, incluida `uv run alas check` y las revisiones del repositorio. No ejecutar `alas e2e` simultáneamente con `alas check`.
- Distinguir fallos previos o de entorno de regresiones propias. No debilitar pruebas ni desactivar hooks para completar el trabajo.
- Mantener el procesamiento de cámara local y los controles de acceso administrativos.
- No cambiar dependencias fijas, modelos publicados, archivos protegidos ni trabajo ajeno incidentalmente.
- Informar qué quedó implementado, qué se probó, si se entrenó realmente un candidato y qué requiere personas/datos. Una mejora de interfaz por sí sola no demuestra mayor precisión.

## Flujo esperado para quien practica

Elegir una seña → ver el ejemplo → colocarse en el encuadre → realizar la seña → analizar la secuencia completa → recibir el resultado y una indicación concreta → repetir.
