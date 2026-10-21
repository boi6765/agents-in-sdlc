---
title: "Ejercicio 2 - Añadir funciones con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele incluir localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a actualizar el código. Y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: empieza por explorar la estructura existente del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Vamos a ver cómo utilizar el modo agente para añadir funciones al sitio.

> [!NOTE]
> Aunque sus nombres sean parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos rápidos de comentarios y colaboración. El agente en la nube está diseñado para trabajar de forma asíncrona como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- utilizar el modo agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios los filtren por categoría y editorial. Ya añadiste un asistente de editoriales en el ejercicio anterior; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar una lista de juegos y ver sus detalles. Es una aplicación Astro única que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde sus metadatos, mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los asistentes reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts Astro `<script>` pequeños y específicos que utilizan las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo Astro. Sigue estos pasos para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre una ventana nueva del terminal en el Codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el comando siguiente para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir `http://localhost:4321` en una ventana del navegador, ciérralo seleccionando **x**.

5. Abre el sitio web. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro, `http://localhost:4321`, en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace a la URL localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado te permite acceder al servidor web que ahora se ejecuta en tu codespace.

### Explora el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso indicará cuáles debes utilizar.

La implementación inicial del sitio funciona, pero queremos ampliarlo con nuevas funciones. Empecemos por revisar el backlog. Al configurar el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te las muestre.

1. En la lista desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos del terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están recomendando un modelo por encima de los demás. Al crear el taller utilizamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código fueran relativamente parecidas y la experiencia resultara más coherente. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias que recibas pueden diferir de los ejemplos. Es algo normal y esperado.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría utilizar otro comando MCP, pero debería poder completar la tarea.

3. Pregunta a Copilot por las incidencias del backlog con este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista que genere Copilot.

Observa que Copilot incluso ha priorizado las incidencias según las que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que utilizarás para proporcionarle contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar las mejoras del sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, que incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones específicas de distintos tipos de archivo, como:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos grandes en otros más pequeños y mostrar ejemplos y plantillas.

## Implementa la función de filtrado

Para completar el filtrado, hay que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, hay que ejecutar las pruebas y asegurarse de que se superan antes de combinar los cambios en el código. ¡El modo agente de Copilot puede encargarse de estas tareas! Vamos a añadir esta función.

1. Puedes continuar en la conversación actual de Copilot o seleccionar **New Chat** para iniciar una nueva.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que el modelo seleccionado sigue siendo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la función según los requisitos de la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora primero el proyecto y localiza los archivos relacionados con la función. Debería encontrar los asistentes de la capa de datos, la interfaz y las pruebas. Después empezará a modificar los archivos y ejecutar las pruebas.

   ![Captura de pantalla de Copilot mientras explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Mientras tanto, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, aparecerá uno de estos botones para cada cambio. No es necesario seleccionarlos mientras Copilot sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del panel Copilot Chat que muestra un diálogo para confirmar que quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre la generación de código y la ejecución de pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat con el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y comprueba si hay posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has utilizado el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido a:

- utilizar el modo agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos;
- utilizar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos en el código.

Ahora vamos a [probar la función con el servidor Playwright MCP][next-lesson] y abrir una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que aumenta la lista de juegos, será necesario habilitar la paginación. Utiliza las habilidades que has aprendido en este ejercicio para pedir a Copilot que añada la paginación al sitio. Al hacerlo:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo;
- incluye en el mensaje las directrices necesarias para que Copilot implemente la solución que quieres;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
