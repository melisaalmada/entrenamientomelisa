# Caso 04 — Evento ausente

## Objetivo didáctico

Enseñar a separar la acción que ocurrió en el sitio de la señal que debería representarla en Meta. Una compra confirmada no garantiza que `Purchase` haya sido enviado o recibido.

## Comportamiento esperado

Después de una compra confirmada correctamente, la integración debería enviar el evento correspondiente:

`1 compra confirmada → 1 Purchase`

## Síntoma observable

El sitio muestra una confirmación exitosa para el pedido, pero Meta no recibe `Purchase`:

`1 compra confirmada → 0 Purchase`

En el caso simulado, `PageView` está presente. Esto demuestra que el código base puede cargar mientras falta un evento específico.

## Impacto posible

- Conversiones reales que no aparecen en los reportes.
- Valor e ingresos atribuidos por debajo de los resultados reales.
- Costo por compra aparentemente superior.
- Audiencias de compradores incompletas.
- Señales insuficientes para medición u optimización.
- Diferencias entre la plataforma de comercio y Meta.

## Causas frecuentes

### 1. La llamada del evento no existe

La acción se completa, pero el código no ejecuta:

```javascript
fbq('track', 'Purchase', parameters);
```

Este es el error reproducido intencionalmente en el laboratorio.

### 2. El disparador no coincide con la confirmación

La regla puede apuntar a un botón, selector, URL o condición que cambió y ya no se cumple.

### 3. Un error de JavaScript interrumpe la ejecución

Otro problema en el código puede detener el script antes de llegar a la llamada del evento.

### 4. La página navega antes de completar el envío

Una redirección inmediata puede impedir que una señal asociada al clic termine de procesarse. Conviene ubicar el evento en un momento de confirmación confiable.

### 5. El píxel no está disponible en esa página o recorrido

El código base puede faltar en una plantilla específica, checkout externo, subdominio o versión móvil.

### 6. La señal se envía a otro conjunto de datos

El evento puede existir, pero llegar a un identificador distinto del que se está revisando.

### 7. El navegador o la configuración bloquean la solicitud

Consentimiento, extensiones, restricciones de tráfico o errores de carga pueden afectar el envío desde el navegador. Esto debe comprobarse, no asumirse.

### 8. El evento aún no se refleja

Antes de concluir que está ausente, se debe considerar el entorno de prueba, el tiempo transcurrido y la herramienta utilizada para observarlo.

## Ruta de investigación

1. Confirmar que la acción realmente terminó con éxito.
2. Reproducir una única compra en un entorno controlado.
3. Revisar Pixel Helper y Probar eventos durante la prueba.
4. Confirmar si `PageView` u otros eventos aparecen en la misma página.
5. Verificar que se está observando el conjunto de datos correcto.
6. Revisar el disparador y la llamada de `Purchase`.
7. Comprobar errores del navegador, redirecciones y bloqueos.
8. Comparar otros productos, dispositivos o recorridos.
9. Corregir una causa y repetir la prueba.

## Preguntas útiles para el diagnóstico

- ¿La compra quedó confirmada en el sistema del sitio?
- ¿Falta únicamente `Purchase` o también faltan otros eventos?
- ¿`PageView` aparece en la misma página?
- ¿Se está revisando el conjunto de datos correcto?
- ¿La regla depende de un botón, URL o selector que cambió?
- ¿El checkout ocurre en otro dominio o plantilla?
- ¿Existe una redirección inmediata después de la acción?
- ¿El problema ocurre siempre o solamente en algunos recorridos?

## Posibles soluciones

- Agregar o restaurar la llamada de `Purchase` en la confirmación correcta.
- Corregir el disparador, selector o regla de URL.
- Resolver errores de JavaScript previos.
- Confirmar que el código base exista en todo el recorrido necesario.
- Corregir el identificador del conjunto de datos.
- Ajustar la implementación para una confirmación asincrónica confiable.
- Revisar consentimiento y bloqueos cuando correspondan.
- Volver a probar después de cada cambio.

## Resolución del caso simulado

En `evento-ausente.html`, el botón confirma visualmente la compra, pero no ejecuta ninguna llamada de `Purchase`. La corrección sería enviar el evento, con sus parámetros, una vez confirmada la operación.

## Mensaje clave para el participante

> La confirmación del sitio y la recepción del evento son hechos diferentes. Ambos deben comprobarse.

## Nota para el entrenador

Pedir al grupo que no salte inmediatamente a una causa. Primero deben confirmar: acción real, evento esperado, evento recibido y conjunto de datos observado. Después pueden formular hipótesis.

