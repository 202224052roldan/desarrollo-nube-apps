# Webhooks y privacidad

## Uso

Preferir webhooks cuando la app necesita reaccionar a cambios. No hacer GET continuo para detectar modificaciones.

Registrar solo eventos necesarios y permitidos por los scopes.

## Seguridad de recepción

1. Leer el cuerpo crudo sin transformarlo.
2. Leer la firma del encabezado documentado.
3. Calcular HMAC-SHA256 con el secreto de la app cuando ese sea el esquema del evento.
4. Comparar en tiempo constante.
5. Rechazar firmas ausentes o inválidas antes de parsear JSON.
6. Validar después esquema, evento y tienda.
7. Aplicar la acción de forma idempotente.

No aceptar webhooks sin verificar cuando falte el secreto en producción.

## Eventos de ciclo de vida y privacidad

Contemplar:

- `app/uninstalled`: desactivar la instalación y dejar de operar para la tienda.
- store redact: eliminar token, configuración y todos los datos de la tienda.
- customer redact: eliminar datos personales del cliente cuando se almacenen.
- customer data request: producir la respuesta de datos requerida cuando aplique.

Configurar las URLs de privacidad en el Portal de Socios. No asumir que todos esos eventos se registran mediante el mismo endpoint de webhooks.

## Entrega y procesamiento

- Tolerar campos desconocidos.
- Deduplicar por ID de entrega o clave de negocio cuando exista.
- Separar recepción autenticada de procesamiento pesado mediante una cola si el flujo lo necesita.
- Registrar `Request-ID`, evento, tienda y resultado sin almacenar datos sensibles innecesarios.
- Definir reintentos internos y dead-letter queue para fallos posteriores al acuse.
- No efectuar mutaciones dobles ante reentregas.

Verificar la política de respuesta del evento concreto. No aplicar ciegamente “siempre 200” a todos los endpoints propios; mantener semántica HTTP correcta y evitar bucles de reintento.

## Reinstalación

- Hacer upsert de la tienda.
- Reemplazar el token cifrado.
- Reactivar la instalación.
- Tolerar registros de webhook ya existentes.
- Reconciliar suscripciones necesarias.

## Fuentes oficiales

- [Buenas prácticas de homologación](https://dev.nuvemshop.com.br/es/docs/homologation/guidelines)
- [Autenticación y URLs de privacidad](https://tiendanube.github.io/api-documentation/authentication)
- [Directrices de publicación](https://dev.nuvemshop.com.br/es/docs/applications/guidelines)
- [Documentación API](https://tiendanube.github.io/api-documentation/)
