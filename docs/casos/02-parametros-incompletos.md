# Caso 02 — Evento recibido con parámetros incompletos

## Objetivo didáctico

Mostrar que la presencia de un evento no confirma por sí sola la calidad de una integración. El participante debe revisar también los parámetros necesarios para el uso previsto.

## Comportamiento esperado

La compra simulada debería enviar `Purchase` con información suficiente para interpretar la conversión:

- `value: 149.90`
- `currency: "BRL"`
- `content_ids: ["SKU-LAB02"]`
- `content_type: "product"`
- `num_items: 1`

## Síntoma observable

Meta recibe `Purchase`, pero el evento no contiene `value`, `currency` ni `content_ids`. El evento está presente; sus datos no permiten representar correctamente la compra simulada.

## Impacto posible

- Valor de conversión ausente o incorrecto.
- Reportes de ingresos y retorno menos confiables.
- Dificultad para relacionar la compra con un producto.
- Limitaciones en usos que dependan de parámetros específicos.
- Diagnósticos o advertencias en el Administrador de eventos.
- Diferencias entre los pedidos reales y la información enviada a Meta.

## Causas frecuentes

### 1. La llamada no incluye el objeto de parámetros

```javascript
fbq('track', 'Purchase');
```

Este es el error reproducido intencionalmente en el laboratorio.

### 2. Las variables no tienen valores disponibles

El sitio intenta enviar parámetros, pero las variables del precio, la moneda o el producto están vacías, no definidas o se cargan después del evento.

### 3. Existe un problema de mapeo

La plataforma o integración guarda el dato con un nombre, pero el evento intenta leer otro campo. Por ejemplo, el precio existe en el pedido, pero no está asociado a `value`.

### 4. El disparador ocurre demasiado temprano

El evento se envía antes de que la página o la aplicación termine de cargar los datos de la compra.

### 5. La implementación usa nombres o formatos incorrectos

Un parámetro puede estar mal escrito, usar un formato inesperado o enviar una moneda que no corresponde al valor.

### 6. La plantilla no contempla todos los productos o recorridos

La integración funciona para algunos artículos, páginas o métodos de compra, pero no para otros.

### 7. Una actualización modificó la estructura del sitio

Cambios en el tema, checkout, plugin o capa de datos pueden dejar parámetros sin una fuente válida.

## Ruta de investigación

1. Confirmar que el evento esperado fue recibido.
2. Abrir los detalles del evento en Probar eventos.
3. Comparar los parámetros recibidos con la compra real.
4. Identificar cuáles faltan, están vacíos o tienen un formato incorrecto.
5. Revisar de dónde debería obtenerse cada valor.
6. Comprobar el momento en que se dispara el evento.
7. Comparar distintos productos, páginas y recorridos.
8. Corregir una variable o mapeo y volver a probar.

## Preguntas útiles para el diagnóstico

- ¿Qué parámetros necesita el uso previsto de este evento?
- ¿El valor coincide con el total real de la compra?
- ¿La moneda está presente y corresponde al valor?
- ¿El identificador coincide con el producto o catálogo?
- ¿Los parámetros faltan siempre o solamente en algunos casos?
- ¿El evento se dispara antes de que los datos estén disponibles?
- ¿Hubo cambios recientes en el sitio o en la integración?

## Posibles soluciones

- Agregar el objeto de parámetros a la llamada del evento.
- Corregir variables vacías o no definidas.
- Ajustar el mapeo entre la plataforma y los parámetros de Meta.
- Cambiar el momento del disparo para esperar los datos necesarios.
- Corregir nombres, tipos y formatos.
- Actualizar plugins o configuraciones que hayan perdido compatibilidad.
- Validar la solución con diferentes productos y recorridos.

## Resolución del caso simulado

En `incompleto.html`, el evento se envía deliberadamente como `fbq('track', 'Purchase')`. La corrección sería incluir los parámetros de la compra en la llamada.

## Mensaje clave para el participante

> Un evento puede estar presente y aun así no contener la información necesaria para medir correctamente.

## Nota para el entrenador

Pedir primero que el grupo enumere los hechos observables: evento recibido, compra real de R$ 149,90 y campos ausentes. Recién después separar impacto, hipótesis y causa confirmada.

