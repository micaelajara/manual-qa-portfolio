# Bug reports

23 bugs, cada uno con ambiente, pasos, resultado actual vs. esperado, severidad, evidencia y el caso de prueba que lo encontró. Los re-tests quedan anotados adentro de cada reporte con su fecha.

| Severidad | Bugs |
|---|---|
| Crítico | 2 |
| Alto | 6 |
| Medio | 11 |
| Bajo | 4 |

| Estado | Bugs |
|---|---|
| Abierto | 15 |
| En progreso | 1 |
| Resuelto | 5 |
| Rechazado | 2 |

Los 2 rechazados quedan a la vista a propósito: los reporté con evidencia, el equipo de desarrollo me mostró que era el comportamiento esperado (la regla de negocio vivía en código custom, no en la configuración que yo estaba mirando) y actualicé los casos afectados. Un rechazo bien fundamentado también es información.

## Índice

| ID | Título | Severidad | Estado | Caso |
|---|---|---|---|---|
| [BUG-15](#bug-15) | Es imposible que un cliente nuevo use el descuento de primera compra por código de referido de laboratorio | Crítico | Abierto | REF-01 |
| [BUG-22](#bug-22) | El descuento por volumen aplicado en el carrito se pierde al completar la orden — el cliente termina pagando el precio completo | Crítico | Rechazado | VOL-08 |
| [BUG-05](#bug-05) | La URL /robots.txt no sirve un archivo robots.txt real | Alto | Abierto | SEO-04 |
| [BUG-06](#bug-06) | La URL /sitemap.xml no sirve un sitemap real | Alto | Abierto | SEO-05 |
| [BUG-11](#bug-11) | Las promociones de descuento por volumen (VOL_5, VOL_75, VOL_125) no tienen regla de cantidad mínima ni son automáticas | Alto | Rechazado | VOL-10 |
| [BUG-18](#bug-18) | El selector de cantidad en la tarjeta de producto no valida el stock disponible ni muestra ninguna alerta al superarlo | Alto | Resuelto | CB-06 |
| [BUG-20](#bug-20) | La página del pedido en el front no refleja la cancelación de la orden tras recargar | Alto | Resuelto | CO-15 |
| [BUG-21](#bug-21) | No se envía ningún email al cliente notificando que se procesó la devolución de su dinero (reembolso) | Alto | Resuelto | EM-10 |
| [BUG-01](#bug-01) | No se valida la longitud máxima de caracteres en los campos de Dirección de envío y Localidad del checkout | Medio | Abierto | CO-09 |
| [BUG-03](#bug-03) | El <title> de las páginas es genérico ("Catálogo") y no incluye el nombre de la marca | Medio | Abierto | SEO-02 |
| [BUG-04](#bug-04) | La meta description es genérica y se reutiliza igual en Open Graph y Twitter Cards | Medio | Abierto | SEO-03 |
| [BUG-07](#bug-07) | Falta el tag <link rel="canonical"> en las páginas | Medio | Abierto | SEO-06 |
| [BUG-08](#bug-08) | Las imágenes de producto usan un alt genérico ("Thumbnail") | Medio | Abierto | SEO-07 |
| [BUG-10](#bug-10) | Falta el tag <h1> en las páginas de ficha de producto | Medio | Resuelto | SEO-10 |
| [BUG-12](#bug-12) | El campo Nombre/Apellido en "Mi perfil" acepta cualquier símbolo | Medio | Abierto | US-07 |
| [BUG-13](#bug-13) | El mensaje de confirmación al editar cualquier campo de "Mi perfil" siempre dice "Email actualizado correctamente" | Medio | Abierto | US-10 |
| [BUG-14](#bug-14) | Existen 2 "Default Sales Channel" duplicados, cada uno con su propia publishable key | Medio | Resuelto | AD-08 |
| [BUG-16](#bug-16) | Los campos Email, Nombre de contacto y Teléfono en checkout no validan longitud máxima | Medio | Abierto | CO-10 |
| [BUG-17](#bug-17) | Caracteres especiales/emojis en los campos de dirección de envío rompen el layout del checkout | Medio | Abierto | CO-11 |
| [BUG-02](#bug-02) | El atributo lang del HTML está en inglés ("en") pese a que el sitio está en español (Argentina) | Bajo | Abierto | SEO-01 |
| [BUG-09](#bug-09) | El campo de Tracking del Fulfillment en el Admin no valida un límite máximo de caracteres | Bajo | Abierto | AD-07 |
| [BUG-19](#bug-19) | No existe un botón explícito de "Reenviar código" en la pantalla de identificación | Bajo | En progreso | US-06 |
| [BUG-23](#bug-23) | Las fichas de producto no tienen datos estructurados (schema.org Product/Offer) | Bajo | Abierto | SEO-08 |

## BUG-01

**No se valida la longitud máxima de caracteres en los campos de Dirección de envío y Localidad del checkout**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Checkout, paso "delivery"
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** CO-09

**Pasos para reproducir**

1. Ir a checkout, paso de dirección de envío
2. Completar el campo Dirección y/o Localidad con una cadena larga de caracteres repetidos (cientos de "3" o "1")
3. Confirmar la dirección y avanzar al paso de envío
4. Revisar el resumen "Tu compra"

**Resultado actual**

El texto largo desborda su columna y se superpone con "Contacto" y "Dirección de facturación", rompiendo el layout y volviendo ilegible esa información

**Resultado esperado**

El formulario debería limitar la cantidad de caracteres permitidos, y/o el resumen debería truncar/wrappear el texto sin invadir las columnas vecinas

**Evidencia y re-tests**

Screenshot del checkout paso "delivery" (compartido 2026-07-05)

[Re-test 2026-07-06] Sigue roto, NO corregido: Dirección/Contacto siguen aceptando texto sin límite y el texto largo se superpone visualmente entre columnas en el resumen de checkout real (sin ningún mensaje de error de validación). Screenshot: evidence/BUG-01-checkout-overflow-still-broken-2026-07-06.png

[Actualización 2026-07-06] Automatizado de punta a punta (login real + pedido real completado, Manual Payment sin cobro real): confirmado que el desborde se ve en la página de orden confirmada (/order/{id}/confirmed), no en el paso de Revisión del checkout (ahí el overflow queda contenido en su columna). scrollWidth (549) casi duplica clientWidth (284) en el párrafo de Dirección de envío — el texto se derrama sobre Contacto y Método. Test: tests/optica-global/checkout.spec.js (orden real #12).

---

## BUG-02

**El atributo lang del HTML está en inglés ("en") pese a que el sitio está en español (Argentina)**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Home
- **Severidad:** Bajo
- **Estado:** Abierto
- **Caso relacionado:** SEO-01

**Pasos para reproducir**

1. Ir a la home
2. Inspeccionar el código fuente, etiqueta <html lang="...">

**Resultado actual**

Devuelve lang="en"

**Resultado esperado**

Debería devolver lang="es-AR" o "es"

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-03

**El <title> de las páginas es genérico ("Catálogo") y no incluye el nombre de la marca**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Home
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** SEO-02

**Pasos para reproducir**

1. Ir a la home
2. Inspeccionar el <title> del documento

**Resultado actual**

El título es "Catálogo", sin mención a la marca "Óptica Global" ni diferenciación por página

**Resultado esperado**

El título debería incluir el nombre de la marca y ser descriptivo/único por página

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-04

**La meta description es genérica y se reutiliza igual en Open Graph y Twitter Cards**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Home
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** SEO-03

**Pasos para reproducir**

1. Ir a la home
2. Inspeccionar meta name="description", og:description, twitter:description

**Resultado actual**

Las tres etiquetas devuelven el mismo texto genérico: "Catálogo de productos."

**Resultado esperado**

Cada página debería tener una descripción única y descriptiva para SEO y para compartir en redes

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-05

**La URL /robots.txt no sirve un archivo robots.txt real**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar)
- **Severidad:** Alto
- **Estado:** Abierto
- **Caso relacionado:** SEO-04

**Pasos para reproducir**

1. Acceder a https://medusa-storefront-optica.vercel.app/robots.txt

**Resultado actual**

Devuelve HTTP 200 con content-type text/html, sirviendo el HTML de la home en vez del archivo esperado

**Resultado esperado**

Debería devolver un archivo de texto plano con las directivas de crawling

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-06

**La URL /sitemap.xml no sirve un sitemap real**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar)
- **Severidad:** Alto
- **Estado:** Abierto
- **Caso relacionado:** SEO-05

**Pasos para reproducir**

1. Acceder a https://medusa-storefront-optica.vercel.app/sitemap.xml

**Resultado actual**

Devuelve HTTP 200 con content-type text/html, sirviendo el HTML de la home en vez del XML esperado

**Resultado esperado**

Debería devolver un XML válido listando las URLs indexables del sitio

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-07

**Falta el tag <link rel="canonical"> en las páginas**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Home
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** SEO-06

**Pasos para reproducir**

1. Inspeccionar el <head> de la home
2. Buscar <link rel="canonical">

**Resultado actual**

No se encontró ningún <link rel="canonical"> en la home

**Resultado esperado**

Cada página debería declarar su URL canónica, especialmente por el parámetro v_id de variantes que puede generar URLs duplicadas

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-08

**Las imágenes de producto usan un alt genérico ("Thumbnail")**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Home/Catálogo
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** SEO-07

**Pasos para reproducir**

1. Inspeccionar los atributos alt de las miniaturas de producto en la home

**Resultado actual**

Las 14 miniaturas de producto revisadas devuelven alt="Thumbnail"

**Resultado esperado**

El alt debería describir el producto (nombre/color) para accesibilidad y SEO de imágenes

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-09

**El campo de Tracking del Fulfillment en el Admin no valida un límite máximo de caracteres**

- **Ambiente:** Panel Admin de Óptica Global — Orders > detalle de orden > Fulfillment
- **Severidad:** Bajo
- **Estado:** Abierto
- **Caso relacionado:** AD-07

**Pasos para reproducir**

1. Abrir una orden en el Admin
2. Ir al detalle del Fulfillment y completar el campo Tracking con una cadena larga de caracteres repetidos
3. Guardar y revisar cómo se muestra

**Resultado actual**

El texto largo desborda el contenedor del campo Tracking sin truncarse ni wrappear correctamente

**Resultado esperado**

El campo debería limitar la longitud permitida y/o la UI debería truncar/wrappear el texto

**Evidencia y re-tests**

Screenshot del detalle de Fulfillment en el Admin (compartido 2026-07-05)

[Re-test 2026-07-06] Sigue vigente, confirmado en vivo: se guardó un tracking number de 300 caracteres sin ningún límite (sin atributo maxlength) en la orden real #13, y el texto desborda la tarjeta del Fulfillment hacia la derecha, saliéndose del layout.

---

## BUG-10

**Falta el tag <h1> en las páginas de ficha de producto**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Ficha de producto
- **Severidad:** Medio
- **Estado:** Resuelto
- **Caso relacionado:** SEO-10

**Pasos para reproducir**

1. Ir a una ficha de producto (ej. /ar/products/optica-zr6046)
2. Inspeccionar los headings de la página

**Resultado actual**

El nombre del producto es un <h2 data-testid="product-title">; no hay ningún <h1> en toda la página

**Resultado esperado**

La ficha de producto debería tener un único <h1> descriptivo (ej. el nombre del producto)

**Evidencia y re-tests**

Confirmado con test automatizado de Playwright (tests/optica-global/seo.spec.js) — 2026-07-05

---

## BUG-11

**Las promociones de descuento por volumen (VOL_5, VOL_75, VOL_125) no tienen regla de cantidad mínima ni son automáticas**

- **Ambiente:** API de Medusa de Óptica Global — Promotions
- **Severidad:** Alto
- **Estado:** Rechazado
- **Caso relacionado:** VOL-10

**Pasos para reproducir**

1. GET /admin/promotions?fields=id,code,is_automatic,status,*application_method,*rules con token de Admin
2. Revisar is_automatic y rules de cada una

**Resultado actual**

Las 3 promociones devuelven is_automatic:false y rules:[] — se puede aplicar el 5%, 7.5% o 12.5% con una sola unidad, y ninguna se aplica sola al alcanzar una cantidad

**Resultado esperado**

Cada código debería tener una regla de cantidad mínima real (ej. 50/75/125 unidades) que impida aplicarlo por debajo del umbral correspondiente

**Evidencia y re-tests**

respuesta real de la API guardada como evidencia (VOL-promotions-config-2026-07-05.json, repo privado) (respuesta real de la API)

[Actualización 2026-07-06] Rechazado por el equipo de desarrollo: los códigos VOL_* no son autoservicio — un subscriber los aplica automáticamente según la cantidad real de la orden; la validación de umbral vive en código custom (las reglas nativas de Medusa no pueden expresar "cantidad total >= N"). rules:[] es el comportamiento esperado, no un bug. Cascada: ver VOL-01, VOL-02, VOL-03, VOL-04, VOL-09, VOL-10 en Casos de prueba.

---

## BUG-12

**El campo Nombre/Apellido en "Mi perfil" acepta cualquier símbolo**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Mi cuenta > Mi perfil
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** US-07

**Pasos para reproducir**

1. Loguearse y entrar a Mi cuenta > Mi perfil
2. Completar Nombre y Apellido con símbolos arbitrarios (ej. "!!!!", "ssssssssss")
3. Guardar cambios

**Resultado actual**

Los campos aceptan y guardan cualquier símbolo sin restricción

**Resultado esperado**

Nombre y Apellido deberían validar el tipo de caracteres permitido (letras, espacios, acentos), rechazando símbolos como "!"

**Evidencia y re-tests**

Screenshot de "Mi perfil" (compartido 2026-07-05)

[Re-test 2026-07-06] Reconfirmado por Micaela: sigue vigente.

---

## BUG-13

**El mensaje de confirmación al editar cualquier campo de "Mi perfil" siempre dice "Email actualizado correctamente"**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Mi cuenta > Mi perfil
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** US-10

**Pasos para reproducir**

1. Ir a Mi cuenta > Mi perfil
2. Editar el campo Nombre (o cualquier otro campo) y guardar
3. Revisar el texto del toast de confirmación

**Resultado actual**

El toast muestra "Email actualizado correctamente" sin importar qué campo se haya editado realmente

**Resultado esperado**

El mensaje debería reflejar el campo efectivamente editado (ej. "Nombre actualizado correctamente")

**Evidencia y re-tests**

Video "Evidencia bug editor account.avi" (compartido 2026-07-05)

[Re-test 2026-07-06] Reconfirmado por Micaela: sigue vigente, el toast de confirmación en Mi perfil sigue mostrando el mensaje genérico sin reflejar el campo realmente editado.

---

## BUG-14

**Existen 2 "Default Sales Channel" duplicados, cada uno con su propia publishable key**

- **Ambiente:** API de Medusa de Óptica Global — API Keys / Sales Channels
- **Severidad:** Medio
- **Estado:** Resuelto
- **Caso relacionado:** AD-08

**Pasos para reproducir**

1. GET /admin/api-keys?fields=id,title,redacted,type,*sales_channels con token de Admin
2. Revisar cuántos sales channels/publishable keys existen

**Resultado actual**

Existen 2 sales channels y 2 publishable keys, ambos llamados "Default...", creados con 1 minuto de diferencia el día de la configuración inicial

**Resultado esperado**

Debería existir un solo sales channel/publishable key salvo que haya una razón de negocio real; pendiente de confirmar con el equipo

**Evidencia y re-tests**

respuesta real de la API guardada como evidencia (AD-08-api-keys-2026-07-05.json, repo privado)

[Re-test 2026-07-06] Resuelto: GET /admin/api-keys ahora devuelve una sola publishable key ("Default Publishable API Key") con un solo Default Sales Channel. El duplicado ya no existe.

---

## BUG-15

**Es imposible que un cliente nuevo use el descuento de primera compra por código de referido de laboratorio**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Mi perfil / Checkout / Cuentas
- **Severidad:** Crítico
- **Estado:** Abierto
- **Caso relacionado:** REF-01

**Pasos para reproducir**

1. Como cliente sin cuenta, buscar dónde ingresar un código de referido antes de comprar
2. Confirmar que el campo de código solo existe en "Mi perfil"
3. Confirmar que "Mi perfil" solo es accesible con una cuenta ya creada
4. Confirmar que la cuenta solo se crea completando la primera compra (ver US-01/US-11)
5. Confirmar que el descuento del código solo aplica en la primera compra (regla de negocio confirmada)

**Resultado actual**

El código de referido solo puede ingresarse DESPUÉS de crear la cuenta, y la cuenta solo se crea completando la primera compra — para cuando el cliente puede ingresar el código, su primera compra (la única elegible para el descuento) ya pasó sin él

**Resultado esperado**

El cliente debería poder ingresar el código de referido antes o durante su primera compra (ej. un campo en el checkout), no solo después en "Mi perfil"

**Evidencia y re-tests**

Explicado y confirmado por Micaela en la conversación (2026-07-05)

[Actualización 2026-07-06] Re-testeado tras un fix parcial: ahora SÍ existe un campo para ingresar el código de laboratorio durante el checkout (ya no hace falta pasar por Mi perfil), lo cual resuelve el problema original de timing. Pero el descuento sigue sin reflejarse: orden real #10 (cliente-de-prueba@yopmail.com, hoy) completada con discount_total=0 y promotions=[], y el customer de esa orden no tiene ningún laboratorio vinculado en su metadata. Los 2 laboratorios en /admin/labs siguen con active_optica_count=0. El vínculo cliente-laboratorio ingresado en checkout no se está persistiendo del lado del backend antes de completar la orden. Evidencia: respuesta real de la API guardada como evidencia (BUG-15-lab-discount-not-applied-2026-07-06.json, repo privado). Sigue Abierto.

---

## BUG-16

**Los campos Email, Nombre de contacto y Teléfono en checkout no validan longitud máxima**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Checkout, paso "delivery"
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** CO-10

**Pasos para reproducir**

1. Ir a checkout
2. Completar Email con 200+ caracteres antes de la @, y Nombre de contacto/Teléfono con 100+ caracteres repetidos
3. Confirmar y revisar el resumen "Tu compra"

**Resultado actual**

Mismo patrón que BUG-01: los campos aceptan longitud ilimitada y el resumen se rompe

**Resultado esperado**

Cada campo debería limitar la longitud acorde al tipo de dato (email, nombre, teléfono)

**Evidencia y re-tests**

Probado manualmente por Micaela (2026-07-05)

[Re-test 2026-07-06] Reconfirmado por Micaela con screenshot: sigue vigente.

---

## BUG-17

**Caracteres especiales/emojis en los campos de dirección de envío rompen el layout del checkout**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Checkout, paso "delivery"
- **Severidad:** Medio
- **Estado:** Abierto
- **Caso relacionado:** CO-11

**Pasos para reproducir**

1. En el formulario de dirección de envío, ingresar caracteres especiales/emojis
2. Confirmar la dirección
3. Revisar el resumen de checkout

**Resultado actual**

El layout del resumen se rompe con este tipo de caracteres, de forma similar a BUG-01 (aunque el problema ahí era la longitud, no el tipo de caracter)

**Resultado esperado**

El formulario debería sanitizar o validar el tipo de caracteres permitidos sin romper el layout del resumen

**Evidencia y re-tests**

Probado manualmente por Micaela (2026-07-05)

[Re-test 2026-07-06] Reconfirmado por Micaela con screenshot: sigue vigente.

---

## BUG-18

**El selector de cantidad en la tarjeta de producto no valida el stock disponible ni muestra ninguna alerta al superarlo**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Catálogo, tarjeta de producto
- **Severidad:** Alto
- **Estado:** Resuelto
- **Caso relacionado:** CB-06

**Pasos para reproducir**

1. En el listado de catálogo, seleccionar una variante de un producto
2. Usar el botón "+" del selector de cantidad repetidamente, superando el stock real disponible de esa variante
3. Observar si aparece algún mensaje de advertencia

**Resultado actual**

La cantidad sube sin límite (probado hasta 21+ unidades) sin ningún mensaje de alerta, sin importar el stock real disponible

**Resultado esperado**

El sistema debería limitar la cantidad seleccionable al stock disponible, o al menos mostrar un mensaje de alerta al alcanzar/superar el límite

**Evidencia y re-tests**

Video "Screencast_2026-07-05-20-07-31.mp4.avi" (compartido 2026-07-05)

[Re-test 2026-07-06] Resuelto, confirmado por Micaela manualmente.

---

## BUG-19

**No existe un botón explícito de "Reenviar código" en la pantalla de identificación**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Identificación (paso "Tu email")
- **Severidad:** Bajo
- **Estado:** En progreso
- **Caso relacionado:** US-06

**Pasos para reproducir**

1. Ir al paso de identificación en checkout
2. Ingresar el email y llegar a la pantalla "Tu email" con el campo de código
3. Buscar una opción para reenviar el código

**Resultado actual**

La pantalla solo ofrece "Verificar y continuar" y "Cambiar email" — no hay un botón directo de "Reenviar código"

**Resultado esperado**

Debería existir una opción explícita de reenvío del código sin tener que pasar por "Cambiar email" (que no está confirmado que funcione como reenvío real)

**Evidencia y re-tests**

Screenshot de la pantalla "Tu email" (compartido 2026-07-05)

[Re-test 2026-07-06] Resuelto PARCIALMENTE: el paso de identificación del checkout (/ar/checkout?step=identify) ya muestra "Reenviar código (29s)" con cuenta regresiva — confirmado en vivo. Pero la pantalla de login standalone (Mi cuenta > Ingresar, /ar/account fuera del checkout) sigue sin ese botón, solo "Entrar" y "Usar otro email". Falta aplicar el mismo fix ahí.

---

## BUG-20

**La página del pedido en el front no refleja la cancelación de la orden tras recargar**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Mi cuenta > Pedidos / confirmación de orden
- **Severidad:** Alto
- **Estado:** Resuelto
- **Caso relacionado:** CO-15

**Pasos para reproducir**

1. Completar un pedido y ver su confirmación/detalle en el front
2. Cancelar esa orden desde el Admin
3. Recargar (F5) la página del pedido en el front

**Resultado actual**

La página sigue mostrando el pedido como realizado/exitoso pese a estar cancelado en el Admin, incluso después de recargar

**Resultado esperado**

La página debería reflejar el estado real de la orden (cancelado) al recargar, no quedar con datos obsoletos

**Evidencia y re-tests**

Reportado manualmente por Micaela (2026-07-05)

[Re-test 2026-07-06] Resuelto, confirmado por Micaela manualmente: la cancelación ya se refleja en el front.

---

## BUG-21

**No se envía ningún email al cliente notificando que se procesó la devolución de su dinero (reembolso)**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Emails de devolución
- **Severidad:** Alto
- **Estado:** Resuelto
- **Caso relacionado:** EM-10

**Pasos para reproducir**

1. Procesar un reembolso (total o parcial) de una orden desde el Admin
2. Revisar la bandeja de entrada del cliente

**Resultado actual**

Solo se envía email cuando se confirma la cancelación de la orden; no hay ningún email que notifique específicamente que el dinero fue reembolsado

**Resultado esperado**

El cliente debería recibir una notificación por email que confirme que se procesó su reembolso, con el monto correspondiente

**Evidencia y re-tests**

Reportado manualmente por Micaela (2026-07-05)

[Re-test 2026-07-06] Resuelto, confirmado por Micaela manualmente: ya se envía el mail confirmando la devolución.

---

## BUG-22

**El descuento por volumen aplicado en el carrito se pierde al completar la orden — el cliente termina pagando el precio completo**

- **Ambiente:** API de Medusa de Óptica Global — carrito → orden
- **Severidad:** Crítico
- **Estado:** Rechazado
- **Caso relacionado:** VOL-08

**Pasos para reproducir**

1. Armar un carrito con un total superior a $150.000
2. Aplicar el código VOL_5 (el carrito calcula correctamente el 5% de descuento)
3. Completar el carrito en una orden real (POST /store/carts/{id}/complete)
4. Revisar la orden resultante en el Admin

**Resultado actual**

La orden real quedó con discount_total=0, promotions=None, y total=original_total=$159.500 (precio completo). El payment_collection también quedó autorizado por el monto completo sin descuento — el cliente pagaría de más

**Resultado esperado**

El descuento aplicado en el carrito debería preservarse en la orden final y en el monto a pagar/transferir

**Evidencia y re-tests**

respuesta real de la API guardada como evidencia (BUG-22-order-discount-lost-2026-07-05.json, repo privado) (orden real order_XXXX)

[Actualización 2026-07-06] Rechazado por el equipo de desarrollo: el carrito de prueba tenía ~11 unidades (VOL_5 exige 50). El middleware reconcileVolumeBeforeComplete removió correctamente el código no calificado antes de completar la orden — es la salvaguarda anti-abuso funcionando, no una pérdida de descuento legítimo. Cascada: ver VOL-08 en Casos de prueba.

---

## BUG-23

**Las fichas de producto no tienen datos estructurados (schema.org Product/Offer)**

- **Ambiente:** Storefront Óptica Global (medusa-storefront-optica.vercel.app/ar) — Ficha de producto
- **Severidad:** Bajo
- **Estado:** Abierto
- **Caso relacionado:** SEO-08

**Pasos para reproducir**

1. Ir a una ficha de producto (ej. /ar/products/optica-zr6046)
2. Buscar en el HTML un bloque <script type="application/ld+json">

**Resultado actual**

No se encontró ningún bloque de datos estructurados en la ficha de producto revisada

**Resultado esperado**

La ficha de producto debería incluir datos estructurados (schema.org Product/Offer) para habilitar rich snippets de precio/disponibilidad en buscadores

**Evidencia y re-tests**

Verificado con curl sobre la ficha de producto ZR6046 (2026-07-05)

---
