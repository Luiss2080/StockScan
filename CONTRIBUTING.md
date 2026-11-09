# Guía de contribución

¡Gracias por querer aportar a este proyecto!

## Cómo proponer cambios

1. Abre un issue describiendo el bug o la idea antes de empezar un cambio grande.
2. Haz un fork y crea una rama con un nombre claro (`fix/descripcion`, `feat/descripcion`).
3. Mantén el cambio pequeño y enfocado: una cosa por PR.
4. Cuando existan pruebas, ejecútalas con `npm test` y revisa el código con `npm run lint` antes de abrir el PR.
5. Corre `npm test` y confirma que todo pasa antes de abrir el PR.

## Estilo

- TypeScript y componentes funcionales de React Native (Expo).
- La app debe funcionar sin conexión: los datos se guardan en el dispositivo (SQLite).
- No agregues dependencias de pago ni servicios con suscripción.
- Nunca subas claves, tokens ni archivos `.env`.
- Mensajes de commit en español, con prefijo (`feat:`, `fix:`, `docs:`, `chore:`).

## Reportar problemas de seguridad

No abras un issue público: sigue la [política de seguridad](SECURITY.md).
