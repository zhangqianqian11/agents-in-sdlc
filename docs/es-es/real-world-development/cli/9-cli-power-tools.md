---
title: "Lección 9: Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Estos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de una conversación;
- usarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de la CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando es necesario, y puedes inspeccionarla o compactarla tú mismo con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para decidir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas ventajas y cada desarrollador puede tener sus preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a todos a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller sobre el agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de una conversación;
- usaste `/model` para explorar los modelos disponibles;
- aprendiste cómo `/share` puede exportar o compartir una sesión;
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI tiene más comandos de barra y funciones por explorar. Terminemos el recorrido [repasando lo aprendido][next-lesson] y algunos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
