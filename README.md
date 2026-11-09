<div align="center">

<img src="docs/assets/logo.svg" alt="StockScan" width="96" height="96"/>

# StockScan

**Inventario con escáner de códigos de barras, alertas de stock mínimo y cierre de caja diario para tiendas pequeñas. Offline-first.**

<p>
<img src="https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white" alt="Expo"/>
<img src="https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React Native"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
<img src="https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite"/>
<img src="https://img.shields.io/badge/Licencia-MIT-green?style=for-the-badge" alt="MIT"/>
</p>

[🚀 Características](#-características) · [🧰 Stack](#-stack) · [📂 Estructura](#-estructura) · [🔧 Configuración](#-configuración) · [🧪 Pruebas](#-pruebas) · [🤝 Contribuir](#-contribuir) · [📄 Licencia](#-licencia)

</div>

---

## 🚀 Características

- **Escáner de códigos** con la cámara para vender y reponer.
- **Stock mínimo**: alertas de productos por agotarse.
- **Ventas rápidas** y entradas de mercadería.
- **Cierre de caja diario** con totales.
- **Catálogo** con costo, precio y categoría.
- **Exportación CSV** de inventario y ventas.

> Sin servidores, sin suscripciones y sin servicios de pago: todos los datos viven en el dispositivo (SQLite).

## 🧰 Stack

| Capa | Tecnología |
| --- | --- |
| App | React Native + Expo (TypeScript) |
| Datos | SQLite local (`expo-sqlite`) |
| Navegación | Expo Router |
| Distribución | APK / EAS Build local |

### Modelo de datos

- `productos (id, codigo, nombre, costo, precio, stock, minimo)`
- `movimientos (id, producto_id, tipo, cantidad, fecha)`
- `ventas (id, fecha, total)`
- `cierres (id, fecha, total, efectivo)`

## 🗺️ Roadmap

- [ ] 1. Estructura base y navegación
- [ ] 2. Productos y escáner
- [ ] 3. Ventas y movimientos
- [ ] 4. Alertas y cierre diario
- [ ] 5. Exportación CSV

## 📂 Estructura

```text
StockScan/
├── app/            # Pantallas y rutas (Expo Router)
├── src/
│   ├── db/         # Esquema y consultas SQLite
│   ├── components/ # Componentes reutilizables
│   └── lib/        # Utilidades
├── docs/assets/    # Logo y recursos del README
└── .github/        # Plantillas, CI y Dependabot
```

## 🔧 Configuración

Requisitos: Node.js 20+ y la app Expo Go (o un emulador).

```bash
git clone https://github.com/Luiss2080/StockScan.git
cd StockScan
npm install
npx expo start
```

> El repositorio está en fase inicial: la estructura y la documentación están listas y el código de la app se construye siguiendo el roadmap.

## 🧪 Pruebas

```bash
npm test
npx tsc --noEmit
```

## 🤝 Contribuir

Lee la [guía de contribución](CONTRIBUTING.md) y el [código de conducta](CODE_OF_CONDUCT.md). Para reportar vulnerabilidades consulta la [política de seguridad](SECURITY.md); para dudas, [SUPPORT.md](SUPPORT.md).

## 📄 Licencia

Distribuido bajo licencia MIT. Consulta [LICENSE](LICENSE).

---

<div align="center">

Hecho por [Luiss2080](https://github.com/Luiss2080)

</div>
