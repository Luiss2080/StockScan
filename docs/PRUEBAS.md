# Plan de pruebas — StockScan

> Qué probar, cuándo y cómo. Cada historia y cada regla de negocio tiene casos concretos. Complementa a [SPEC.md](SPEC.md).

## 1. Estrategia

| Nivel | Herramienta | Qué cubre | Cuándo |
|---|---|---|---|
| Unitarias | jest-expo (`npm test`) | Lógica pura de `src/lib/` (cálculos, validaciones, formatos) | En cada commit (CI) |
| Manuales por historia | Teléfono con Expo Go | Criterios de aceptación de SPEC (casos CP-) | Al cerrar cada tarea de pantalla |
| Regresión por fase | Teléfono real | Lo ya terminado sigue andando | Al cerrar cada fase |
| Aceptación final | APK en teléfono limpio, modo avión | Criterios de lanzamiento | Antes de v1.0.0 |

Meta de cobertura: **100 % de las reglas de negocio** con prueba automática o caso manual, y **100 % de los criterios de aceptación** ejecutados al menos una vez en un teléfono real.

## 2. Pruebas unitarias por regla de negocio

Crear un archivo `src/lib/<tema>.test.ts` por tema. Una prueba por regla como mínimo; agregar los bordes de la sección 4.

| ID | Regla | Tipo de prueba | Casos mínimos |
|---|---|---|---|
| RN-01 | El código de barras es único por producto; productos sin código se identifican por nombre. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-02 | El stock de un producto es la suma de sus movimientos: entradas y ajustes positivos suman, ventas y ajustes negativos restan. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-03 | Una venta descuenta stock; si el stock llegaría a negativo se avisa y se permite continuar con confirmación (ajuste posterior). | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-04 | Un producto está «por agotarse» cuando stock ≤ mínimo. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-05 | Total de venta = suma de cantidad × precio unitario del momento (el precio queda guardado en el movimiento). | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-06 | Margen = precio − costo; ganancia del día = suma de (precio − costo) × cantidad vendida. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-07 | Solo puede haber un cierre por día; la diferencia = efectivo contado − total vendido en efectivo. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |

## 3. Casos manuales por historia

Formato: ejecutar en el teléfono, marcar ✔ o ✘ y anotar la fecha. Si falla, abrir un issue con el ID del caso.

### HU-01 — Cargar el catálogo de productos (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-01-1 | el formulario de producto | escaneo un código con la cámara | el campo código se completa solo | ☐ |
| CP-01-2 | un código que ya existe | intento crear otro producto con ese código | la app avisa y ofrece abrir el producto existente | ☐ |

Casos transversales: **CP-01-T1** modo avión · **CP-01-T2** cerrar y reabrir la app (persistencia) · **CP-01-T3** texto del sistema en tamaño grande.

### HU-02 — Vender escaneando (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-02-1 | la pantalla de venta | escaneo 3 productos | se arma la lista con el total y al confirmar baja el stock | ☐ |
| CP-02-2 | un código no registrado | lo escaneo | la app ofrece crear el producto sobre la marcha | ☐ |

Casos transversales: **CP-02-T1** modo avión · **CP-02-T2** cerrar y reabrir la app (persistencia) · **CP-02-T3** texto del sistema en tamaño grande.

### HU-03 — Registrar entradas de mercadería (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-03-1 | un producto con stock 3 | registro una entrada de 24 | el stock pasa a 27 y queda un movimiento de entrada | ☐ |

Casos transversales: **CP-03-T1** modo avión · **CP-03-T2** cerrar y reabrir la app (persistencia) · **CP-03-T3** texto del sistema en tamaño grande.

### HU-04 — Alertas de stock bajo (fase 4)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-04-1 | un producto con mínimo 5 y stock 4 | abro Alertas | aparece en la lista de reposición | ☐ |
| CP-04-2 | llega el stock a 0 | abro Alertas | aparece primero con la marca «agotado» | ☐ |

Casos transversales: **CP-04-T1** modo avión · **CP-04-T2** cerrar y reabrir la app (persistencia) · **CP-04-T3** texto del sistema en tamaño grande.

### HU-05 — Cierre de caja diario (fase 4)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-05-1 | ventas por 1.250 Bs en efectivo | ingreso efectivo contado 1.240 Bs | el cierre muestra una diferencia de −10 Bs | ☐ |
| CP-05-2 | ya existe el cierre de hoy | intento cerrar de nuevo | la app muestra el cierre existente y no duplica | ☐ |

Casos transversales: **CP-05-T1** modo avión · **CP-05-T2** cerrar y reabrir la app (persistencia) · **CP-05-T3** texto del sistema en tamaño grande.

### HU-06 — Exportar a CSV (fase 5)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-06-1 | un inventario de 500 productos | toco «Exportar inventario» | se genera un CSV con una fila por producto | ☐ |
| CP-06-2 | ventas del mes | toco «Exportar ventas» | se genera un CSV con una fila por línea de venta | ☐ |

Casos transversales: **CP-06-T1** modo avión · **CP-06-T2** cerrar y reabrir la app (persistencia) · **CP-06-T3** texto del sistema en tamaño grande.

## 4. Casos borde

| ID | Caso | Resultado esperado | Resultado |
|---|---|---|---|
| CB-01 | Código leído con espacios o saltos de línea: se normaliza antes de buscar. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-02 | Dos productos sin código con el mismo nombre: advertir posible duplicado. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-03 | Cambio de precio: las ventas pasadas conservan el precio con el que se vendieron. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-04 | Anulación de una venta: genera movimientos inversos y no borra el historial. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-05 | Producto dado de baja con stock: se oculta de ventas pero se conserva en reportes. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |

## 5. Pruebas de datos y migraciones

| ID | Prueba | Esperado |
|---|---|---|
| CD-01 | Primer arranque en teléfono limpio | La base se crea con todas las tablas y `user_version` queda en 1 |
| CD-02 | Actualizar de una versión anterior con datos cargados | Las migraciones nuevas se aplican y no se pierde ningún registro |
| CD-03 | Borrar un registro padre | Los hijos se borran o se bloquea con mensaje, según el esquema |
| CD-04 | Intentar guardar con campos obligatorios vacíos | No se inserta nada y se marcan los campos |
| CD-05 | Cargar 1.000 registros de prueba | Las listas siguen fluidas (scroll sin trabas) y abren en menos de 2 s |

## 6. Regresión al cerrar cada fase

**Fase 1 — Estructura base y navegación**

- [ ] La app abre y navega sin errores

**Fase 2 — Productos y escáner**

- [ ] Un producto se crea escaneando su código.
- [ ] Con el permiso denegado la app sigue usable con ingreso manual.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 3 — Ventas y movimientos**

- [ ] Una venta de 3 productos baja el stock correctamente y queda en historial.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 4 — Alertas y cierre diario**

- [ ] El cierre calcula la diferencia correcta en los casos de prueba.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 5 — Exportación CSV**

- [ ] Los CSV abren bien en Excel en español.
- [ ] Los criterios de las fases anteriores siguen funcionando

## 7. Matriz de dispositivos

| Dispositivo | Android | Qué se verifica |
|---|---|---|
| Gama baja (2–3 GB de RAM) | 8–10 | Fluidez de listas, tiempo de arranque |
| Gama media | 11–13 | Flujo completo y APK |
| Pantalla pequeña (5") | cualquiera | Que nada quede cortado ni oculto por el teclado |
| Texto del sistema grande | cualquiera | Que los formularios sigan siendo usables |

## 8. Lista de verificación para el lanzamiento (v1.0.0)

- [ ] Todas las historias ejecutadas en teléfono real (casos CP-)
- [ ] Todos los casos borde (CB-) ejecutados
- [ ] Pruebas de datos (CD-) ejecutadas
- [ ] APK instalado en un teléfono limpio sin Expo Go
- [ ] Probado en modo avión durante todo el flujo principal
- [ ] Rotación de pantalla no pierde datos del formulario
- [ ] Permisos solicitados solo cuando se necesitan y con explicación
- [ ] Sin errores en la consola durante un recorrido completo
- [ ] README con capturas y CHANGELOG al día

## 9. Informe de errores

```
Título: [Bug] <qué pasó>
ID de caso: CP-XX-X
Pasos: 1... 2... 3...
Esperado: ...
Obtenido: ...
Dispositivo / Android: ...
Captura o video: ...
```
