---
title: "Lección 10: Recapitulación y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En este taller:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog;
- añadiste valoraciones por estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR);
- empezaste por la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste con Autopilot y lo revisaste en el modo Interactive;
- guiaste al agente con instrucciones personalizadas, personalizaste la skill `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados del skill y las pruebas del navegador;
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para la PR correspondiente;
- exploraste comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en el `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Combinamos todo el trabajo en una sola PR para agilizar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el sitio en el navegador y usando Copilot para explorar el sitio mediante MCP. El skill `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas añadidas deben cubrir carencias reales; si QA no necesita añadir pruebas, también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una skill, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones fijan estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar en un enfoque antes de empezar, **Interactive** para seguir interviniendo en cambios específicos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia sustancialmente el resultado.

## Más por explorar

Ya has completado el flujo de trabajo principal. Aquí tienes algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
