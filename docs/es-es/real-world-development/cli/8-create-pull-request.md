---
title: "Lección 8 - Crear y combinar la PR de la función"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas están guardados en una sola rama. Ahora los revisarás en conjunto y abrirás una solicitud de incorporación de cambios. Combinaste tú mismo la PR de la valoración con estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, separaríamos la función, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de control de calidad en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás todos los cambios de la función y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisar el trabajo y confirmar que se ha combinado la PR.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Tailspin Toys quiere ahora automatizar el resto del trabajo de la PR, manteniendo el control de la autorización para combinarla en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los problemas que bloquean la PR: corrige errores en las comprobaciones de integración continua (CI), responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, si la configuración del repositorio lo permite, habilita la combinación automática de GitHub.

Hasta ahora, has seleccionado **Merge pull request** por tu cuenta. Agent Merge puede encargarse de esta tarea. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Deja que Agent Merge gestione la PR

Ya has creado todo el código. Ahora revisarás los cambios, crearás la PR y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes con el comando `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y la combinará cuando todo se haya completado correctamente.
7. Comprueba que Copilot muestre un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y, cuando la configuración del repositorio lo permita, realiza manualmente la combinación que ya has revisado.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionaste todos los cambios de la función y las pruebas de verificación;
- creaste la PR de filtrado;
- habilitaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
