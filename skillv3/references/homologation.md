# Gate y proceso de homologación

## Regla dura

No iniciar homologación solo porque “la app funciona manualmente”.

No enviar solicitud, formulario, ticket, video, credenciales o diagrama sin autorización explícita del usuario. Si el usuario solicita homologar, ejecutar primero el readiness gate.

## Readiness gate

### Producto

- Funcionalidad principal completa y testeable.
- Sin pantallas rotas, errores 404/500 ni flujos incompletos.
- Estados vacío, error, carga, éxito y confirmación implementados.
- Responsive, accesibilidad e idiomas alineados con las geografías.
- Onboarding claro después de instalar.
- Soporte y datos de contacto disponibles en cada idioma publicado.

### Arquitectura y SDK

- Tipo de app y superficies documentados.
- Admin integrado: React, Nimbus, Nexo, navegación sincronizada y ErrorBoundary.
- Storefront/checkout: NubeSDK, Web Worker, eventos, slots y ausencia de DOM legado.
- TLS, CSP, aislamiento de tenant y secretos verificados.

### Instalación y API

- Instalación iniciada desde Tiendanube.
- OAuth `state`, código, callback y token probados.
- Registro, login, desinstalación y reinstalación probados.
- Scopes mínimos y coherentes con el Portal y el diagrama.
- Paginación, rate limit, `429`, errores e idempotencia probados.
- Webhooks sustituyen polling donde corresponde.

### Datos y privacidad

- Tokens cifrados y ausentes de logs.
- Firma de webhooks y duplicados probados.
- Desinstalación detiene el acceso.
- Eliminación de tienda y cliente implementada cuando aplique.
- Política de privacidad y URLs de soporte configuradas.

### Evidencias

- Diagrama de secuencia que muestre autenticación, llamadas API, webhooks, backend y outputs.
- Video demo desde la instalación en Tiendanube.
- Registro de usuario nuevo y login de usuario existente.
- Desinstalación y reinstalación.
- Todas las funciones y escenarios del diagrama.
- Configuración técnica explicada al merchant.
- Cuenta demo sin bloqueos de suscripción, trial o activación.
- Credenciales de terceros preparadas para el equipo revisor.

### Categorías sensibles

Para ERP, Payments y Shipping:

- obtener y cubrir el guion específico;
- demostrar cada escenario de la checklist;
- mostrar sincronización, errores y recuperación;
- entregar nueva evidencia después de cada ajuste.

## Resultado

Clasificar el proyecto:

- **NO APTO:** existe un requisito obligatorio o de alta prioridad incumplido.
- **APTO CON PENDIENTES:** no hay bloqueos técnicos, pero faltan evidencias o elementos recomendados.
- **APTO PARA SOLICITAR:** todos los requisitos aplicables y evidencias están verificados.

Un estado apto no autoriza el envío. Pedir aprobación explícita.

## Durante la revisión

- Responder solicitudes del equipo en un máximo de cinco días.
- Implementar ajustes antes de pedir revalidación.
- No afirmar que un ajuste existe sin evidencia fresca.
- Repetir prueba y video de los escenarios afectados.

## Publicación

Revisar además:

- app usa APIs públicas de Tiendanube;
- no procesa pagos fuera del checkout;
- no duplica otra app propia;
- no comparte datos con terceros sin consentimiento;
- plan y facturación permiten upgrade/downgrade cuando aplique;
- ficha explica valor, funcionamiento, funciones, integración, precios y soporte;
- nombre de hasta 35 caracteres sin marcas reservadas;
- ícono 600×600 PNG/JPEG sin texto para la ficha;
- ícono de navegación embedded 16×16 SVG cuando aplique;
- screencast en español o subtitulado.

## Fuentes oficiales

- [Visión general de homologación](https://dev.nuvemshop.com.br/es/docs/homologation/overview)
- [Buenas prácticas](https://dev.nuvemshop.com.br/es/docs/homologation/guidelines)
- [Requisitos obligatorios](https://dev.nuvemshop.com.br/es/docs/homologation/requirements)
- [Checklist de homologación](https://dev.nuvemshop.com.br/es/docs/homologation/checklist)
- [Directrices de publicación](https://dev.nuvemshop.com.br/es/docs/applications/guidelines)
- [Landing Page](https://dev.nuvemshop.com.br/es/docs/applications/landing-page)
