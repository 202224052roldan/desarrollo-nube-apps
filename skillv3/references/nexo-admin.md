# Nexo para aplicaciones integradas en el Administrador

## Ámbito

Usar `@tiendanube/nexo` únicamente para comunicar una app iframe con el Administrador. No usarlo para inyectar UI en storefront o checkout.

## Pila

- React para el frontend integrado.
- Nimbus para componentes, plantillas y UX/UI.
- Nexo para mensajes, sesión, navegación y acciones.
- Backend propio para OAuth, secretos, API y persistencia.

## Inicio

1. Crear una sola instancia Nexo con el `clientId`.
2. Mantener `log: false` fuera de una depuración segura.
3. Ejecutar `connect(nexo)`.
4. Preparar la suscripción de navegación requerida.
5. Ejecutar `iAmReady(nexo)`.
6. Procesar `ACTION_NAVIGATE_SYNC` y sincronizar la ruta con `syncPathname`.
7. Obtener `getStoreInfo` para presentación y `getSessionToken` para autenticar el backend.
8. Renderizar la app solo cuando el Admin confirme conexión.

La documentación oficial muestra el cliente mediante el export por defecto y helpers en `@tiendanube/nexo/helpers`. Verificar los exports reales de la versión instalada antes de fijar imports.

## Sesión

Enviar el token de sesión al backend:

```http
Authorization: Bearer {nexo_session_jwt}
```

Validar en el backend firma, algoritmo permitido, expiración, audience y `storeId`. Resolver el tenant desde esa identidad; no usar un `store_id` proporcionado por el frontend.

Mantener el JWT en memoria cuando sea posible. No almacenarlo en URL, logs, analytics o almacenamiento persistente.

## StoreInfo

Contemplar al menos:

- `id`
- `name`
- `url`
- `country`
- `language`
- `currency`

Usar estos valores para UI y localización. No sustituir con ellos la validación del tenant en el backend.

## Manejo de errores

Envolver la raíz React con `ErrorBoundary`. Tratarlo como requisito de publicación, no como mejora opcional.

Modelar:

- `loading`
- `ready`
- `nexo-error`
- `session-error`

Mostrar skeleton durante conexión y ofrecer reintento solo para fallos recuperables.

## Helpers condicionales

Consultar la documentación cuando se requiera:

- `navigateExit`
- `goTo`
- `goToOldAdmin`
- `navigateHeader` / `navigateHeaderRemove`
- `getIsMobileDevice`
- `copyToClipboard`
- `getFeatureStatus`
- `runWithUpsell`

No implementar acciones no utilizadas.

## Verificación

- Probar dentro del Admin, no solo en navegador independiente.
- Confirmar sincronización de rutas, navegación atrás y recarga profunda.
- Probar escritorio y móvil; algunos helpers solo funcionan en web.
- Verificar que errores de React activen el fallback del Admin.
- Comprobar que tokens no aparecen en consola.
- Validar Nimbus, responsive, estados vacío/error/carga y accesibilidad.

## Fuentes oficiales

- [Nexo](https://dev.nuvemshop.com.br/es/docs/developer-tools/nexo)
- [Aplicaciones integradas](https://dev.nuvemshop.com.br/es/docs/applications/native)
- [Primeros pasos](https://dev.nuvemshop.com.br/es/docs/getting-started)
- [Requisitos de homologación](https://dev.nuvemshop.com.br/es/docs/homologation/checklist)
