# Plan de desarrollo — StockScan

Guía para construir el MVP **fase por fase**. Complementa a [ROADMAP.md](ROADMAP.md) (qué y cuándo) y [SPEC.md](SPEC.md) (qué debe hacer la app).

## Forma de trabajo

1. **Una fase por rama:** `git checkout -b fase-N-nombre`.
2. **Un issue por tarea** (usá los títulos de abajo) y un milestone por fase en GitHub.
3. **Commits chicos** con prefijo: `feat:`, `fix:`, `test:`, `docs:`, `chore:`.
4. **Pull request al cerrar la fase** con la lista de criterios de salida marcada.
5. **Primero la lógica pura y sus pruebas** (en `src/lib/`), después la pantalla que la usa.
6. **Probá en el teléfono** al terminar cada tarea de pantalla, no solo al final de la fase.

### Definición de «terminado» (vale para toda tarea y fase)

- [ ] `npm run lint`, `npx tsc --noEmit` y `npm test` sin errores.
- [ ] Funciona en modo avión.
- [ ] Estados vacíos, de carga y de error resueltos, con textos en español.
- [ ] Los datos persisten al reiniciar la app.
- [ ] La regla de negocio correspondiente tiene prueba o verificación manual anotada.
- [ ] CHANGELOG actualizado al cerrar la fase.

## Calendario sugerido

| Fase | Nombre | Días | Acumulado |
|---|---|---|---|
| 1 | Estructura base y navegación | 3 | 3 |
| 2 | Productos y escáner | 5 | 8 |
| 3 | Ventas y movimientos | 5 | 13 |
| 4 | Alertas y cierre diario | 4 | 17 |
| 5 | Exportación CSV | 3 | 20 |

Total: **20 días** (~4 semanas). Si una fase se pasa del doble de lo estimado, recortá alcance dentro de la fase en lugar de saltar a la siguiente.

---

## Fase 1 — Estructura base y navegación

**Estimación:** 3 días.

**Objetivo:** crear el proyecto Expo con base de datos, navegación, tema y CI. Este repositorio hoy solo tiene documentación; la fase 1 agrega el código base.

**Tareas**

- [ ] **F1-T1** Crear el proyecto en una carpeta temporal con `npx create-expo-app@latest` (plantilla con TypeScript y Expo Router) y copiar su contenido a este repositorio sin pisar README, LICENSE ni los archivos de comunidad.
- [ ] **F1-T2** Alinear versiones y herramientas con los repositorios hermanos (por ejemplo PasanakuYa): Expo SDK 58, `expo-sqlite`, `jest-expo`, ESLint, TypeScript estricto, `.editorconfig` y `.prettierrc`.
- [ ] **F1-T3** Crear la estructura `app/`, `src/db/`, `src/components/`, `src/lib/` y `src/theme.ts`.
- [ ] **F1-T4** Implementar `src/db/index.ts` (apertura de la base) y `src/db/schema.ts` con la migración 1 tomada de la sección 6 del [SPEC](SPEC.md).
- [ ] **F1-T5** Definir la navegación principal en `app/_layout.tsx` con una ruta por sección.
- [ ] **F1-T6** Agregar el componente `Pantalla`, el tema y la pantalla de inicio con accesos a cada sección.
- [ ] **F1-T7** Configurar scripts `lint`, `test`, `build:apk` y el workflow de GitHub Actions (`.github/workflows/ci.yml`).
- [ ] **F1-T8** Escribir una primera prueba de `src/lib` para validar que Jest funciona.

**Criterio de salida:** la app abre, navega entre secciones vacías y no da errores; lint, tipos y tests en verde.

---

## Fase 2 — Productos y escáner

**Estimación:** 5 días. **Rama sugerida:** `fase-2-productos-y-escaner`.

**Objetivo:** Catálogo de productos y escáner.

**Historias que se completan:** HU-01 (Cargar el catálogo de productos).

**Tareas**

- [ ] **F2-T1** Consultas de productos con código único y baja lógica.
- [ ] **F2-T2** Pantalla Productos con búsqueda y filtro por categoría.
- [ ] **F2-T3** Integrar expo-camera (CameraView) para leer EAN-13, EAN-8, UPC y Code 128.
- [ ] **F2-T4** Permiso de cámara pedido en contexto, con pantalla de ayuda si se deniega.
- [ ] **F2-T5** Ingreso manual del código como alternativa al escáner.
- [ ] **F2-T6** Pruebas de validación de precios y códigos.
- [ ] **F2-T7** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F2-T8** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F2-T9** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-01 — el formulario de producto → escaneo un código con la cámara → el campo código se completa solo.
- [ ] HU-01 — un código que ya existe → intento crear otro producto con ese código → la app avisa y ofrece abrir el producto existente.

**Criterio de salida:**

- [ ] Un producto se crea escaneando su código.
- [ ] Con el permiso denegado la app sigue usable con ingreso manual.
- [ ] Definición de «terminado» cumplida.

---

## Fase 3 — Ventas y movimientos

**Estimación:** 5 días. **Rama sugerida:** `fase-3-ventas-y-movimientos`.

**Objetivo:** Ventas y movimientos de stock.

**Historias que se completan:** HU-02 (Vender escaneando), HU-03 (Registrar entradas de mercadería).

**Tareas**

- [ ] **F3-T1** Pantalla Nueva venta con lista de líneas y total en vivo.
- [ ] **F3-T2** Transacción única que crea venta, movimientos y descuenta stock.
- [ ] **F3-T3** Aviso si el stock quedaría negativo.
- [ ] **F3-T4** Registro de entradas de mercadería.
- [ ] **F3-T5** Función pura de stock a partir de movimientos, con pruebas.
- [ ] **F3-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F3-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F3-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-02 — la pantalla de venta → escaneo 3 productos → se arma la lista con el total y al confirmar baja el stock.
- [ ] HU-02 — un código no registrado → lo escaneo → la app ofrece crear el producto sobre la marcha.
- [ ] HU-03 — un producto con stock 3 → registro una entrada de 24 → el stock pasa a 27 y queda un movimiento de entrada.

**Criterio de salida:**

- [ ] Una venta de 3 productos baja el stock correctamente y queda en historial.
- [ ] Definición de «terminado» cumplida.

---

## Fase 4 — Alertas y cierre diario

**Estimación:** 4 días. **Rama sugerida:** `fase-4-alertas-y-cierre-diario`.

**Objetivo:** Alertas de stock y cierre diario.

**Historias que se completan:** HU-04 (Alertas de stock bajo), HU-05 (Cierre de caja diario).

**Tareas**

- [ ] **F4-T1** Lista de productos por agotarse (stock ≤ mínimo).
- [ ] **F4-T2** Indicador en Inicio con la cantidad de alertas.
- [ ] **F4-T3** Función pura de cierre (total, efectivo, diferencia) con pruebas.
- [ ] **F4-T4** Pantalla Cierre del día con restricción de un cierre por fecha.
- [ ] **F4-T5** Historial de cierres.
- [ ] **F4-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F4-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F4-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-04 — un producto con mínimo 5 y stock 4 → abro Alertas → aparece en la lista de reposición.
- [ ] HU-04 — llega el stock a 0 → abro Alertas → aparece primero con la marca «agotado».
- [ ] HU-05 — ventas por 1.250 Bs en efectivo → ingreso efectivo contado 1.240 Bs → el cierre muestra una diferencia de −10 Bs.
- [ ] HU-05 — ya existe el cierre de hoy → intento cerrar de nuevo → la app muestra el cierre existente y no duplica.

**Criterio de salida:**

- [ ] El cierre calcula la diferencia correcta en los casos de prueba.
- [ ] Definición de «terminado» cumplida.

---

## Fase 5 — Exportación CSV

**Estimación:** 3 días. **Rama sugerida:** `fase-5-exportacion-csv`.

**Objetivo:** Exportación a CSV.

**Historias que se completan:** HU-06 (Exportar a CSV).

**Tareas**

- [ ] **F5-T1** Generador CSV con separador `;`, comillas y BOM UTF-8.
- [ ] **F5-T2** Exportar inventario y ventas con expo-file-system.
- [ ] **F5-T3** Compartir con expo-sharing.
- [ ] **F5-T4** Pruebas del generador (comas, comillas, acentos).
- [ ] **F5-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F5-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F5-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-06 — un inventario de 500 productos → toco «Exportar inventario» → se genera un CSV con una fila por producto.
- [ ] HU-06 — ventas del mes → toco «Exportar ventas» → se genera un CSV con una fila por línea de venta.

**Criterio de salida:**

- [ ] Los CSV abren bien en Excel en español.
- [ ] Definición de «terminado» cumplida.

---

## Lanzamiento del MVP (v1.0.0)

- [ ] Revisar todas las historias de [SPEC.md](SPEC.md) y marcar sus criterios.
- [ ] Capturas de pantalla para el README.
- [ ] Revisar textos, íconos y nombre de la app en `app.json`.
- [ ] `npm run build:apk` y probar el APK en un teléfono limpio, sin Expo Go y en modo avión.
- [ ] Publicar el release v1.0.0 con el APK adjunto.
- [ ] Pasar las ideas pendientes a issues para la versión 1.1.

## Plantilla de issue

Copiá esto al crear cada issue:

```
Título: [F2-T1] <descripción de la tarea>
Fase: N — <nombre>
Historia: HU-XX
Criterios:
- [ ] ...
Notas técnicas: ...
```

---

## Cronograma día por día

Guía para no perder el hilo: cada día tiene tareas concretas. Si un día se atrasa, mové la tarea al siguiente, no la saltees.

### Fase 1 — Estructura base y navegación

- **Día 1** (día 1 de la fase): `F1-T1` Crear el proyecto Expo con TypeScript y Expo Router; `F1-T2` Alinear versiones y herramientas con los repos hermanos; `F1-T3` Crear la estructura de carpetas.
- **Día 2** (día 2 de la fase): `F1-T4` Implementar apertura de base y migración 1; `F1-T5` Definir la navegación principal; `F1-T6` Agregar componente Pantalla, tema e inicio.
- **Día 3** (día 3 de la fase): `F1-T7` Configurar scripts y CI; `F1-T8` Primera prueba de src/lib.

### Fase 2 — Productos y escáner

- **Día 4** (día 1 de la fase): `F2-T1` Consultas de productos con código único y baja lógica.; `F2-T2` Pantalla Productos con búsqueda y filtro por categoría..
- **Día 5** (día 2 de la fase): `F2-T3` Integrar expo-camera (CameraView) para leer EAN-13, EAN-8, UPC y Code 128.; `F2-T4` Permiso de cámara pedido en contexto, con pantalla de ayuda si se deniega..
- **Día 6** (día 3 de la fase): `F2-T5` Ingreso manual del código como alternativa al escáner.; `F2-T6` Pruebas de validación de precios y códigos..
- **Día 7** (día 4 de la fase): `F2-T7` Validaciones, estados vacíos y mensajes en español; `F2-T8` Pruebas automáticas y verificación manual de criterios.
- **Día 8** (día 5 de la fase): `F2-T9` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 3 — Ventas y movimientos

- **Día 9** (día 1 de la fase): `F3-T1` Pantalla Nueva venta con lista de líneas y total en vivo.; `F3-T2` Transacción única que crea venta, movimientos y descuenta stock..
- **Día 10** (día 2 de la fase): `F3-T3` Aviso si el stock quedaría negativo.; `F3-T4` Registro de entradas de mercadería..
- **Día 11** (día 3 de la fase): `F3-T5` Función pura de stock a partir de movimientos, con pruebas.; `F3-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 12** (día 4 de la fase): `F3-T7` Pruebas automáticas y verificación manual de criterios; `F3-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.
- **Día 13** (día 5 de la fase): colchón: terminar pendientes, probar en el teléfono y ajustar detalles.

### Fase 4 — Alertas y cierre diario

- **Día 14** (día 1 de la fase): `F4-T1` Lista de productos por agotarse (stock ≤ mínimo).; `F4-T2` Indicador en Inicio con la cantidad de alertas..
- **Día 15** (día 2 de la fase): `F4-T3` Función pura de cierre (total, efectivo, diferencia) con pruebas.; `F4-T4` Pantalla Cierre del día con restricción de un cierre por fecha..
- **Día 16** (día 3 de la fase): `F4-T5` Historial de cierres.; `F4-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 17** (día 4 de la fase): `F4-T7` Pruebas automáticas y verificación manual de criterios; `F4-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 5 — Exportación CSV

- **Día 18** (día 1 de la fase): `F5-T1` Generador CSV con separador `;`, comillas y BOM UTF-8.; `F5-T2` Exportar inventario y ventas con expo-file-system.; `F5-T3` Compartir con expo-sharing..
- **Día 19** (día 2 de la fase): `F5-T4` Pruebas del generador (comas, comillas, acentos).; `F5-T5` Validaciones, estados vacíos y mensajes en español; `F5-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 20** (día 3 de la fase): `F5-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

## Rutina diaria

1. Empezá releyendo la tarea del día en [BACKLOG.md](BACKLOG.md) y su regla o historia en [SPEC.md](SPEC.md).
2. Escribí primero la prueba de la lógica, después el código.
3. Probá en el teléfono antes de hacer commit.
4. Cerrá el día con un commit, el issue marcado y una línea en el CHANGELOG si corresponde.

## Qué hacer si te trabás

- Más de 2 horas sin avanzar: simplificá la tarea al caso mínimo y anotá lo que quedó fuera como issue `mejora`.
- Duda de alcance: la respuesta está en la sección 2 del [SPEC](SPEC.md); si no está, no entra al MVP.
- Duda de datos o validaciones: [DATOS.md](DATOS.md).
- Duda de pantallas: [PANTALLAS.md](PANTALLAS.md).
- Duda de cómo probar: [PRUEBAS.md](PRUEBAS.md).
