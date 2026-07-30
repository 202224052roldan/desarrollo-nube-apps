---
name: skillv3
description: Use when designing, building, migrating, reviewing, testing, or preparing a Tiendanube/Nuvemshop app involving OAuth, the Public API, an embedded Admin UI with Nexo, storefront or checkout extensions with NubeSDK, webhooks, privacy, orders, publication, or homologation.
---

# Construcción de aplicaciones Tiendanube

## Principio rector

Determinar primero **dónde se ejecuta cada parte de la app**. No tratar Nexo y NubeSDK como alternativas equivalentes:

- Usar **Nexo** para una interfaz integrada mediante iframe en el Administrador.
- Usar **NubeSDK** para código que se ejecuta en storefront o checkout.
- Usar **OAuth + API Tiendanube** desde el backend para acceder a datos de una tienda.
- Separar estas superficies explícitamente cuando la solución sea híbrida.

Tomar la documentación oficial actual como autoridad. Usar las skills locales anteriores como experiencia de una aplicación real, pero no conservar una regla heredada cuando contradiga el DevHub o la API vigente.

## Fuentes heredadas

Consultar estas fuentes locales únicamente para recuperar patrones ya comprobados:

- [`../SKILL.md`](../SKILL.md): integración original derivada de la app de redirecciones.
- [`../skillv1.md`](../skillv1.md): extracción genérica en inglés.
- [`../skillv2.md`](../skillv2.md): extracción genérica en español.

No copiar de ellas los endpoints de redirecciones URL. No conservar su encabezado legado `Authentication: bearer` como regla general: la documentación oficial actual usa `Authorization: Bearer`.

## Enrutamiento de referencias

Leer solo las referencias que correspondan al encargo:

| Situación | Referencia obligatoria |
|---|---|
| Elegir arquitectura o combinar superficies | [`references/app-types-and-architecture.md`](references/app-types-and-architecture.md) |
| Instalar la app, usar OAuth o consumir la API | [`references/oauth-and-api.md`](references/oauth-and-api.md) |
| Construir una UI dentro del Administrador | [`references/nexo-admin.md`](references/nexo-admin.md) |
| Ejecutar lógica o UI en storefront/checkout | [`references/nubesdk-storefront-checkout.md`](references/nubesdk-storefront-checkout.md) |
| Registrar o recibir webhooks y eventos de privacidad | [`references/webhooks-and-privacy.md`](references/webhooks-and-privacy.md) |
| Leer o editar pedidos, integrar ERP o logística | [`references/orders.md`](references/orders.md) |
| Evaluar preparación, evidencias o homologación | [`references/homologation.md`](references/homologation.md) |
| Verificar procedencia o resolver una contradicción | [`references/official-sources.md`](references/official-sources.md) |

## Flujo obligatorio

1. Inspeccionar el proyecto y clasificar cada superficie: Admin integrado, aplicación externa, storefront, checkout o combinación.
2. Cargar las referencias correspondientes antes de proponer arquitectura o escribir código.
3. Identificar `app_id`, geografías, distribución pública/privada, scopes, datos tratados, facturación y categoría de la app.
4. Diseñar límites separados para frontend, NubeSDK, backend, almacenamiento, API y webhooks.
5. Implementar seguridad, privacidad, observabilidad y requisitos de publicación durante el desarrollo.
6. Probar instalación, reinstalación, aislamiento entre tiendas, errores, rate limits y webhooks en una tienda demo.
7. Evaluar el gate de homologación sin iniciar acciones externas automáticamente.

## Invariantes técnicas

- Mantener `client_secret` y access tokens fuera del navegador y del repositorio.
- Cifrar el access token de cada tienda en reposo.
- Generar y validar OAuth `state`; tratar el authorization code como secreto de corta duración.
- Solicitar únicamente los scopes necesarios; reinstalar la app después de modificarlos.
- Usar `Authorization: Bearer {access_token}` y un `User-Agent` identificable en la API actual.
- Derivar el tenant desde una identidad autenticada; no confiar en un `store_id` recibido del cliente.
- Preferir webhooks sobre polling continuo.
- Implementar paginación, backoff y manejo explícito de `429` cuando se consuman colecciones.
- Verificar firmas sobre el cuerpo crudo antes de procesar webhooks.
- Hacer idempotentes las instalaciones, reinstalaciones, desinstalaciones y entregas repetidas.
- No registrar secretos, tokens, códigos OAuth, cookies ni encabezados de autenticación.
- Usar TLS y restringir `frame-ancestors` a los orígenes requeridos cuando exista un iframe.

## Gate estricto de homologación

Preparar la app para homologación desde el primer diseño, pero separar **preparación** de **inicio del proceso**.

No generar ni enviar una solicitud de homologación, formulario, ticket o artefacto externo salvo que:

1. el usuario lo solicite explícitamente; y
2. el readiness gate de [`references/homologation.md`](references/homologation.md) esté aprobado.

Si el proyecto parece apto sin que el usuario lo haya pedido, realizar únicamente una auditoría de preparación y comunicar los pendientes. Solicitar autorización antes de cualquier envío externo.

Si el usuario pide homologar un proyecto no apto, detener el envío, presentar los bloqueos de alta prioridad y proponer el trabajo necesario. No suavizar requisitos obligatorios para “avanzar”.

## Manejo de contradicciones

Cuando dos páginas oficiales discrepen:

1. preferir la referencia específica de autenticación o del recurso frente a una guía general;
2. preferir la página con actualización más reciente;
3. comprobar el comportamiento en una tienda demo sin exponer secretos;
4. documentar la decisión y la evidencia;
5. no inventar compatibilidad.

Ejemplo conocido: una directriz general menciona `refresh_token`, pero la documentación específica de autenticación indica tokens sin expiración y muestra una respuesta sin refresh token. No implementar un flujo de refresh hasta que el endpoint o un contrato oficial inequívoco lo confirme.

## Criterios de terminación

Antes de declarar una integración lista:

- confirmar la arquitectura y los SDK por superficie;
- ejecutar pruebas de instalación, reinstalación y cambio de scopes;
- verificar aislamiento de tenants y cifrado de tokens;
- comprobar Nexo o NubeSDK en su entorno real;
- probar API, paginación, rate limit y errores aplicables;
- probar firmas, duplicados, desinstalación y privacidad;
- revisar responsive, estados vacíos, errores, carga e idiomas;
- ejecutar el readiness gate de homologación cuando corresponda;
- informar cualquier requisito oficial que no pueda verificarse.
