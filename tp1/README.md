# TP 1 — Ingreso Telefónico Incremental

Un falso trámite automotor que arranca con un formulario normal y luego te obliga a ingresar tu teléfono número por número usando un botón de "+1". Si te equivocaste en un número no perdona. Funciona perfecto y usarlo es una tortura, tal como se esperaba.

## Cómo se ejecuta

Doble click en `index.html`. Un solo archivo, sin dependencias.

## Qué me propuse construir

Una bad UI basada en la frustración de los trámites burocráticos. Arranca simulando un sistema obsoleto pero funcional en un sistema operativo prehistórico, donde el usuario completa sus datos (Nombre, DNI, Patente) para una transferencia o alta de un vehículo. Al principio parece funcionar con normalidad, hasta el momento de encontrarse atrapado en un ingreso numérico insufrible, causando mayor frustración de lo que un trámite de este estilo normalmente haría. Salió en cuatro iteraciones dentro de una sola conversación de Gemini Canvas.

## Decisiones que tomé yo

**El contraste de pasos.** La trampa no funciona si el diseño es hostil desde el segundo cero. El paso 1 pide datos con normalidad y luego usa un *loader* de 3 segundos para darle un toque de realismo burocrático y hacerle creer al usuario que el sistema está trabajando.

**Ingreso incremental de derecha a izquierda.** Obliga al usuario a pensar y rellenar su propio número, de a uno, y al revés (por si no fuera suficiente tortura). El estado se maneja explícitamente con un array `valores` de 10 posiciones y un `indiceActual` que arranca en el extremo derecho. 

**Estética Windows 98.** Aquí me ayude con otra IA para conseguir la estética y los colores propios de Windows 98. Variables CSS en `:root` para los grises `#c0c0c0`, bordes biselados `outset`/`inset` y el clásico azul `#000080`. Le da un aura de software gubernamental que no se actualiza hace treinta años, validando lo ridículo del proceso.

**El castigo es un bucle infinito.** La interfaz evalúa los índices 0, 1 y 2 para validar el código de área. Si te equivocás (< 11), un pop-up te avisa del error y te resetea todo al paso 1. Si lo ponés bien, te felicita con un pop-up de éxito... y también te resetea todo al paso 1, vaciando tus datos.

## Qué salió mal y cómo lo corregí

Al armar la lógica de los pop-ups, el modelo por defecto tendía a asumir el comportamiento amigable y estándar: cerrar la ventana pop-up y dejar el estado donde estaba. Perdonar el error arruina el concepto.

La corrección fue puramente estructural en los prompts (Patrón 2). Hubo que definir explícitamente la "condición de vuelta" para atar el click de la "X" (o del botón "Aceptar") a un reseteo total del estado: devolver el array `valores` a 0, reiniciar el `indiceActual` a 9, y forzar la mutación del DOM para volver a mostrar la pantalla inicial completamente vacía.

Lo que funcionó perfectamente por diseño fue delegar el estado en el array antes de tocar el DOM. Pedirle a la IA que leyera el código de área concatenando los índices del array evitó que el modelo intentara extraer valores directo de los `<input>` de la pantalla.

## Prompts

El registro completo está en `prompts.md`. Los que más pesaron fueron obviamente el primero, que fija el artefacto entero y la interacción incremental, y los de reordenamiento de estado (iteraciones 3 y 4), que transformaron un formulario incómodo en una trampa de la que no se puede salir.
