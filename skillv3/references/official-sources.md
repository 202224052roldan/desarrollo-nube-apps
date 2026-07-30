# Índice de fuentes oficiales

Usar este archivo para verificar cambios y procedencia. Preferir siempre la página específica del recurso.

## Inicio y aplicaciones

- [Primeros pasos](https://dev.nuvemshop.com.br/es/docs/getting-started)
- [Aplicaciones: visión general](https://dev.nuvemshop.com.br/es/docs/applications/overview)
- [Autenticación](https://dev.nuvemshop.com.br/es/docs/applications/authentication)
- [Integradas](https://dev.nuvemshop.com.br/es/docs/applications/native)
- [Externas](https://dev.nuvemshop.com.br/es/docs/applications/standalone)
- [Directrices de publicación](https://dev.nuvemshop.com.br/es/docs/applications/guidelines)
- [Landing Page](https://dev.nuvemshop.com.br/es/docs/applications/landing-page)
- [Patrones HTTP](https://dev.nuvemshop.com.br/es/docs/applications/http-status)

## Herramientas

- [Nexo](https://dev.nuvemshop.com.br/es/docs/developer-tools/nexo)
- [API Tiendanube](https://dev.nuvemshop.com.br/es/docs/developer-tools/nuvemshop-api)
- [API completa](https://tiendanube.github.io/api-documentation/)
- [Autenticación API completa](https://tiendanube.github.io/api-documentation/authentication)
- [Edición de pedidos](https://dev.nuvemshop.com.br/es/docs/developer-tools/edit-order)

## NubeSDK

- [Overview](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/overview)
- [Getting Started](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/getting-started)
- [DevTools](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/devtools)
- [Script Structure](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/script-structure)
- [Events](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/events/overview)
- [Browser APIs](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/browser-apis)
- [Components](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/components/overview)
- [UI Slots](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/slots/overview)
- [Migration Guide](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/migration-guide)

## Homologación

- [Visión general](https://dev.nuvemshop.com.br/es/docs/homologation/overview)
- [Buenas prácticas](https://dev.nuvemshop.com.br/es/docs/homologation/guidelines)
- [Requisitos obligatorios](https://dev.nuvemshop.com.br/es/docs/homologation/requirements)
- [Checklist](https://dev.nuvemshop.com.br/es/docs/homologation/checklist)

## Contradicciones conocidas

### Encabezado de API

Las skills heredadas usan `Authentication: bearer`. El DevHub y la documentación API actuales muestran `Authorization: Bearer`. Usar la forma actual en integraciones nuevas.

### Refresh token

La guía específica de autenticación declara access tokens sin expiración y muestra una respuesta sin `refresh_token`. Una guía general de publicación menciona `refresh_token`. No implementar refresh automático hasta que el endpoint o un contrato oficial específico lo confirme.

### Nexo y NubeSDK

Nexo corresponde al iframe del Admin. NubeSDK corresponde a storefront/checkout. Una app híbrida puede necesitar ambos, pero en procesos distintos.
