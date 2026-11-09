---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot debería conocer cierta información de fondo, asegúrate de que ese contexto esté disponible. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos con ámbito de ruta;
- generarás el primer fragmento de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás los cambios de las instrucciones y el filtrado en `main` para que Copilot utilice las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede desviarse de algunos estándares que indiques. Copilot no es determinista. El objetivo es observar la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no hacer que la salida coincida carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Debe añadirse documentación al código en forma de comentarios de documentación TSDoc.
- Debe añadirse un bloque de comentarios al inicio de cada archivo para describir su función.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar tus convenciones de programación preferidas, bibliotecas e incluso los tipos de comentarios que quieres incluir en el código. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, a fin de aportar contexto para tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes; por ejemplo, la pila tecnológica, una descripción general de lo que se desarrolla, buenas prácticas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` sirven para proporcionar directrices para lenguajes concretos, como TypeScript o Astro, o tareas como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube utilizan tanto los archivos a nivel de repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Además, Copilot [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una explicación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén `copilot-instructions.md` centrado en directrices a nivel de proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas según el tipo de archivo (pruebas unitarias, componentes Astro o la capa de datos) o la tarea.
- Usa lenguaje natural, mantén claras las directrices y proporciona ejemplos de cómo debería ser el código y de cómo no debería ser.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de utilizar la IA. La experimentación te permitirá descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían contar con un conjunto sólido de archivos de instrucciones. Puede que veas instrucciones para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo ofrece de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección de la interfaz que estés utilizando lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un `copilot-instructions.md` principal y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Coincidirá con cualquier archivo de pruebas TypeScript, por ejemplo, uno que coincida con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir al generar código. (Las rutas indicadas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios de documentación TSDoc y encabezados de comentario a nivel de archivo.

## Crea una rama para los cambios

Sigamos algunas buenas prácticas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos lo indicarán cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en el selector de agentes de Chat para que Copilot pueda modificar archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, normalmente al helper y a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios de documentación TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación incluso sin pedírselo. Si ocurre, no pasa nada: el objetivo sigue siendo observar la mejora de *coherencia* tras actualizar las instrucciones.

## Añade un nuevo estándar del repositorio

Como se ha indicado antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero todavía no incluye una regla para la documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares generales, como la regla sobre comentarios de documentación que acabas de añadir.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el búfer.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has dirigido a Copilot para que siga un nuevo estándar y lo aplique al código real sobre el que trabajarás en el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan mediante el control de código fuente. Confirmemos y enviemos la rama.

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que se han aplicado la actualización de instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ahora que has enviado la rama, crea una solicitud de incorporación de cambios (PR) vinculada a la incidencia del backlog sobre el estándar de documentación. Copilot puede buscar la incidencia, crear una PR que la cierre y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot utilice una herramienta de GitHub, VS Code puede pedirte que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual, describiendo tanto la actualización de las instrucciones como el helper inicial de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos del shell, revisa el comando y selecciona **Allow** según corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar los cambios de instrucciones y el helper inicial en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] para que Copilot añada una funcionalidad en varios puntos del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
