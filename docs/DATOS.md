# Modelo de datos — StockScan

> Diseño detallado de la base SQLite: campos, validaciones, consultas y migraciones. Complementa la sección 6 de [SPEC.md](SPEC.md).

## 1. Convenciones

- Base local `SQLite` con `expo-sqlite`; versión del esquema en `PRAGMA user_version`.
- Claves primarias `INTEGER AUTOINCREMENT` llamadas `id`.
- Fechas como texto ISO `AAAA-MM-DD` (y `AAAA-MM-DDTHH:MM` cuando hay hora). Nunca se guardan textos como «hoy».
- Montos como `REAL` en bolivianos; se redondean a 2 decimales al guardar y se formatean al mostrar.
- Booleanos como `INTEGER` 0/1.
- Los textos se guardan sin espacios sobrantes (`trim()`).
- Todas las consultas usan parámetros (`?`), nunca concatenan texto del usuario.
- Las claves foráneas se activan con `PRAGMA foreign_keys = ON` al abrir la base.

## 2. Relaciones

| Campo | Apunta a | Comportamiento |
|---|---|---|
| `movimientos.producto_id` | `productos` | Sin borrado en cascada |
| `movimientos.venta_id` | `ventas` | Sin borrado en cascada |

```
productos  ──<  movimientos
ventas  ──<  movimientos
movimientos
cierres
```

## 3. Detalle por tabla

### 3.1 `productos`

Catálogo con código de barras, costo, precio, stock y mínimo (se agrega categoría y estado activo respecto al README).

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `codigo` | TEXT → `string \| null` | Texto / escáner | Opcional; Sin espacios; máximo 64 caracteres; Único en la tabla | «Código no válido» |
| `nombre` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí nombre» |
| `categoria` | TEXT → `string \| null` | Texto | Opcional; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí categoria» |
| `costo` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales; Valor inicial `0` | «Ingresá un número válido» |
| `precio` | REAL → `number` | Numérico decimal | Obligatorio; Mayor a 0; Hasta 2 decimales | «Ingresá un monto mayor a 0» |
| `stock` | INTEGER → `number` | Numérico entero | Obligatorio; Entero mayor o igual a 0; Valor inicial `0` | «Ingresá un número entero» |
| `minimo` | INTEGER → `number` | Numérico entero | Obligatorio; Entero mayor o igual a 0; Valor inicial `0` | «Ingresá un número entero» |
| `activo` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `1` | — |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_productos_codigo ON productos(codigo);
```

**Consultas base** (en `src/db/productos.ts`):

```ts
listar(): Promise<Producto[]>            // SELECT * FROM productos ORDER BY id DESC
obtener(id: number): Promise<Producto | null>   // SELECT * FROM productos WHERE id = ?
crear(datos: NuevoProducto): Promise<number>   // INSERT INTO productos (codigo, nombre, categoria, costo, precio, stock, minimo, activo) VALUES (?, ?, ?, ?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoProducto): Promise<void>   // UPDATE productos SET codigo = ?, nombre = ?, categoria = ?, costo = ?, precio = ?, stock = ?, minimo = ?, activo = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM productos WHERE id = ?
```

### 3.2 `ventas`

Cabecera de cada venta con su total.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `total` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_ventas_fecha ON ventas(fecha);
```

**Consultas base** (en `src/db/ventas.ts`):

```ts
listar(): Promise<Venta[]>            // SELECT * FROM ventas ORDER BY id DESC
obtener(id: number): Promise<Venta | null>   // SELECT * FROM ventas WHERE id = ?
crear(datos: NuevoVenta): Promise<number>   // INSERT INTO ventas (fecha, total) VALUES (?, ?)
actualizar(id: number, datos: NuevoVenta): Promise<void>   // UPDATE ventas SET fecha = ?, total = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM ventas WHERE id = ?
```

### 3.3 `movimientos`

Cada entrada, salida por venta o ajuste de stock.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `producto_id` | INTEGER → `number` | Selector de productos | Obligatorio; Debe existir en `productos` | «Elegí un registro» |
| `tipo` | TEXT → `string` | Selector / fichas | Obligatorio; Uno de: `entrada`, `venta`, `ajuste` | «Elegí una opción» |
| `cantidad` | INTEGER → `number` | Numérico entero | Obligatorio; Entero mayor o igual a 0 | «Ingresá un número entero» |
| `precio_unitario` | REAL → `number \| null` | Numérico decimal | Opcional; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |
| `venta_id` | INTEGER → `number \| null` | Selector de ventas | Opcional; Debe existir en `ventas` | «Elegí un registro» |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_movimientos_producto_id ON movimientos(producto_id);
CREATE INDEX IF NOT EXISTS idx_movimientos_venta_id ON movimientos(venta_id);
CREATE INDEX IF NOT EXISTS idx_movimientos_fecha ON movimientos(fecha);
```

**Consultas base** (en `src/db/movimientos.ts`):

```ts
listar(): Promise<Movimiento[]>            // SELECT * FROM movimientos ORDER BY id DESC
obtener(id: number): Promise<Movimiento | null>   // SELECT * FROM movimientos WHERE id = ?
crear(datos: NuevoMovimiento): Promise<number>   // INSERT INTO movimientos (producto_id, tipo, cantidad, precio_unitario, venta_id, fecha) VALUES (?, ?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoMovimiento): Promise<void>   // UPDATE movimientos SET producto_id = ?, tipo = ?, cantidad = ?, precio_unitario = ?, venta_id = ?, fecha = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM movimientos WHERE id = ?
listarPorProducto(producto_id: number): Promise<Movimiento[]>   // WHERE producto_id = ?
listarPorVenta(venta_id: number): Promise<Movimiento[]>   // WHERE venta_id = ?
```

### 3.4 `cierres`

Cierre diario con total esperado, efectivo contado y diferencia.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD; Único en la tabla | «Fecha no válida» |
| `total` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |
| `efectivo` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |
| `diferencia` | REAL → `number` | Numérico decimal | Obligatorio; Mayor o igual a 0; Hasta 2 decimales; Valor inicial `0` | «Ingresá un número válido» |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_cierres_fecha ON cierres(fecha);
```

**Consultas base** (en `src/db/cierres.ts`):

```ts
listar(): Promise<Cierr[]>            // SELECT * FROM cierres ORDER BY id DESC
obtener(id: number): Promise<Cierr | null>   // SELECT * FROM cierres WHERE id = ?
crear(datos: NuevoCierr): Promise<number>   // INSERT INTO cierres (fecha, total, efectivo, diferencia) VALUES (?, ?, ?, ?)
actualizar(id: number, datos: NuevoCierr): Promise<void>   // UPDATE cierres SET fecha = ?, total = ?, efectivo = ?, diferencia = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM cierres WHERE id = ?
```

## 4. Tipos TypeScript

Copiar a `src/db/tipos.ts`:

```ts
export interface Producto {
  id: number;
  codigo: string | null;
  nombre: string;
  categoria: string | null;
  costo: number;
  precio: number;
  stock: number;
  minimo: number;
  activo: number;
}
export type NuevoProducto = Omit<Producto, 'id'>;

export interface Venta {
  id: number;
  fecha: string;
  total: number;
}
export type NuevoVenta = Omit<Venta, 'id'>;

export interface Movimiento {
  id: number;
  producto_id: number;
  tipo: string;
  cantidad: number;
  precio_unitario: number | null;
  venta_id: number | null;
  fecha: string;
}
export type NuevoMovimiento = Omit<Movimiento, 'id'>;

export interface Cierr {
  id: number;
  fecha: string;
  total: number;
  efectivo: number;
  diferencia: number;
}
export type NuevoCierr = Omit<Cierr, 'id'>;

```

## 5. Valores permitidos (enumeraciones)

SQLite no tiene tipo enumerado: se valida en la capa de datos y se muestra con la etiqueta de la tabla.

**`movimientos.tipo`**

| Valor guardado | Etiqueta en pantalla |
|---|---|
| `entrada` | Entrada |
| `venta` | Venta |
| `ajuste` | Ajuste |

```ts
export const MOVIMIENTOS_TIPO = ['entrada', 'venta', 'ajuste'] as const;
```

## 6. Migraciones

Migración 1 = esquema inicial (ya en `src/db/schema.ts` tras la fase 1). Cada cambio posterior es una migración nueva al final de la lista; nunca se edita una ya publicada.

No se prevén migraciones adicionales dentro del MVP.

## 7. Esquema inicial completo

```sql
CREATE TABLE productos (id INTEGER PRIMARY KEY AUTOINCREMENT, codigo TEXT UNIQUE, nombre TEXT NOT NULL, categoria TEXT, costo REAL NOT NULL DEFAULT 0, precio REAL NOT NULL, stock INTEGER NOT NULL DEFAULT 0, minimo INTEGER NOT NULL DEFAULT 0, activo INTEGER NOT NULL DEFAULT 1);
CREATE TABLE ventas (id INTEGER PRIMARY KEY AUTOINCREMENT, fecha TEXT NOT NULL, total REAL NOT NULL);
CREATE TABLE movimientos (id INTEGER PRIMARY KEY AUTOINCREMENT, producto_id INTEGER NOT NULL REFERENCES productos(id), tipo TEXT NOT NULL, cantidad INTEGER NOT NULL, precio_unitario REAL, venta_id INTEGER REFERENCES ventas(id), fecha TEXT NOT NULL);
CREATE TABLE cierres (id INTEGER PRIMARY KEY AUTOINCREMENT, fecha TEXT NOT NULL UNIQUE, total REAL NOT NULL, efectivo REAL NOT NULL, diferencia REAL NOT NULL DEFAULT 0);
```

## 8. Reglas de integridad que NO delega a la base

- **RN-01** El código de barras es único por producto; productos sin código se identifican por nombre.
- **RN-02** El stock de un producto es la suma de sus movimientos: entradas y ajustes positivos suman, ventas y ajustes negativos restan.
- **RN-03** Una venta descuenta stock; si el stock llegaría a negativo se avisa y se permite continuar con confirmación (ajuste posterior).
- **RN-04** Un producto está «por agotarse» cuando stock ≤ mínimo.
- **RN-05** Total de venta = suma de cantidad × precio unitario del momento (el precio queda guardado en el movimiento).
- **RN-06** Margen = precio − costo; ganancia del día = suma de (precio − costo) × cantidad vendida.
- **RN-07** Solo puede haber un cierre por día; la diferencia = efectivo contado − total vendido en efectivo.

Estas reglas se implementan en `src/lib/` como funciones puras y se prueban de forma automática (ver [PRUEBAS.md](PRUEBAS.md)).
