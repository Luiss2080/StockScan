# Backlog de issues — StockScan

> Lista lista para pasar a GitHub Issues, con milestones, etiquetas, estimaciones y trazabilidad a las historias. Se puede crear todo de una vez con [crear-issues.sh](crear-issues.sh).

## 1. Milestones

| Milestone | Fase | Días | Tareas | Horas est. |
|---|---|---|---|---|
| Fase 1 — Estructura base y navegación | 1 | 3 | 8 | 20 |
| Fase 2 — Productos y escáner | 2 | 5 | 9 | 31.5 |
| Fase 3 — Ventas y movimientos | 3 | 5 | 8 | 32 |
| Fase 4 — Alertas y cierre diario | 4 | 4 | 8 | 24 |
| Fase 5 — Exportación CSV | 5 | 3 | 7 | 17.5 |

Total: **40 tareas**, ~125 horas (20 días a 6 horas efectivas).

## 2. Etiquetas

| Etiqueta | Uso |
|---|---|
| ui | Pantallas y componentes |
| datos | Tablas, consultas y migraciones |
| lógica | Reglas de negocio y cálculos |
| pruebas | Pruebas automáticas y manuales |
| integración | Cámara, notificaciones, PDF, compartir |
| tarea | Otras tareas |
| bug | Errores |
| mejora | Ideas para v1.1+ |

## 3. Issues del MVP

### Fase 1 — Estructura base y navegación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F1-T1` | Crear el proyecto Expo con TypeScript y Expo Router | — | tarea | Alta | 2.5 |
| `F1-T2` | Alinear versiones y herramientas con los repos hermanos | — | tarea | Alta | 2.5 |
| `F1-T3` | Crear la estructura de carpetas | — | tarea | Alta | 2.5 |
| `F1-T4` | Implementar apertura de base y migración 1 | — | datos | Alta | 2.5 |
| `F1-T5` | Definir la navegación principal | — | ui | Alta | 2.5 |
| `F1-T6` | Agregar componente Pantalla, tema e inicio | — | ui | Alta | 2.5 |
| `F1-T7` | Configurar scripts y CI | — | tarea | Alta | 2.5 |
| `F1-T8` | Primera prueba de src/lib | — | pruebas | Alta | 2.5 |

### Fase 2 — Productos y escáner

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F2-T1` | Consultas de productos con código único y baja lógica. | HU-01 | datos, lógica | Alta | 3.5 |
| `F2-T2` | Pantalla Productos con búsqueda y filtro por categoría. | HU-01 | ui | Alta | 3.5 |
| `F2-T3` | Integrar expo-camera (CameraView) para leer EAN-13, EAN-8, UPC y Code 128. | HU-01 | tarea | Alta | 3.5 |
| `F2-T4` | Permiso de cámara pedido en contexto, con pantalla de ayuda si se deniega. | HU-01 | ui, integración | Alta | 3.5 |
| `F2-T5` | Ingreso manual del código como alternativa al escáner. | HU-01 | tarea | Alta | 3.5 |
| `F2-T6` | Pruebas de validación de precios y códigos. | HU-01 | lógica, pruebas | Alta | 3.5 |
| `F2-T7` | Validaciones, estados vacíos y mensajes en español | HU-01 | lógica | Alta | 3.5 |
| `F2-T8` | Pruebas automáticas y verificación manual de criterios | HU-01 | pruebas | Alta | 3.5 |
| `F2-T9` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-01 | pruebas | Alta | 3.5 |

### Fase 3 — Ventas y movimientos

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F3-T1` | Pantalla Nueva venta con lista de líneas y total en vivo. | HU-02, HU-03 | ui | Alta | 4 |
| `F3-T2` | Transacción única que crea venta, movimientos y descuenta stock. | HU-02, HU-03 | tarea | Alta | 4 |
| `F3-T3` | Aviso si el stock quedaría negativo. | HU-02, HU-03 | tarea | Alta | 4 |
| `F3-T4` | Registro de entradas de mercadería. | HU-02, HU-03 | tarea | Alta | 4 |
| `F3-T5` | Función pura de stock a partir de movimientos, con pruebas. | HU-02, HU-03 | lógica, pruebas | Alta | 4 |
| `F3-T6` | Validaciones, estados vacíos y mensajes en español | HU-02, HU-03 | lógica | Alta | 4 |
| `F3-T7` | Pruebas automáticas y verificación manual de criterios | HU-02, HU-03 | pruebas | Alta | 4 |
| `F3-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-02, HU-03 | pruebas | Alta | 4 |

### Fase 4 — Alertas y cierre diario

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F4-T1` | Lista de productos por agotarse (stock ≤ mínimo). | HU-04, HU-05 | ui | Media | 3 |
| `F4-T2` | Indicador en Inicio con la cantidad de alertas. | HU-04, HU-05 | tarea | Media | 3 |
| `F4-T3` | Función pura de cierre (total, efectivo, diferencia) con pruebas. | HU-04, HU-05 | lógica, pruebas | Media | 3 |
| `F4-T4` | Pantalla Cierre del día con restricción de un cierre por fecha. | HU-04, HU-05 | ui | Media | 3 |
| `F4-T5` | Historial de cierres. | HU-04, HU-05 | tarea | Media | 3 |
| `F4-T6` | Validaciones, estados vacíos y mensajes en español | HU-04, HU-05 | lógica | Media | 3 |
| `F4-T7` | Pruebas automáticas y verificación manual de criterios | HU-04, HU-05 | pruebas | Media | 3 |
| `F4-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-04, HU-05 | pruebas | Media | 3 |

### Fase 5 — Exportación CSV

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F5-T1` | Generador CSV con separador `;`, comillas y BOM UTF-8. | HU-06 | integración | Media | 2.5 |
| `F5-T2` | Exportar inventario y ventas con expo-file-system. | HU-06 | integración | Media | 2.5 |
| `F5-T3` | Compartir con expo-sharing. | HU-06 | integración | Media | 2.5 |
| `F5-T4` | Pruebas del generador (comas, comillas, acentos). | HU-06 | pruebas | Media | 2.5 |
| `F5-T5` | Validaciones, estados vacíos y mensajes en español | HU-06 | lógica | Media | 2.5 |
| `F5-T6` | Pruebas automáticas y verificación manual de criterios | HU-06 | pruebas | Media | 2.5 |
| `F5-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-06 | pruebas | Media | 2.5 |

## 4. Orden recomendado de ataque

1. Terminá cada fase completa antes de abrir la siguiente; dentro de la fase, de arriba hacia abajo.
2. Las tareas `lógica` y `datos` van antes que las `ui` que las usan.
3. Cada issue se cierra con un commit o PR que lo mencione (`Cierra #N`).
4. Nada de abrir issues de v1.1 hasta tener el APK de la v1.0.0.

## 5. Ideas para después del MVP

| Idea | Esfuerzo | Valor |
|---|---|---|
| Modo oscuro | Bajo | Medio |
| Búsqueda y filtros avanzados | Medio | Alto |
| Estadísticas y gráficos simples en inicio | Medio | Medio |
| Respaldo y restauración de la base | Medio | Alto |
| Publicación en Google Play | Bajo | Alto |
| Sincronización opcional entre dispositivos | Alto | Depende de demanda |
