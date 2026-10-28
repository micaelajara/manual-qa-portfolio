# Propuestas de mejora

No son bugs: son cosas que funcionan como se programaron pero que, usando la tienda, me parecieron un problema para el cliente final. Las separé de los bugs para no inflar la lista.

| ID | Área | Mejora | Comportamiento sugerido | Prioridad |
|---|---|---|---|---|
| IMP-01 | Checkout / Confirmación de orden | Falta un botón de call to action para volver al inicio al finalizar la compra | Al completar la compra exitosamente, mostrar en la pantalla de confirmación un botón visible (ej. "Volver al inicio" o "Seguir comprando") que lleve al home | Media |
| IMP-02 | Catálogo / Búsqueda | No existe un buscador de productos en el storefront | Agregar una barra de búsqueda que permita encontrar productos por nombre/modelo, para mejorar la navegación del catálogo | Media |
| IMP-03 | Cuentas de usuario | No existe un flujo de registro explícito ("Crear cuenta"); la única forma de tener cuenta es completar el primer pedido | Si un usuario intenta loguearse antes de comprar con un email nuevo, ve el mensaje "No hay cuenta para ese email. Hacé tu primer pedido para crearla.", lo cual genera confusión. Además, pese a ese mensaje, el código de verificación se envía igual al mail. Se sugiere agregar una opción de "Crear cuenta" independiente del checkout, o al menos ajustar el mensaje para que sea coherente con que el código sí se envía | Media |
