# NubeSDK para storefront y checkout

## Modelo

Ejecutar la app dentro de un Web Worker aislado. No acceder a:

- `document`
- `window`
- DOM directo o `innerHTML`
- jQuery
- React, Vue o Angular para renderizado DOM
- `localStorage` o `sessionStorage` síncronos

Usar eventos, estado, componentes, UI Slots y Browser APIs de NubeSDK.

## Inicio

Crear proyectos nuevos con:

```sh
npm create nube-app@latest
```

Exportar el entry point:

```ts
import type { NubeSDK } from "@tiendanube/nube-sdk-types";

export function App(nube: NubeSDK) {
  // registrar eventos, configuración y UI
}
```

## API esencial

- `nube.on(event, listener)`: escuchar cambios.
- `nube.off(event, listener)`: retirar el mismo listener.
- `nube.send(event, modifier)`: enviar acciones o modificar estado.
- `nube.getState()`: leer estado inmutable actual.
- `nube.render(slot, component)`: renderizar componentes declarativos.
- `nube.clearSlot(slot)`: limpiar un slot.
- `nube.getBrowserAPIs()`: almacenamiento asíncrono, navegación y capacidades permitidas.

## Patrones

- Reaccionar a `location:updated` en storefront y `checkout:ready` en checkout.
- Reaccionar al efecto de una interacción, no a clicks del DOM.
- Usar `cart:update`, `shipping:update`, `payment:update`, `customer:update` y `order:update` según la superficie.
- Habilitar `has_cart_validation` antes de enviar `cart:validate`.
- Renderizar únicamente en slots documentados.
- Usar componentes JSX de NubeSDK o la API declarativa.
- Usar `styled`, `StyleSheet.create` y tokens del tema.
- Usar `fetch` dentro del Worker con endpoints controlados.
- Sustituir almacenamiento por `asyncLocalStorage` o `asyncSessionStorage`.
- Navegar mediante Browser APIs; la navegación directa solo funciona en el dominio admitido.

## Migración desde scripts legados

Reemplazar:

| Legado | NubeSDK |
|---|---|
| IIFE / `DOMContentLoaded` | `App(nube)` |
| `window.location` | eventos de ubicación y Browser APIs |
| listeners DOM | eventos de comercio |
| `createElement` / HTML inyectado | `nube.render` + slots |
| lectura visual del carrito | `nube.getState()` / `cart:update` |
| `<style>` inyectado | styling y theme tokens |
| localStorage síncrono | almacenamiento asíncrono |

## DevTools y pruebas

- Instalar NubeSDK DevTools.
- Usar Local Mode con `npm run dev` y el script local.
- Inspeccionar Apps, Components, Events, Storage y State.
- Probar storefront y checkout en los temas y geografías objetivo.
- Confirmar ausencia de DOM, `window`, jQuery y frameworks DOM.
- Confirmar el flag “Uses NubeSDK” en el Portal de Socios.

## Gate específico

No considerar lista una app NubeSDK hasta comprobar:

- `App(nube)` exportado;
- UI solo mediante slots;
- eventos y estado tipados;
- almacenamiento y navegación mediante Browser APIs;
- comportamiento en todas las superficies declaradas;
- DevTools reconoce la app;
- ningún acceso legado al DOM;
- script productivo compilado y accesible.

## Fuentes oficiales

- [NubeSDK Overview](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/overview)
- [Getting Started](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/getting-started)
- [Script Structure](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/script-structure)
- [Events](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/events/overview)
- [Browser APIs](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/browser-apis)
- [UI Slots](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/slots/overview)
- [DevTools](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/devtools)
- [Migración](https://dev.nuvemshop.com.br/es/docs/applications/nube-sdk/migration-guide)
