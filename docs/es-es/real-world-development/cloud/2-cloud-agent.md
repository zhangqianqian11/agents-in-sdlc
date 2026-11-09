---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en cuestiones de mayor alcance.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno para generar código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante delimitar claramente las incidencias;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver cierta deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. En la página de detalles de cada juego quiere mostrar títulos relacionados de la misma categoría para que las personas puedan seguir explorando. Todavía no necesita una implementación pulida; solo quiere algo que sirva para las pruebas de aceptación de la experiencia. La tarea es un bloqueo, aunque por ahora hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas con facilidad y son adecuadas para asignarlas al agente de Copilot en la nube. El agente puede trabajar de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo de Copilot.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna, Copilot crea una PR en borrador para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de terminar. Cuando la solución propuesta esté lista para revisión, te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones que parecen sencillas pueden ser complejas; por eso, [delimita con cuidado las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por añadir IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en funcionamiento. Si no, este es un buen momento para echarle un vistazo. Antes de asignar tareas al agente en la nube, revisa los archivos de instrucciones que incluye el repositorio para observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, comprueba si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas directrices proceden de los archivos de instrucciones. Volveremos a revisarlas al inspeccionar PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Para crear código suele ser necesario un entorno concreto y ejecutar scripts de configuración. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así, el agente dispone de las herramientas y dependencias necesarias. El taller ya configura este flujo para que el agente tenga Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos necesarios para estos pasos. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitarán el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación y la interfaz: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
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

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta incidencia es adecuada para el agente en la nube. Trabajará en segundo plano y revisarás su resultado en un ejercicio posterior.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que se acaba de generar; tendrá un título parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno, ejecutando el flujo de trabajo descrito anteriormente, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR; deberías ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código y su estado, se detiene a pensar y decidir un plan y, después, crea código.

El proceso puede tardar varios minutos. Una ventaja importante del agente en la nube es que realiza tareas de forma asíncrona y te permite ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, añadiremos funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente de Copilot en la nube es poder dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Asígnasela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot**.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerán varios emojis 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software, así que no hace falta supervisarlo continuamente; revisa el resultado cuando termine. Mientras tanto, exploremos los agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y resuelva deuda técnica, cree funcionalidades o ayude a migrar código entre frameworks.

Has aprendido:

- cómo personalizar el entorno para generar código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante delimitar claramente las incidencias;
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que exploramos en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
