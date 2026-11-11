# Pantallas y flujos — StockScan

> Guía de interfaz para construir cada pantalla: ruta, contenido, estados y bosquejo. Complementa la sección 5 de [SPEC.md](SPEC.md).

## 1. Mapa de rutas

| Pantalla | Archivo (Expo Router) | Tipo |
|---|---|---|
| Inicio | `app/index.tsx` | Inicio |
| Productos | `app/productos.tsx` | Lista / vista |
| Formulario de producto | `app/formulario-de-producto.tsx` | Formulario |
| Escáner | `app/escaner.tsx` | Lista / vista |
| Nueva venta | `app/nueva-venta.tsx` | Formulario |
| Entradas | `app/entradas.tsx` | Lista / vista |
| Alertas | `app/alertas.tsx` | Lista / vista |
| Cierre del día | `app/cierre-del-dia.tsx` | Lista / vista |
| Exportar | `app/exportar.tsx` | Lista / vista |

**Convenciones de interfaz**

- Una acción principal por pantalla, en un botón grande abajo o en un botón flotante.
- Los formularios se abren como pantalla completa, validan al enviar y muestran el error junto al campo.
- Toda lista tiene estado vacío con una frase y un botón que lleva a la acción.
- Las acciones que borran piden confirmación explícita con el nombre del registro.
- El botón atrás nunca pierde datos sin avisar: si el formulario tiene cambios, se pregunta antes de salir.
- Textos en español de Bolivia (voseo opcional, siempre claros); montos como `Bs 1.250,00`.

## 2. Detalle por pantalla

### 2.1 Inicio

- **Archivo:** `app/index.tsx`
- **Qué muestra:** Ventas de hoy, ganancia estimada y productos por agotarse
- **Acciones:** Nueva venta, escanear

**Bosquejo**

```
┌──────────────────────────────┐
│ StockScan                    │
├──────────────────────────────┤
│ [ Resumen y alertas ]        │
│                              │
│  ▸ Productos                │
│  ▸ Escáner                  │
│  ▸ Entradas                 │
│  ▸ Alertas                  │
│                              │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Ventas de hoy, ganancia estimada y productos por agotarse |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.2 Productos

- **Archivo:** `app/productos.tsx`
- **Qué muestra:** Lista con búsqueda por nombre o código y filtro por categoría
- **Acciones:** Crear, editar, escanear
- **Historias relacionadas:** HU-01, HU-02, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Productos                   │
├──────────────────────────────┤
│ codigo · costo               │
│ codigo · costo               │
│ codigo · costo               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay productos. Tocá «Crear» para empezar. |
| Con datos | Lista con búsqueda por nombre o código y filtro por categoría |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.3 Formulario de producto

- **Archivo:** `app/formulario-de-producto.tsx`
- **Qué muestra:** Código, nombre, categoría, costo, precio, stock, mínimo
- **Acciones:** Guardar, escanear código
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Campos del formulario**

| Campo | Control | Obligatorio | Validación |
|---|---|---|---|
| codigo | Texto / escáner | No | Sin espacios; máximo 64 caracteres; Único en la tabla |
| nombre | Texto | Sí | Texto de 1 a 80 caracteres sin espacios sobrantes |
| categoria | Texto | No | Texto de 1 a 80 caracteres sin espacios sobrantes |
| costo | Numérico decimal | Sí | Mayor o igual a 0; Hasta 2 decimales |
| precio | Numérico decimal | Sí | Mayor a 0; Hasta 2 decimales |
| stock | Numérico entero | Sí | Entero mayor o igual a 0 |
| minimo | Numérico entero | Sí | Entero mayor o igual a 0 |
| activo | Interruptor | Sí | 0 o 1 |

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Formulario de producto      │
├──────────────────────────────┤
│ codigo                       │
│ [____________________]       │
│ nombre                       │
│ [____________________]       │
│ categoria                    │
│ [____________________]       │
│ costo                        │
│ [____________________]       │
│ precio                       │
│ [____________________]       │
│                              │
│ [        Guardar        ]    │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay productos. Tocá «Guardar» para empezar. |
| Con datos | Código, nombre, categoría, costo, precio, stock, mínimo |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.4 Escáner

- **Archivo:** `app/escaner.tsx`
- **Qué muestra:** Vista de cámara con detección de códigos de barras
- **Acciones:** Escanear, linterna, ingresar manual
- **Historias relacionadas:** HU-01, HU-02 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Escáner                     │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Vista de cámara con detección de códigos de barras |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.5 Nueva venta

- **Archivo:** `app/nueva-venta.tsx`
- **Qué muestra:** Líneas escaneadas con cantidad editable y total
- **Acciones:** Confirmar, cancelar
- **Historias relacionadas:** HU-02, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Campos del formulario**

| Campo | Control | Obligatorio | Validación |
|---|---|---|---|
| fecha | Fecha | Sí | Fecha real en formato AAAA-MM-DD |
| total | Numérico decimal | Sí | Mayor o igual a 0; Hasta 2 decimales |

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Nueva venta                 │
├──────────────────────────────┤
│ fecha                        │
│ [____________________]       │
│ total                        │
│ [____________________]       │
│                              │
│ [        Guardar        ]    │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay ventas. Tocá «Confirmar» para empezar. |
| Con datos | Líneas escaneadas con cantidad editable y total |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.6 Entradas

- **Archivo:** `app/entradas.tsx`
- **Qué muestra:** Producto, cantidad y costo opcional
- **Acciones:** Guardar
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Entradas                    │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Producto, cantidad y costo opcional |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.7 Alertas

- **Archivo:** `app/alertas.tsx`
- **Qué muestra:** Productos bajo mínimo ordenados por urgencia
- **Acciones:** Registrar entrada
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Alertas                     │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Productos bajo mínimo ordenados por urgencia |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.8 Cierre del día

- **Archivo:** `app/cierre-del-dia.tsx`
- **Qué muestra:** Total vendido, efectivo contado y diferencia
- **Acciones:** Cerrar, ver historial
- **Historias relacionadas:** HU-04, HU-05 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Cierre del día              │
├──────────────────────────────┤
│ fecha · fecha                │
│ fecha · fecha                │
│ fecha · fecha                │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay cierres. Tocá «Cerrar» para empezar. |
| Con datos | Total vendido, efectivo contado y diferencia |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.9 Exportar

- **Archivo:** `app/exportar.tsx`
- **Qué muestra:** Opciones de CSV
- **Acciones:** Exportar y compartir
- **Historias relacionadas:** HU-05, HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Exportar                    │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Opciones de CSV |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

## 3. Flujos principales

**HU-01 — Cargar el catálogo de productos**

```
escaneo un código con la cámara  →  el campo código se completa solo
```

**HU-02 — Vender escaneando**

```
escaneo 3 productos  →  se arma la lista con el total y al confirmar baja el stock
```

**HU-03 — Registrar entradas de mercadería**

```
registro una entrada de 24  →  el stock pasa a 27 y queda un movimiento de entrada
```

**HU-04 — Alertas de stock bajo**

```
abro Alertas  →  aparece en la lista de reposición
```

**HU-05 — Cierre de caja diario**

```
ingreso efectivo contado 1.240 Bs  →  el cierre muestra una diferencia de −10 Bs
```

**HU-06 — Exportar a CSV**

```
toco «Exportar inventario»  →  se genera un CSV con una fila por producto
```
