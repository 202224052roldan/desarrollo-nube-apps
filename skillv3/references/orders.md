# Pedidos, ERP y EditOrders

Leer esta referencia solo cuando la app consulte, sincronice o edite pedidos, o integre ERP, shipping, fulfillment o pagos.

## Sincronización

- Usar webhooks para detectar cambios.
- Ante `order/edited`, volver a consultar el pedido y reconciliar los campos modificados.
- Contemplar que también puede emitirse `order/updated`.
- Mantener consistencia entre Tiendanube, ERP, logística y pagos.
- Evitar suponer que el payload del webhook contiene todo el estado actualizado.

## Edición

- Usar el endpoint documentado `POST /orders/{id}/edit` cuando el ERP inicia una edición.
- Permitir edición únicamente cuando el Fulfillment Order esté `UNPACKED`.
- Recordar que un pedido `PACKED` puede volver a `UNPACKED` desde el Admin, pero uno `DISPATCHED` ya no.
- No eliminar todos los productos de un Fulfillment Order.
- Contemplar adición/eliminación de productos, descuentos, destino y cambios de método de pago admitidos.

## Efectos secundarios

Una modificación de productos vuelve a cotizar el envío. Si la app ofrece shipping prepago, fulfillment o recolección:

- escuchar los eventos de edición;
- invalidar cálculos obsoletos;
- reconciliar importes y servicios;
- impedir despachos con datos anteriores.

## Pruebas

- Tiendanube edita y el ERP recibe/reconcilia.
- ERP edita y Tiendanube refleja el resultado.
- Pedido `UNPACKED` permitido.
- Pedido `PACKED`/`DISPATCHED` rechazado según la regla.
- Ruptura de stock sin dejar un fulfillment vacío.
- Recalculo de shipping y manejo de duplicados.

## Fuente oficial

- [Edición de Pedidos](https://dev.nuvemshop.com.br/es/docs/developer-tools/edit-order)
