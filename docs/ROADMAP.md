# Roadmap de StockScan

> Que el dueño de la tienda sepa cuánto stock tiene, qué se va a agotar y cuánto debería haber en caja al cerrar el día.

Ruta de desarrollo del MVP, fase por fase. Cada fase termina con algo que se puede **usar y probar en un teléfono**. Trabajá una fase a la vez y no pases a la siguiente hasta cumplir su criterio de salida.

## Resumen

- **Objetivo del MVP:** Inventario con código de barras, stock mínimo y cierre de caja diario para tiendas pequeñas.
- **Usuario:** Dueño o encargado de una tienda de barrio, kiosco o minimarket con 100 a 2000 productos.
- **Duración estimada:** ~20 días de trabajo efectivo (~4 semanas a ritmo tranquilo).
- **Costo:** $0 (sin servidores, sin suscripciones, todo offline en SQLite).

## Línea de tiempo

| Fase | Nombre | Objetivo | Días | Semana acumulada |
|---|---|---|---|---|
| 1 | Estructura base y navegación | Crear el proyecto Expo con base de datos, navegación, tema y CI. | 3 | 1 |
| 2 | Productos y escáner | Catálogo de productos y escáner. | 5 | 2 |
| 3 | Ventas y movimientos | Ventas y movimientos de stock. | 5 | 3 |
| 4 | Alertas y cierre diario | Alertas de stock y cierre diario. | 4 | 4 |
| 5 | Exportación CSV | Exportación a CSV. | 3 | 4 |

## Hitos

### Hito 1 — Estructura base y navegación

**Demo:** La app abre en Expo Go, navega entre secciones vacías y el CI pasa en verde.

### Hito 2 — Productos y escáner

**Demo:** Un producto se crea escaneando su código. Con el permiso denegado la app sigue usable con ingreso manual.

### Hito 3 — Ventas y movimientos

**Demo:** Una venta de 3 productos baja el stock correctamente y queda en historial.

### Hito 4 — Alertas y cierre diario

**Demo:** El cierre calcula la diferencia correcta en los casos de prueba.

### Hito 5 — Exportación CSV

**Demo:** Los CSV abren bien en Excel en español.

## Detalle por fase

### Fase 1 — Estructura base y navegación

- **Objetivo:** dejar el proyecto creado y funcionando con la base de datos y la navegación.
- **Historias cubiertas:** ninguna (infraestructura).
- **Depende de:** nada.
- **Criterio de salida:** la app abre, navega y el CI está en verde.

### Fase 2 — Productos y escáner

- **Objetivo:** Catálogo de productos y escáner.
- **Historias cubiertas:** HU-01 Cargar el catálogo de productos
- **Depende de:** fase 1.
- **Duración:** 5 días.
- **Criterio de salida:**
  - Un producto se crea escaneando su código.
  - Con el permiso denegado la app sigue usable con ingreso manual.

### Fase 3 — Ventas y movimientos

- **Objetivo:** Ventas y movimientos de stock.
- **Historias cubiertas:** HU-02 Vender escaneando; HU-03 Registrar entradas de mercadería
- **Depende de:** fase 2.
- **Duración:** 5 días.
- **Criterio de salida:**
  - Una venta de 3 productos baja el stock correctamente y queda en historial.

### Fase 4 — Alertas y cierre diario

- **Objetivo:** Alertas de stock y cierre diario.
- **Historias cubiertas:** HU-04 Alertas de stock bajo; HU-05 Cierre de caja diario
- **Depende de:** fase 3.
- **Duración:** 4 días.
- **Criterio de salida:**
  - El cierre calcula la diferencia correcta en los casos de prueba.

### Fase 5 — Exportación CSV

- **Objetivo:** Exportación a CSV.
- **Historias cubiertas:** HU-06 Exportar a CSV
- **Depende de:** fase 4.
- **Duración:** 3 días.
- **Criterio de salida:**
  - Los CSV abren bien en Excel en español.

## Estado

- [ ] **Fase 1:** Estructura base y navegación
- [ ] **Fase 2:** Productos y escáner
- [ ] **Fase 3:** Ventas y movimientos
- [ ] **Fase 4:** Alertas y cierre diario
- [ ] **Fase 5:** Exportación CSV

## Dependencias y supuestos

- Un teléfono Android real para probar (Expo Go) y, al final, para instalar el APK.
- Node.js 20+ y la app Expo Go.
- Todo lo demás (cuentas, nube, pagos) queda fuera del MVP.

## Después del MVP (v1.1 en adelante)

- [ ] Modo oscuro.
- [ ] Búsqueda y filtros avanzados en las listas.
- [ ] Estadísticas y gráficos simples en la pantalla de inicio.
- [ ] Publicación en Google Play (cuenta de desarrollador, pago único de USD 25).
- [ ] Respaldo y restauración de la base de datos.
- [ ] Sincronización opcional entre dispositivos (solo si hay demanda real).

## Cómo usar este documento

1. [SPEC.md](SPEC.md) define **qué** debe hacer la app (historias, reglas, pantallas, datos).
2. [PLANNING.md](PLANNING.md) define **cómo** construirla fase por fase (tareas, pruebas, issues).
3. Al cerrar una fase: marcá el estado acá, actualizá el [CHANGELOG](../CHANGELOG.md) y fusioná la rama a `main`.

## Trazabilidad: historias por fase

| Historia | Fase | Entrega |
|---|---|---|
| HU-01 Cargar el catálogo de productos | 2 | Productos y escáner |
| HU-02 Vender escaneando | 3 | Ventas y movimientos |
| HU-03 Registrar entradas de mercadería | 3 | Ventas y movimientos |
| HU-04 Alertas de stock bajo | 4 | Alertas y cierre diario |
| HU-05 Cierre de caja diario | 4 | Alertas y cierre diario |
| HU-06 Exportar a CSV | 5 | Exportación CSV |

## Indicadores de avance

| Indicador | Cómo se mide | Meta |
|---|---|---|
| Avance de tareas | Issues cerrados / 40 | 100 % al lanzar |
| Historias aceptadas | Casos CP- marcados ✔ / 11 | 100 % |
| Reglas cubiertas | RN con prueba / 7 | 100 % |
| Ritmo | Días reales vs estimados por fase | Desvío menor al 50 % |
| Calidad | Errores abiertos al cierre de fase | 0 críticos |

## Revisión de fin de fase

Antes de pasar a la siguiente fase respondé por escrito (5 minutos):

1. ¿Se cumplieron todos los criterios de salida?
2. ¿Cuántos días tomó realmente frente a lo estimado?
3. ¿Qué se recortó o se dejó para después?
4. ¿Hay algo que cambie el SPEC? Actualizalo antes de seguir.

## Documentos de apoyo

- [DATOS.md](DATOS.md): campos, validaciones, consultas y migraciones.
- [PANTALLAS.md](PANTALLAS.md): ruta, estados y bosquejo de cada pantalla.
- [PRUEBAS.md](PRUEBAS.md): casos de prueba por regla, historia y borde.
- [BACKLOG.md](BACKLOG.md): issues listos para GitHub con milestones y etiquetas.
