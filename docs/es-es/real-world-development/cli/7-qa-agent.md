---
title: "Lección 7 - Crear y utilizar un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección:

- explorarás cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- crearás e inspeccionarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de control de calidad y revisarás sus resultados según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar ese proceso y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, sus instrucciones y las herramientas disponibles. En este taller definirás una función de control de calidad en `.github/agents/qa.agent.md` y la seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado cumplen distintas funciones. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo utilizar estas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a las otras personalizaciones ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la función, pedirás a Copilot que cree un perfil de control de calidad reutilizable. El perfil definirá tanto las comprobaciones que debe realizar como los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de utilizar el agente, revisa el perfil para confirmar que Copilot ha recogido el proceso y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio cambie la función cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. El campo `description` es obligatorio; `name` es opcional, pero permite mostrar un nombre claro al agente.
3. Lee las instrucciones del perfil y confirma que el agente parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que el agente informa de las pruebas con evidencias, pide permiso antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado no incluye alguna de estas responsabilidades o limitaciones, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad de la incidencia

Copilot CLI carga los agentes del proyecto cuando se inicia una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda utilizar el contexto de la incidencia y las decisiones de planificación.

1. Activa el agente con este mensaje:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá aún no aparezca en la lista. El comando anterior lo activará igualmente.

2. Envía este mensaje al agente de control de calidad para que revise la función:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has incorporado una función especializada y reutilizable al flujo de trabajo, y has revisado sus resultados. En esta lección:

- exploraste cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP;
- creaste e inspeccionaste un perfil reutilizable de control de calidad;
- seleccionaste el agente de control de calidad y revisaste sus resultados según la incidencia de filtrado.

La implementación, la actualización de la habilidad, el perfil de control de calidad, las pruebas y el informe de verificación ya están listos para su revisión. A continuación, [los reunirás en una PR de la función y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
