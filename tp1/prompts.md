# Prompts — TP 1

El registro del proceso, en orden. Iteración de prompts en una sola conversación de Gemini Canvas.

---

## 1 — Prompt inicial

```text
Construí un ingreso de teléfono incremental.
Estructura:
- <header> con un <h1> "Número telefónico" y un <p> "Ingrese su número telefónico, con codigo de area".
- <main> que contenga un contenedor flex o grid con 10 <input readonly> (para los números), y debajo un grupo con 3 <button>: "+1", "Siguiente Número" y "Limpiar".
- <footer> con un <p> que diga "Al terminar, presione 'Enviar'" y un <button disabled> más pequeño que diga "Enviar".
Estilo:
- Paleta y diseño estilo Windows 98 clásico (gris #c0c0c0, bordes biselados outset/inset, azul oscuro #000080).
- Tipografía: Arial, sans-serif para todo.
- Usa variables CSS en :root para los colores.
- El casillero "seleccionado" debe tener un background o border distinto para que el usuario sepa dónde está parado.
Comportamiento:
- Estado: un array "valores" de 10 posiciones (inicializadas en 0) y un "indiceActual" que inicia en 9 (el extremo derecho).
- Eventos al click:
1. En botón "+1": suma 1 al valor en "indiceActual" (si llega a 9, vuelve a 0) y actualiza ese <input> en el DOM.
2. En botón "Siguiente Número": mueve el "indiceActual" un paso a la izquierda. Refleja el cambio de selección en el DOM. Si el "indiceActual" llega a 0 (extremo izquierdo), quita el atributo disabled del botón "Enviar" del <footer>.
En botón "Limpiar": restablece el array valores a ceros, vuelve el indiceActual a 9, y vuelve a deshabilitar el botón "Enviar". Actualiza el DOM para reflejar el reseteo.
Constraints:
- Un solo archivo HTML, con el CSS en un <style> y el JS en un <script>.
- Vanilla JS, sin frameworks ni dependencias externas.
- No usar <canvas>: quiero poder ver el estado reflejado en el DOM. 
```

**Qué intentaba lograr:** el artefacto entero de una sola vez, nombrando las capas estructurales, la paleta de estilo Windows 98 y el comportamiento interactivo basado en un estado explícito (`valores` e `indiceActual`) para sentar la mecánica base de la Bad UI.

**Qué devolvió:** el esqueleto funcional en un solo archivo con el comportamiento solicitado. Hubo un problema técnico con la extensión del navegador que bloqueó el previsualizador del Canvas, por lo que el código se descargó para probarlo.

**Qué hice con eso:** lo probé de forma local en el navegador. Al confirmar que la trampa de interacción funcionaba correctamente, pasé a diseñar la segunda iteración para agregar el castigo al usuario.

---

## 2 — Iterar sobre el estado: error y reseteo

```text
Agregale al ingreso de teléfono un estado para manejar un error "isErrorOpen" inicializado en false.
- Al recibir un click en "Enviar", tomá los índices 0, 1 y 2 del array "valores" y unilos en un solo número (por ejemplo, si son 0, 1 y 0, el valor es 10). Si el valor es <= 11: isErrorOpen = true.
Mutación del DOM: Mostrá un pop-up modal centrado por encima de la interfaz. Estilo Windows 98 estricto: borde outset, barra de título azul oscuro con un botón "X" en la esquina, un ícono de cruz roja de error típica de Windows en el cuerpo, y el texto "Código de área incorrecto".
- Al hacer click en la "X" del pop-up, isErrorOpen = false. Además, ejecutá la misma lógica del botón "Limpiar": el array "valores" vuelve todo a 0, y el "indiceActual" vuelve a 9. Modifica el DOM para ocultar el pop-up. Actualizá los 10 <input> para que vuelvan a mostrar "0", mové el indicador visual de selección de nuevo al extremo derecho, y volvé a deshabilitar (con el atributo disabled) al botón "Enviar" del <footer>. 
```

**Qué intentaba lograr:** agregar el castigo principal de la Bad UI usando el Patrón 2. La clave era definir explícitamente la condición de vuelta (cerrar el pop-up de error) atada a un reseteo total del estado (`valores` a 0, `indiceActual` a 9). Si el usuario comete un error tras tantos clicks en el falso trámite automotor, el sistema lo devuelve a la casilla de salida sin piedad.

**Qué devolvió:** el modal de error integrado por encima de la interfaz y la lógica de lectura de los primeros tres índices funcionando. Al hacer click en la "X", el DOM se limpia por completo reflejando la pérdida del progreso.

**Qué hice con eso:** validé la mecánica de frustración y pasé a diseñar la tercera iteración para manejar la condición opuesta: el mensaje de "éxito" burocrático que también termina expulsando al usuario al inicio.

---

## 3 — Envolver en un flujo de dos pasos (Wizard)

```text
Vamos a convertir esta interfaz en un flujo de dos pasos (wizard), manteniendo todo en un solo archivo, con el mismo estilo de Windows 98:

Envolvé todo el código actual del teléfono en un <section id="paso2"> y que quede oculto.
Creá un nuevo <section id="paso1"> que sea la pantalla inicial. Debe tener un título "Dirección Nacional - Alta de Dominio". Adentro, un formulario normal con <input type="text"> para Nombre, Apellido, DNI y Patente. Debajo, un botón "Validar Datos".
Creá un <div id="loader"> (también oculto por defecto) que simule una ventana de carga de Windows 98 con el texto "Validando en base de datos central..." y una barra de progreso falsa.
Comportamiento (Estado y Eventos):

Estado nuevo: pasoActual = 1.
Evento de transición: Al hacer click en "Validar Datos" del Paso 1, ocultá el #paso1 y mostrá el #loader.
Usá un setTimeout de 3 segundos. Cuando termine, ocultá el #loader, mostrá el #paso2 (la interfaz del teléfono) y actualizá el subtítulo del teléfono para que diga: "Dominio validado correctamente. Para finalizar el trámite, ingrese su teléfono de contacto local".
La lógica del teléfono incremental y del pop-up de error que ya armamos debe mantenerse intacta en el Paso 2.
```

**Qué intentaba lograr:** envolver la Bad UI en un contexto burocrático que genere falsa confianza. Al simular un trámite real de transferencia o alta de dominio automotor, el contraste con el formulario absurdo del teléfono es mucho más frustrante. El uso del loader de 3 segundos suma ansiedad antes de revelar la trampa.

**Qué devolvió:** la estructura de pasos funcionando correctamente usando la manipulación del DOM. El formulario inicial carga con normalidad, la transición con `setTimeout` bloquea la pantalla simulando el procesamiento, y finalmente revela el Paso 2 manteniendo intacta la lógica de error y reseteo de los prompts anteriores.

**Qué hice con eso:** validé que la secuencia de cambio de DOM no rompiera el estado del teléfono. Una vez confirmado que el flujo del trámite funcionaba, quedó todo el terreno preparado para la cuarta iteración: el falso mensaje de éxito que devuelve al usuario al inicio.

---

## 4 — Iterar sobre el estado: éxito burocrático y bucle infinito

```text
Modificá la lógica del evento del botón "Enviar" para manejar el caso de éxito. Agregá un nuevo estado "isSuccessOpen" inicializado en false.
Al click en "Enviar": Mantené la lógica de lectura de los índices 0, 1 y 2. Si el código de área es < 11: ejecutá la lógica del pop-up de error que ya armamos. Si el código de área es >= 11 (condición de exito): Efecto en Estado: isSuccessOpen = true. Mostrá un nuevo pop-up de éxito. Estilo Windows 98: borde outset, barra de título azul oscuro con su botón "X", un ícono de información (la típica letra "i" azul en un globo de diálogo de Windows) y un mensaje central que diga "El trámite fue completado correctamente". También agregá un botón "Aceptar" debajo del texto.
Al click en la "X" o en el botón "Aceptar" del pop-up de éxito: Efecto en el Estado: isSuccessOpen = false, pasoActual = 1, reseteá el array valores a ceros y el indiceActual a 9.
Ocultá el pop-up de éxito. Ocultá el #paso2 y volvé a mostrar el #paso1 (el formulario de datos inicial). Vaciá los <input> de Nombre, Apellido, DNI y Patente para que el bucle vuelva a empezar desde cero. 
```

**Qué intentaba lograr:** cerrar el flujo de la Bad UI con la burla final. En lugar de que el éxito libere al usuario tras completar el trámite, el botón de "Aceptar" actúa como una trampa. Al vaciar los inputs del primer paso y devolver el DOM a la pantalla inicial, el alta del dominio se convierte en un bucle infinito del que no se puede escapar. 

**Qué devolvió:** el modal de éxito con la estética clásica de Windows 98 (ícono de información incluido) y la lógica de ruteo funcionando a la perfección. Al hacer click en "Aceptar" o en la "X", el sistema oculta el paso 2, muestra el paso 1 y limpia el DOM eliminando todos los strings de los inputs iniciales.

**Qué hice con eso:** con esta última iteración di por terminado el código. El archivo quedó completamente funcional, en un solo `index.html`, cumpliendo con todos los constraints técnicos. Con esto, el registro del proceso para `prompts.md` ya está listo para acompañar la entrega.