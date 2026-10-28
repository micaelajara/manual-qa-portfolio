# Plan de prueba: alcance

Lo que definí antes de escribir un solo caso. El plan arrancó como un plan genérico para e-commerce con Medusa JS (56 casos) y lo ajusté a cómo funciona de verdad esta tienda, que es lo que lo llevó a 111.


**Alcance**

- Front + Back de la tienda, integración Medusa JS
- Prioridad: pagos, checkout, y la relación del back con emails/notificaciones y usuarios
- No incluye features custom ya testeadas previamente
- No incluye la sección "Extensions" del panel Admin (cubierta en otro plan)

**Particularidades confirmadas de Óptica Global (ajustan el plan genérico de Medusa)**

- No existe checkout como invitado: es obligatorio loguearse para registrar un pedido
- No hay cupones ni códigos de descuento; en cambio hay descuento automático por volumen (5% desde 50 unidades, 10% desde 100 unidades)
- Pago único por transferencia bancaria con confirmación manual por WhatsApp; no hay pasarela de pago online integrada
- Envío único vía "Vía Cargo", a coordinar (costo no calculado ni cobrado online)
- No hay buscador de productos en el storefront (se sugiere como mejora en la pestaña Mejoras)
- El registro/verificación de cuentas se prueba con Yopmail y correos temporales

**Áreas cubiertas**

- Checkout (front)
- Pagos (transferencia bancaria manual)
- Cuentas de usuario (registro, login, recuperación de contraseña)
- Emails / Notificaciones
- Store API de Medusa
- Panel Admin de Medusa (sin Extensions)
- Envíos (Vía Cargo)
- Catálogo / Búsqueda (sin buscador)
- Devoluciones / Reembolsos
- Descuentos por Volumen
- SEO (revisado con curl/WebFetch sobre el storefront real; ver bugs BUG-02 a BUG-08 en Reportes)

**Columna Tipo: Positivo / Negativo / Borde (edge case)**


**Columna Prioridad: Alta / Media / Baja — para ordenar la ejecución**


**Columna Estado (en "Casos de prueba"): Pendiente / Pasó / Falló / Bloqueado — se completa al ejecutar**


**Aclaraciones adicionales confirmadas (2026-07-05)**

- La identificación en checkout/cuenta es por email + código de verificación enviado al mail (no hay login con usuario/contraseña); por eso Medusa registra al cliente como tipo "Guest" en el Admin
- El descuento por volumen se implementa como Promotions "VOL_5" (5%) y "VOL_10" (10%) configuradas como automáticas: se aplican solas al cumplir la condición de cantidad, sin que el cliente ingrese ningún código
