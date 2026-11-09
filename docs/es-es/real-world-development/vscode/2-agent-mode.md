---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización... y así sucesivamente.

El modo agente de Copilot ayuda con este proceso.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta e interacción rápidos. El agente en la nube está diseñado para trabajar de forma asíncrona como parte del equipo, interactuando contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. Ya has añadido un helper de editoriales en el ejercicio anterior; ahora completarás con el modo agente de Copilot el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito y las API DOM estándar.

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha proporcionado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Ejecútala en GitHub Codespaces con estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace `localhost` en el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos lo indicarán cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir más capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla se generó automáticamente una lista de incidencias; pide a GitHub Copilot que la muestre.

1. Selecciona **Agent** en el selector de agentes de Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por ningún modelo. Durante su desarrollo utilizamos Claude Sonnet 4.5 y lo incluimos en estas instrucciones para que las sugerencias sean relativamente coherentes y la experiencia sea buena. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede utilizar otro comando MCP y aun así completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo abordar los cambios de funcionalidad del sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura del proceso para seleccionar el archivo de instrucciones de la interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los helpers de la capa de datos y la interfaz como las pruebas. Después modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad, puede tardar varios minutos. Durante el proceso pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no hace falta seleccionar nada mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir capacidades al sitio Tailspin Toys. Hemos visto cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos;
- el modo agente puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio para pedir a Copilot que la implemente en el sitio. Al preparar el prompt:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación, por ejemplo, si el usuario podrá elegir el tamaño de página o será fijo;
- proporciona a Copilot las directrices necesarias para crear la implementación que quieres;
- itera con GitHub Copilot, pidiendo cambios y aportando contexto. Es el flujo normal de trabajo con Copilot.

## Recursos

- [Copilot ask, edit y agent: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
