# Caso 03 — Lead generado antes del envío del formulario

## Objetivo didáctico

Enseñar que un evento puede tener el nombre correcto, pero activarse en el momento equivocado. El participante debe comparar el hecho real del recorrido con la condición utilizada para medirlo.

## Comportamiento esperado

`Lead` debería enviarse después de que el formulario haya sido enviado y confirmado correctamente:

`Formulario enviado con éxito → Lead`

Abrir o iniciar el formulario no demuestra que exista un lead.

## Síntoma observable

La persona hace clic en Abrir formulario, pero no completa campos ni realiza el envío. Sin embargo, Meta recibe `Lead` en ese primer clic.

`0 formularios enviados → 1 Lead`

## Impacto posible

- Cantidad de leads superior a las solicitudes reales.
- Costo por lead artificialmente bajo.
- Diferencias entre Meta y el sistema que recibe los formularios.
- Audiencias de leads que incluyen visitantes sin conversión.
- Optimización basada en una señal que representa visitas en lugar de leads.
- Dificultad para evaluar la eficacia real del formulario.

## Causas frecuentes

### 1. Lead se configuró al abrir el formulario

```javascript
fbq('track', 'Lead');
```

Este es el error reproducido intencionalmente en el laboratorio.

### 2. El evento se activa con el clic y no con la confirmación

Hacer clic en Enviar no garantiza que el formulario haya superado la validación ni que el servidor lo haya recibido.

### 3. La validación falla después de activar el evento

Puede faltar un campo obligatorio, existir un formato incorrecto o producirse un error de red, pero `Lead` ya fue enviado.

### 4. La página de agradecimiento es accesible directamente

Si el evento depende de visitar una URL de confirmación, una visita directa o una recarga puede generar un lead sin un nuevo formulario.

### 5. El evento está asociado a un paso intermedio

Abrir el formulario, avanzar de sección o iniciar la conversación puede estar configurado erróneamente como conversión final.

### 6. Un disparador demasiado amplio coincide con varias páginas

Una regla basada en una URL parcial, un título o un selector genérico puede activar `Lead` fuera de la confirmación esperada.

## Ruta de investigación

1. Definir qué acción comercial representa realmente un lead.
2. Abrir el formulario sin completarlo ni enviarlo.
3. Revisar Pixel Helper y Probar eventos.
4. Comparar la hora del clic en Abrir formulario con la llegada de `Lead`.
5. Probar el botón con campos vacíos o datos ficticios inválidos.
6. Identificar el disparador: carga, clic, envío o confirmación.
7. Comprobar si existe una respuesta exitosa del formulario.
8. Ajustar el disparador y repetir los escenarios válidos e inválidos.

## Preguntas útiles para el diagnóstico

- ¿Qué acción debe representar `Lead` en este sitio?
- ¿El formulario fue realmente recibido por el sistema correspondiente?
- ¿El evento se activa al cargar, al hacer clic o después de una confirmación?
- ¿Se registra también cuando la validación falla?
- ¿La página de agradecimiento puede abrirse o recargarse directamente?
- ¿Existe más de un formulario o recorrido con reglas diferentes?
- ¿La cantidad de leads coincide con los registros del sistema receptor?

## Posibles soluciones

- Mover `Lead` desde la carga de página a la confirmación exitosa.
- Activar el evento después de que el formulario responda correctamente.
- Evitar usar el clic como única evidencia de conversión.
- Restringir reglas demasiado amplias.
- Proteger el evento frente a recargas o accesos directos a la confirmación.
- Probar envíos exitosos, errores de validación y errores de red.

## Resolución del caso simulado

En `lead-anticipado.html`, `Lead` se ejecuta deliberadamente al hacer clic en Abrir formulario. La corrección consistiría en quitar esa llamada del inicio y ejecutarla únicamente después de confirmar un envío exitoso.

## Privacidad en la práctica

El formulario del laboratorio no almacena ni transmite lo escrito. Aun así, durante el entrenamiento debe indicarse que solo se utilicen datos ficticios. La actividad no necesita datos personales para demostrar el problema.

## Mensaje clave para el participante

> Medir la acción correcta en el momento equivocado también produce una integración incorrecta.

## Nota para el entrenador

Antes de revelar la causa, preguntar: “¿Qué hecho demuestra que esta persona se convirtió en lead?”. Guiar al grupo para que distinga visita, intención, clic, envío y confirmación.
