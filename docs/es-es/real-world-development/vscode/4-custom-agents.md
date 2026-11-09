---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para que realice ciertos tipos de trabajo de forma más eficaz. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y skills

Los agentes personalizados y los [skills de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es esta: el **agente personalizado** es quien realiza el trabajo, y los **skills** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar skills e incluso otros agentes como parte de su trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para hacerlo, podría utilizar skills como uno para el flujo de solicitudes de incorporación de cambios u otro para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que mejor se adapten a tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar la plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, quieres implementarlo cuanto antes. Para generar la funcionalidad, utilizarás un agente personalizado.

En este ejercicio aprenderás a:

- revisar un agente personalizado de accesibilidad existente;
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y abre un terminal. Cambia a una rama nueva basada en `main` para el trabajo de accesibilidad; así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores habituales que deben evitarse.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos lo indicarán cuando sea necesario.

1. Si todavía no está seleccionado, elige **Agent** en el selector de agentes de Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes de la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot hará cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas, si las hay.

En el ejercicio siguiente revisarás y dirigirás el trabajo en curso.

## Resumen y siguientes pasos

Esta lección ha tratado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados para tareas y dominios concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot para que realice el trabajo de forma más eficaz.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
