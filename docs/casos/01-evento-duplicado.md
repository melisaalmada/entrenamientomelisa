# Caso 01 — Evento duplicado

## Objetivo didáctico

Ayudar al participante a reconocer que una acción del cliente puede generar más de un registro del mismo evento y que **recibir el evento no garantiza que la medición sea correcta**.

## Comportamiento esperado

Una acción del cliente debería generar un solo evento:

`1 clic en Agregar al carrito → 1 AddToCart`

## Síntoma observable

El cliente realiza una sola acción, pero Meta recibe dos eventos `AddToCart` con parámetros iguales o muy similares y con una diferencia mínima de tiempo:

`1 clic en Agregar al carrito → 2 AddToCart`

La primera diferencia que debe identificar el participante es la relación entre la cantidad de acciones reales y la cantidad de eventos recibidos.

## Impacto posible

- Inflación de la cantidad de eventos y conversiones informadas.
- Valores de conversión superiores a los reales.
- Audiencias basadas en eventos con acciones repetidas.
- Señales de optimización menos confiables.
- Diferencias entre los datos del sitio, la plataforma de comercio y Meta.

El impacto exacto depende del evento, de cómo se utiliza y de si existe un mecanismo válido de deduplicación.

## Causas frecuentes

### 1. La llamada del evento está repetida

El código contiene dos llamadas para la misma acción:

```javascript
fbq('track', 'AddToCart', parameters);
fbq('track', 'AddToCart', parameters);
```

Este es el error reproducido intencionalmente en el laboratorio.

### 2. Dos integraciones envían el mismo evento

El evento puede estar configurado al mismo tiempo mediante código directo, Google Tag Manager, un plugin o una herramienta de configuración de eventos. Si más de una integración responde a la misma acción, cada una puede enviar su propio evento.

### 3. El código base está instalado más de una vez

El píxel puede estar agregado en el tema del sitio y también mediante un plugin o integración. Esto requiere revisar la instalación completa; ver dos códigos no significa automáticamente que todos los eventos se duplicarán.

### 4. Hay más de un listener asociado al botón

Dos controladores de clic pueden ejecutar el mismo evento. Puede ocurrir después de modificaciones, scripts cargados dos veces o componentes que se inicializan nuevamente.

### 5. El evento se activa por más de una condición

Por ejemplo, `AddToCart` podría enviarse al hacer clic y volver a enviarse cuando el carrito cambia o aparece una confirmación.

### 6. Navegador y servidor envían el mismo evento sin deduplicación

Una integración puede enviar el evento desde el Meta Pixel y desde la API de conversiones. Para reconocer ambos registros como representaciones del mismo evento, la configuración debe permitir la deduplicación, normalmente mediante valores consistentes como `event_name` y `event_id`.

### 7. Una página o componente vuelve a ejecutarse

Recargas, redirecciones, actualizaciones parciales o ciclos de renderizado pueden activar nuevamente un evento configurado al cargar la página.

## Ruta de investigación

1. **Confirmar la acción real:** verificar cuántas veces el cliente hizo clic o completó el paso.
2. **Reproducir el comportamiento:** ejecutar una única acción en un entorno controlado.
3. **Observar los eventos:** revisar Meta Pixel Helper y la pestaña Probar eventos.
4. **Comparar los registros:** verificar nombre, hora, origen y parámetros.
5. **Identificar los canales:** comprobar si provienen del navegador, del servidor o de ambos.
6. **Revisar las integraciones:** código manual, plugins, administrador de etiquetas y otras herramientas.
7. **Buscar el primer punto duplicado:** localizar dónde una acción comienza a producir dos envíos.
8. **Modificar una sola variable:** corregir una fuente y volver a probar.

## Preguntas útiles para el diagnóstico

- ¿Cuántas veces realizó realmente la acción el cliente?
- ¿Los eventos tienen la misma hora y los mismos parámetros?
- ¿Ambos provienen del navegador o uno proviene del servidor?
- ¿El sitio usa código manual, plugin, administrador de etiquetas o más de una integración?
- ¿La duplicación ocurre en todos los eventos o solamente en uno?
- ¿Ocurre en todas las páginas, productos o botones?
- ¿Existe un `event_id` consistente cuando intervienen navegador y servidor?

## Posibles soluciones

- Eliminar la llamada repetida del evento.
- Mantener una sola fuente responsable de cada activación.
- Corregir listeners duplicados.
- Ajustar reglas o disparadores superpuestos.
- Evitar que el evento vuelva a ejecutarse durante recargas o renderizados.
- Configurar correctamente la deduplicación entre navegador y servidor.
- Volver a probar después de cada cambio.

La solución debe corresponder a la causa confirmada. No conviene eliminar integraciones al azar ni asumir que todos los eventos cercanos en el tiempo son duplicados.

## Resolución del caso simulado

En `duplicado.html`, el botón ejecuta deliberadamente dos llamadas `fbq('track', 'AddToCart')` con los mismos parámetros. La corrección consistiría en conservar una sola llamada.

## Mensaje clave para el participante

> Un evento presente no siempre es un evento correcto. Primero compara la acción real con la cantidad y calidad de los datos recibidos.

## Nota para el entrenador

Antes de revelar la explicación técnica, pedir al grupo que describa únicamente lo que puede comprobar en pantalla. Separar **síntoma**, **hipótesis** y **causa confirmada** ayuda a evitar diagnósticos apresurados.

