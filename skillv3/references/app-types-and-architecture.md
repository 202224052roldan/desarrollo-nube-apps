# Tipos de aplicación y arquitectura

## Selección por superficie

| Superficie | Tecnología principal | Restricciones |
|---|---|---|
| Administrador integrado | React, Nimbus y Nexo | UI pública cargada en iframe; comunicación con Admin mediante Nexo |
| Aplicación externa | Framework libre, OAuth y API | Se ejecuta fuera del Administrador; Nexo no es obligatorio |
| Storefront o checkout | NubeSDK | Web Worker, eventos, estado, componentes y UI Slots; sin DOM directo |
| Híbrida | Combinación explícita | Mantener Admin/Nexo, NubeSDK y backend como unidades separadas |

No elegir el SDK por preferencia tecnológica. Elegirlo según el lugar donde se ejecuta la funcionalidad.

## Aplicación integrada en el Administrador

- Servir una URL pública mediante TLS.
- Cargarla en un iframe administrado por Tiendanube.
- Usar React porque Nimbus está optimizado para esa pila.
- Usar Nimbus para coherencia visual, accesibilidad, estados y componentes.
- Usar Nexo para conexión, sesión, navegación y acciones del Administrador.
- Implementar CSP con `frame-ancestors` limitado a los orígenes de Tiendanube/Nuvemshop requeridos.

## Aplicación externa

- Completar OAuth y operar con la API desde el backend.
- No asumir la disponibilidad de un JWT de Nexo.
- Diseñar registro, login, onboarding y soporte sin obligar al merchant a introducir manualmente la URL de su tienda.
- Considerar Nimbus como recomendación visual, no como sustituto de la arquitectura.

## Storefront y checkout

Usar NubeSDK cuando la app:

- inserte UI en la tienda o checkout;
- escuche o modifique carrito, cupones, shipping, payment u order;
- reaccione al estado del storefront;
- reemplace scripts legados que usaban DOM, `window`, `document` o jQuery.

No usar Nexo para esta superficie.

## Arquitectura híbrida recomendada

```text
Tiendanube Admin
  └─ iframe React + Nimbus + Nexo
       └─ Backend multi-tenant
            ├─ OAuth y tokens cifrados
            ├─ API Tiendanube
            ├─ webhooks y privacidad
            └─ datos de la app

Storefront / Checkout
  └─ NubeSDK App(nube) dentro de Web Worker
       ├─ eventos y estado
       ├─ componentes y UI Slots
       └─ fetch controlado al backend de la app
```

No compartir secretos con la parte NubeSDK ni con el iframe.

## Distribución y pruebas

- Crear la app en el Portal de Socios.
- Elegir “Tienda de Aplicaciones” cuando será pública y homologada.
- Elegir “Para sus clientes” cuando la distribución será limitada y no requiera App Store.
- Crear y usar una tienda demo para instalación y pruebas.
- Solicitar Modo de Desarrollador cuando sea necesario cargar una app integrada en el Admin.
- Usar NubeSDK DevTools y Local Mode para storefront/checkout.

## Plazos NubeSDK

- Desde el 5 de junio de 2026: NubeSDK es requisito de nuevas homologaciones cuando la app está dentro de su ámbito.
- Desde el 30 de agosto de 2026: nuevas instalaciones de apps afectadas sin NubeSDK quedan bloqueadas.
- Desde el 30 de octubre de 2026: comienza la deprecación/desinstalación progresiva del modelo legado.

## Fuentes oficiales

- [Primeros pasos](https://dev.nuvemshop.com.br/es/docs/getting-started)
- [Aplicaciones: visión general](https://dev.nuvemshop.com.br/es/docs/applications/overview)
- [Aplicaciones integradas](https://dev.nuvemshop.com.br/es/docs/applications/native)
- [Aplicaciones externas](https://dev.nuvemshop.com.br/es/docs/applications/standalone)
- [NubeSDK: visión general](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/overview)
