---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones, si es que hay alguna, no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funciones que llevan tiempo en el backlog porque no ha habido ocasión de implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas y acelerar el desarrollo, lo que permite a los desarrolladores centrarse en cuestiones más amplias.

Con el agente de GitHub Copilot en la nube explorarás:

- cómo personalizar el entorno para generar código;
- cómo asegurarte de que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene cierta deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que no hay documentación. Como primer paso, el equipo quiere que se añadan comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los usuarios puedan seguir explorando. Por ahora no necesita una implementación pulida; quiere algo que pueda utilizar para probar la experiencia de usuario. Esta tarea está bloqueada, aunque hay otras incidencias de mayor prioridad.

Estas son tareas que pueden quedar relegadas fácilmente y que conviene asignar al agente de GitHub Copilot en la nube. Así, el agente trabaja en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede volver para revisar el trabajo y comprobar que todo esté como se esperaba.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de una forma parecida a un desarrollador. Y, al igual que cuando se trabaja con una persona, hay varias maneras de hacerlo, entre ellas, [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará una solicitud de incorporación de cambios en borrador para hacer un seguimiento del trabajo, configurará un entorno y empezará a trabajar en la tarea. Puedes consultar la sesión de Copilot mientras sigue en curso o después de que termine. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Hasta las operaciones que parecen sencillas se vuelven complejas cuando se examinan en detalle. Por tanto, [define con cuidado el alcance de las tareas que asignas al agente de GitHub Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, revisa los archivos de instrucciones que ya incluye el repositorio, sin modificarlos, para que luego puedas apreciar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub del repositorio o en un codespace que ya tengas en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, sobre todo las expectativas relativas a las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera que aparezcan en la solicitud de incorporación de cambios. Esos elementos se derivan de los archivos de instrucciones, que volveremos a mencionar al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de GitHub Copilot en la nube

La creación de código suele requerir un entorno concreto y la ejecución de algunos scripts de configuración para comprobar que todo está en buen estado. Lo mismo ocurre al asignar tareas a Copilot, que las realiza de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], en el archivo `.github/workflows/copilot-setup-steps.yml`, que se ejecutará antes de que el agente empiece a trabajar. Así tendrá acceso a las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado para que el laboratorio sea más fluido y permita aprender. Se asegura de que Copilot tenga acceso a Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y sembrada para la única aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para realizar operaciones.
    permissions:
      # Si quieres clonar el repositorio durante los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en la base de datos SQLite local para que las compilaciones y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El evento `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que el flujo funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Todos conocen la importancia de la documentación, pero muchos proyectos tienen información desactualizada o carecen de ella. Este tipo de deuda técnica suele quedar sin resolver, lo que ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena tarea para asignarle al agente de GitHub Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior revisarás su trabajo.

1. En una pestaña nueva del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación de incidencias.
4. Selecciona **Blank issue** para crear una incidencia vacía.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un par de minutos y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está creando el entorno —ejecutando el flujo de trabajo mencionado anteriormente—, analizando el proyecto y decidiendo cómo abordar mejor el problema.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios; debería aparecer una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Quizá tengas que actualizar la ventana para ver el indicador actualizado.

16. Recorre la sesión en directo y observa cómo Copilot resuelve el problema. Verás cómo explora el código, analiza el estado, se detiene a pensar y decidir el enfoque adecuado y, después, crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente de GitHub Copilot en la nube es realizar tareas de forma asíncrona, para que puedas centrarte en otras. Aprovecharás esta función asignándole otra tarea al agente y, mientras tanto, escribirás código para añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente de GitHub Copilot en la nube es que permite repartir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Vamos a asignársela al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla vacía.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, debería aparecer un símbolo 👀 en el primer comentario de la incidencia, que indica que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. El agente de GitHub Copilot en la nube funciona de manera similar a un ingeniero de software, así que no es necesario supervisarlo continuamente. Revísalo cuando termine. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funciones o migrar código de un framework a otro.

Has aprendido:

- cómo personalizar el entorno para generar código;
- cómo asegurarte de que las operaciones se realicen de forma segura;
- por qué es importante definir incidencias con un alcance claro;
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y utilizar agentes personalizados. El [agente de GitHub Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Información sobre el agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente de GitHub Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
