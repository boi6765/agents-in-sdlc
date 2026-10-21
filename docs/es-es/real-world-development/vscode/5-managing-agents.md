---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Al utilizar el modo agente de GitHub Copilot, el agente trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code muestra continuamente las llamadas a herramientas y las ediciones de archivos. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios individuales y orientar la conversación con mensajes de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas en los archivos;
- orientarás al agente durante la sesión para que añada un selector de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado otra función: un modo claro para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación desde cero, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas. Verás lecturas y ediciones de archivos, así como los comandos del terminal cuya ejecución solicite.
3. Cuando proponga una edición, aparecerán las diferencias en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para consultar todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará un aviso **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final, debería mostrar un mensaje con un resumen de los cambios.

## Orienta al agente para que añada un modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación para añadir un selector de modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes que ha modificado, el enfoque de estilos y el patrón de almacenamiento local que ha configurado.

1. Envía este mensaje de seguimiento en el campo de entrada de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará el trabajo, explorará los cambios existentes y propondrá modificaciones para añadir el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no te convence, por ejemplo, si intenta reorganizar de una forma que no te gusta la interfaz de los selectores, puedes detenerlo con el botón **Stop** junto al campo de chat y enviarle un mensaje para aclarar lo que quieres.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Utiliza mensajes de seguimiento para solicitar ajustes («simplifica el CSS: utiliza variables en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos modos»).

## Revisa los archivos modificados

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha tocado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se siguen las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición a medida que aparecía, has aceptado o rechazado cambios y has enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real;
- aceptaste y rechazaste ediciones individuales;
- orientaste la sesión con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás tu trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
