# Especificación del MVP — StockScan

> Documento vivo. Si una decisión cambia, se actualiza acá primero y después en el código.

## 1. Resumen

**Propuesta de valor:** Que el dueño de la tienda sepa cuánto stock tiene, qué se va a agotar y cuánto debería haber en caja al cerrar el día.

**Problema:** Las tiendas pequeñas llevan el inventario de memoria o en cuadernos; descubren que algo se agotó cuando el cliente lo pide y al cerrar la caja no cuadran las cuentas.

**Cómo se resuelve hoy:** Cuaderno, memoria del dueño y calculadora.

**Usuario objetivo:** Dueño o encargado de una tienda de barrio, kiosco o minimarket con 100 a 2000 productos.

**Métricas de éxito del MVP:**

- Registrar una venta escaneando 3 productos en menos de 20 segundos.
- Ver los productos bajo stock mínimo en una sola lista.
- Cerrar la caja del día con diferencia calculada automáticamente.

## 2. Alcance del MVP

### Incluido

- Escaneo de códigos con la cámara para vender y reponer productos.
- Alertas de productos que están por agotarse.
- Registro de ventas rápidas y de entradas de mercadería.
- Cierre de caja diario con totales.
- Catálogo de productos con costo, precio y categoría.
- Exportación a CSV del inventario y de las ventas.

### Fuera del MVP

- Cuentas de usuario, inicio de sesión y multiusuario.
- Servidor propio, nube o sincronización entre dispositivos.
- Pagos en línea o pasarelas de cobro.
- Publicación en App Store (iOS).
- Cualquier servicio de pago o con costo recurrente.

## 3. Reglas de negocio

Reglas que el código **debe** cumplir. Cada una se cubre con pruebas automáticas cuando es lógica pura.

- **RN-01** El código de barras es único por producto; productos sin código se identifican por nombre.
- **RN-02** El stock de un producto es la suma de sus movimientos: entradas y ajustes positivos suman, ventas y ajustes negativos restan.
- **RN-03** Una venta descuenta stock; si el stock llegaría a negativo se avisa y se permite continuar con confirmación (ajuste posterior).
- **RN-04** Un producto está «por agotarse» cuando stock ≤ mínimo.
- **RN-05** Total de venta = suma de cantidad × precio unitario del momento (el precio queda guardado en el movimiento).
- **RN-06** Margen = precio − costo; ganancia del día = suma de (precio − costo) × cantidad vendida.
- **RN-07** Solo puede haber un cierre por día; la diferencia = efectivo contado − total vendido en efectivo.

## 4. Historias de usuario y criterios de aceptación

Formato: **Dado** un contexto, **Cuando** el usuario hace algo, **Entonces** pasa algo verificable.

### HU-01 — Cargar el catálogo de productos

**Como** dueño de tienda **quiero** crear productos con código, costo, precio, categoría y stock inicial **para** tener mi inventario en la app.

**Fase:** 2 — Productos y escáner

**Criterios de aceptación:**

1. **Dado:** el formulario de producto. **Cuando:** escaneo un código con la cámara. **Entonces:** el campo código se completa solo.
2. **Dado:** un código que ya existe. **Cuando:** intento crear otro producto con ese código. **Entonces:** la app avisa y ofrece abrir el producto existente.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-02 — Vender escaneando

**Como** dueño de tienda **quiero** escanear productos y confirmar la venta **para** cobrar rápido.

**Fase:** 3 — Ventas y movimientos

**Criterios de aceptación:**

1. **Dado:** la pantalla de venta. **Cuando:** escaneo 3 productos. **Entonces:** se arma la lista con el total y al confirmar baja el stock.
2. **Dado:** un código no registrado. **Cuando:** lo escaneo. **Entonces:** la app ofrece crear el producto sobre la marcha.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-03 — Registrar entradas de mercadería

**Como** dueño de tienda **quiero** sumar stock al recibir mercadería **para** mantener el inventario real.

**Fase:** 3 — Ventas y movimientos

**Criterios de aceptación:**

1. **Dado:** un producto con stock 3. **Cuando:** registro una entrada de 24. **Entonces:** el stock pasa a 27 y queda un movimiento de entrada.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-04 — Alertas de stock bajo

**Como** dueño de tienda **quiero** ver los productos por agotarse **para** reponer a tiempo.

**Fase:** 4 — Alertas y cierre diario

**Criterios de aceptación:**

1. **Dado:** un producto con mínimo 5 y stock 4. **Cuando:** abro Alertas. **Entonces:** aparece en la lista de reposición.
2. **Dado:** llega el stock a 0. **Cuando:** abro Alertas. **Entonces:** aparece primero con la marca «agotado».

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-05 — Cierre de caja diario

**Como** dueño de tienda **quiero** cerrar el día comparando lo vendido con el efectivo contado **para** saber si faltó o sobró plata.

**Fase:** 4 — Alertas y cierre diario

**Criterios de aceptación:**

1. **Dado:** ventas por 1.250 Bs en efectivo. **Cuando:** ingreso efectivo contado 1.240 Bs. **Entonces:** el cierre muestra una diferencia de −10 Bs.
2. **Dado:** ya existe el cierre de hoy. **Cuando:** intento cerrar de nuevo. **Entonces:** la app muestra el cierre existente y no duplica.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-06 — Exportar a CSV

**Como** dueño de tienda **quiero** exportar inventario y ventas **para** pasarlos al contador.

**Fase:** 5 — Exportación CSV

**Criterios de aceptación:**

1. **Dado:** un inventario de 500 productos. **Cuando:** toco «Exportar inventario». **Entonces:** se genera un CSV con una fila por producto.
2. **Dado:** ventas del mes. **Cuando:** toco «Exportar ventas». **Entonces:** se genera un CSV con una fila por línea de venta.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

## 5. Pantallas y navegación

| Pantalla | Qué muestra | Acciones principales |
|---|---|---|
| Inicio | Ventas de hoy, ganancia estimada y productos por agotarse | Nueva venta, escanear |
| Productos | Lista con búsqueda por nombre o código y filtro por categoría | Crear, editar, escanear |
| Formulario de producto | Código, nombre, categoría, costo, precio, stock, mínimo | Guardar, escanear código |
| Escáner | Vista de cámara con detección de códigos de barras | Escanear, linterna, ingresar manual |
| Nueva venta | Líneas escaneadas con cantidad editable y total | Confirmar, cancelar |
| Entradas | Producto, cantidad y costo opcional | Guardar |
| Alertas | Productos bajo mínimo ordenados por urgencia | Registrar entrada |
| Cierre del día | Total vendido, efectivo contado y diferencia | Cerrar, ver historial |
| Exportar | Opciones de CSV | Exportar y compartir |

**Navegación:** la pantalla de inicio es el punto de partida; desde ahí se llega a cada sección con un toque y se vuelve con el botón atrás. Las acciones frecuentes (nuevo registro) están siempre a la vista. La navegación se implementa con Expo Router (rutas por archivo en `app/`).

## 6. Modelo de datos

Base SQLite local versionada con `PRAGMA user_version`. Las fechas se guardan como texto ISO 8601 y los montos como números reales (en bolivianos).

### `productos`

Catálogo con código de barras, costo, precio, stock y mínimo (se agrega categoría y estado activo respecto al README).

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `codigo` | TEXT | Único |
| `nombre` | TEXT | Obligatorio |
| `categoria` | TEXT | Opcional |
| `costo` | REAL | Obligatorio; Por defecto 0 |
| `precio` | REAL | Obligatorio |
| `stock` | INTEGER | Obligatorio; Por defecto 0 |
| `minimo` | INTEGER | Obligatorio; Por defecto 0 |
| `activo` | INTEGER | Obligatorio; Por defecto 1 |

### `ventas`

Cabecera de cada venta con su total.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `fecha` | TEXT | Obligatorio |
| `total` | REAL | Obligatorio |

### `movimientos`

Cada entrada, salida por venta o ajuste de stock.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `producto_id` | INTEGER | Obligatorio; Referencia a productos |
| `tipo` | TEXT | Obligatorio |
| `cantidad` | INTEGER | Obligatorio |
| `precio_unitario` | REAL | Opcional |
| `venta_id` | INTEGER | Referencia a ventas |
| `fecha` | TEXT | Obligatorio |

### `cierres`

Cierre diario con total esperado, efectivo contado y diferencia.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `fecha` | TEXT | Obligatorio; Único |
| `total` | REAL | Obligatorio |
| `efectivo` | REAL | Obligatorio |
| `diferencia` | REAL | Obligatorio; Por defecto 0 |

Detalle de la política de migraciones: nunca editar una migración publicada; agregar una nueva al final de la lista en `src/db/schema.ts`.

## 7. Casos borde a contemplar

- Código leído con espacios o saltos de línea: se normaliza antes de buscar.
- Dos productos sin código con el mismo nombre: advertir posible duplicado.
- Cambio de precio: las ventas pasadas conservan el precio con el que se vendieron.
- Anulación de una venta: genera movimientos inversos y no borra el historial.
- Producto dado de baja con stock: se oculta de ventas pero se conserva en reportes.

## 8. Requisitos no funcionales

- **Offline total:** ninguna función del MVP requiere internet.
- **Rendimiento:** la app abre en menos de 2 segundos; las listas responden de inmediato con cientos de registros (consultas con índices y listas virtualizadas).
- **Privacidad:** los datos nunca salen del teléfono, salvo lo que el usuario comparte a propósito.
- **Idioma y formato:** español de Bolivia, montos en bolivianos (Bs) con dos decimales, fechas día/mes/año.
- **Usabilidad:** botones grandes, formularios cortos y flujos de pocos toques; pensado para usarse de pie y con una mano.
- **Accesibilidad básica:** contraste suficiente, etiquetas de accesibilidad en botones y tamaño de texto que respeta el ajuste del sistema.
- **Calidad:** `npm run lint`, `npx tsc --noEmit` y `npm test` pasan en cada commit (CI en GitHub Actions).
- **Compatibilidad:** Android 8 o superior (probado con Expo Go y con el APK).

### Capacidades de Expo que usa

| Paquete | Para qué | Permiso |
|---|---|---|
| `expo-camera` | Escanear códigos de barras o QR | Cámara |
| `expo-file-system` | Guardar y leer archivos locales (CSV, respaldos) | — |
| `expo-sharing` | Compartir PDF, CSV o respaldos con otras apps | — |

## 9. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Lecturas de código lentas o fallidas con poca luz | Linterna, enfoque táctil e ingreso manual. |
| Pérdida del teléfono con todo el inventario | Exportación CSV periódica y respaldo en v1.1. |
| Cambios de esquema que rompan datos | Migraciones con `PRAGMA user_version`; nunca editar una migración publicada |
| El alcance crece | Todo lo que no esté en la sección 2 va a «Después del MVP» en el [ROADMAP](ROADMAP.md) |

## 10. Criterios para lanzar el MVP (v1.0.0)

- [ ] Todas las historias de la sección 4 cumplen sus criterios de aceptación.
- [ ] Todas las reglas de negocio de la sección 3 tienen pruebas o verificación manual.
- [ ] Probado en un teléfono Android real, en modo avión.
- [ ] APK generado con `npm run build:apk` e instalado en un teléfono limpio.
- [ ] README con capturas de pantalla y CHANGELOG actualizado.
- [ ] Release v1.0.0 publicado con el APK adjunto.

## 11. Glosario

| Término | Significado en StockScan |
|---|---|
| `productos` | Catálogo con código de barras, costo, precio, stock y mínimo (se agrega categoría y estado activo respecto al README). |
| `ventas` | Cabecera de cada venta con su total. |
| `movimientos` | Cada entrada, salida por venta o ajuste de stock. |
| `cierres` | Cierre diario con total esperado, efectivo contado y diferencia. |
| `movimientos.tipo` | Valores: entrada, venta, ajuste |

## 12. Mensajes estándar

| Situación | Mensaje |
|---|---|
| Campo obligatorio vacío | Completá este campo |
| Número inválido | Ingresá un número válido |
| Monto cero o negativo | El monto debe ser mayor a 0 |
| Fecha inválida | Fecha no válida |
| Guardado correcto | Guardado |
| Error al guardar | No se pudo guardar. Intentá de nuevo |
| Confirmar borrado | ¿Borrar «{nombre}»? No se puede deshacer |
| Lista vacía | Todavía no hay nada acá. Tocá «+» para empezar |

## 13. Trazabilidad

| Historia | Fase | Casos de prueba |
|---|---|---|
| HU-01 Cargar el catálogo de productos | 2 | CP-01-1, CP-01-2 |
| HU-02 Vender escaneando | 3 | CP-02-1, CP-02-2 |
| HU-03 Registrar entradas de mercadería | 3 | CP-03-1 |
| HU-04 Alertas de stock bajo | 4 | CP-04-1, CP-04-2 |
| HU-05 Cierre de caja diario | 4 | CP-05-1, CP-05-2 |
| HU-06 Exportar a CSV | 5 | CP-06-1, CP-06-2 |

## 14. Documentos relacionados

- [DATOS.md](DATOS.md) · [PANTALLAS.md](PANTALLAS.md) · [PRUEBAS.md](PRUEBAS.md) · [BACKLOG.md](BACKLOG.md) · [ROADMAP.md](ROADMAP.md) · [PLANNING.md](PLANNING.md)
