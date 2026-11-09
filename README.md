<div align="center">

<img src="docs/assets/logo.svg" alt="SocioFit" width="96" height="96"/>

# SocioFit

**Control de membresías para gimnasios y academias, con carnet QR, vencimientos y recordatorios por WhatsApp. Offline-first, sin costos mensuales.**

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

- **Socios y planes**: alta de socios con foto, plan mensual/trimestral/anual y precio libre.
- **Carnet QR**: cada socio tiene un QR para registrar asistencia en la puerta.
- **Vencimientos**: panel con socios por vencer, vencidos y al día.
- **Recordatorios locales**: notificaciones en el dispositivo, sin servidor.
- **Cobro por WhatsApp**: mensaje de pago pre-llenado con un toque.
- **Caja diaria**: pagos del día y resumen mensual.

> Sin servidores, sin suscripciones y sin servicios de pago: todos los datos viven en el dispositivo (SQLite).

## 🧰 Stack

| Capa | Tecnología |
| --- | --- |
| App | React Native + Expo (TypeScript) |
| Datos | SQLite local (`expo-sqlite`) |
| Navegación | Expo Router |
| Distribución | APK / EAS Build local |

### Modelo de datos

- `socios (id, nombre, telefono, foto, creado_en)`
- `planes (id, nombre, dias, precio)`
- `membresias (id, socio_id, plan_id, inicio, vence, pagado)`
- `asistencias (id, socio_id, fecha_hora)`

## 🗺️ Roadmap

- [ ] 1. Estructura base y navegación
- [ ] 2. Socios, planes y membresías
- [ ] 3. Escaneo QR y asistencias
- [ ] 4. Recordatorios y WhatsApp
- [ ] 5. Respaldo/exportación local

## 📂 Estructura

```text
SocioFit/
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
git clone https://github.com/Luiss2080/SocioFit.git
cd SocioFit
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
