# OAuth y API Tiendanube

## OAuth

Usar el flujo restringido OAuth 2.0 Authorization Code.

1. Enviar al merchant a `https://www.tiendanube.com/apps/{app_id}/authorize`.
2. Generar un `state` ligado a la sesión para impedir CSRF.
3. Recibir `code` y `state` en el callback propio.
4. Comparar `state` en tiempo constante cuando corresponda y consumir el código una sola vez.
5. Intercambiarlo desde el backend antes de que expire; la documentación indica cinco minutos.
6. Exigir `access_token`, `user_id` y scopes coherentes antes de persistir.
7. Cifrar el token y asociarlo a la tienda.
8. Redirigir a un onboarding claro; no mostrar el código ni secretos al merchant.

```http
POST https://www.tiendanube.com/apps/authorize/token
Content-Type: application/json

{
  "client_id": "...",
  "client_secret": "...",
  "grant_type": "authorization_code",
  "code": "..."
}
```

Los access tokens normales de apps no tienen vencimiento documentado. Se invalidan al emitir uno nuevo o desinstalar la app. No implementar refresh token basándose únicamente en una guía general contradictoria.

## Scopes y reinstalación

- Solicitar solo scopes necesarios para la funcionalidad demostrable.
- Alinear scopes del Portal, consentimiento, código y diagrama de secuencia.
- Recordar que un scope de escritura implica su lectura relacionada cuando la API así lo define.
- Desinstalar y reinstalar después de agregar o modificar permisos para obtener un token actualizado.
- Registrar únicamente webhooks permitidos por los scopes concedidos.

## Encabezados actuales

Usar para la API pública:

```http
Authorization: Bearer {access_token}
User-Agent: AppName (support@example.com)
Content-Type: application/json; charset=utf-8
```

No seguir la regla heredada `Authentication: bearer` para integraciones nuevas. Si un endpoint legado solo funciona de esa forma, aislar la excepción y conservar evidencia de tienda demo.

## Cliente de API

Centralizar las llamadas en el backend:

- resolver la tienda autenticada;
- descifrar el token justo antes de usarlo;
- aplicar timeouts;
- limitar concurrencia por tienda y app;
- sanear errores;
- no registrar cuerpos o encabezados sensibles;
- usar `api.tiendanube.com` o el dominio regional documentado;
- mantener la versión del endpoint explícita.

## Paginación

- Enviar `page` y `per_page`; el máximo documentado es 200.
- Tratar la numeración como basada en 1.
- Seguir los enlaces del encabezado `Link` en vez de construir URLs manualmente.
- Consultar `x-total-count` cuando se necesite el total.
- No asumir que una respuesta contiene toda la colección.

## Rate limit

El límite base documentado es de dos solicitudes por segundo por merchant y app; planes especiales pueden ampliarlo.

Leer:

- `x-rate-limit-limit`
- `x-rate-limit-remaining`
- `x-rate-limit-reset`

Ante `429`, pausar según los encabezados, aplicar jitter y reintentar solo operaciones seguras. Preferir webhooks a consultas repetitivas.

## Estados HTTP

- `200`: lectura o acción exitosa con cuerpo.
- `201`: recurso creado.
- `204`: éxito sin cuerpo.
- `400`: solicitud inválida.
- `401`: autenticación faltante o inválida.
- `403`: autenticado sin autorización.
- `404`: recurso inexistente.
- `422`: validación semántica.
- `429`: rate limit.
- `500`/`503`: error interno o indisponibilidad.

No devolver `200` con un error de negocio oculto para una API propia. Incluir `Request-ID` en errores 5xx. Hacer repetibles sin efectos inesperados GET, PUT y DELETE.

## Fuentes oficiales

- [Autenticación de aplicaciones](https://dev.nuvemshop.com.br/es/docs/applications/authentication)
- [Autenticación API detallada](https://tiendanube.github.io/api-documentation/authentication)
- [API Tiendanube](https://dev.nuvemshop.com.br/es/docs/developer-tools/nuvemshop-api)
- [Introducción API, paginación y encabezados](https://tiendanube.github.io/api-documentation/v1/intro)
- [Buenas prácticas de homologación](https://dev.nuvemshop.com.br/es/docs/homologation/guidelines)
- [Patrones HTTP](https://dev.nuvemshop.com.br/es/docs/applications/http-status)
