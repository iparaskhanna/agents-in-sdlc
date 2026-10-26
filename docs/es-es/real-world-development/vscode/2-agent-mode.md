---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla de una aplicación suele requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo habitual consiste en localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de comentarios rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- permitir que el modo agente de Copilot explore el proyecto, identifique archivos y coordine cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de incorporarlos al código.

## Escenario

Como la lista de juegos sigue creciendo, quieres permitir que las personas filtren por categoría y editorial. En el ejercicio anterior creaste una función auxiliar para obtener las editoriales. Ahora completarás con el modo agente el resto del trabajo en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática para desarrolladores. Permite consultar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local en sus metadatos mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade mediante pequeños elementos `<script>` de Astro con las API DOM estándar.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo Astro. Sigue estos pasos en Codespaces:

1. Vuelve a tu Codespace. Tras el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en otra ventana, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace local del terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Las principales funciones son:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, abre su página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empieza por consultar el backlog: al crear el proyecto a partir de la plantilla se generaron incidencias automáticamente. Pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de Chat. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo sobre otro. Durante la creación del taller se utilizó Claude Sonnet 4.5 y se incluye en las instrucciones para que los resultados sean más coherentes. Sin embargo, los modelos de lenguaje son probabilísticos y las sugerencias pueden diferir de los ejemplos. Es normal y esperable.

> [!NOTE]
> Por su naturaleza probabilística, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt para preguntar por el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las tareas según las que considera más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene pautas para añadir funcionalidades al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar las directrices, dividir conjuntos extensos en archivos más manejables y aportar ejemplos o plantillas.

## Implementa la función de filtrado

Para completar el filtrado hay que realizar al menos tres cambios:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede encargarse de esas tareas.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Usa este prompt para pedir a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad, incluidos los auxiliares de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer cada cambio; no es necesario seleccionarlos mientras siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión de Copilot Agent Mode completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has utilizado el modo agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos relevantes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

Al crecer la lista de juegos, será necesario habilitar la paginación. Utiliza las habilidades de este ejercicio y pide a Copilot que la implemente. Ten en cuenta estas cuestiones:

- sigue las prácticas existentes y utiliza los archivos de instrucciones correspondientes.
- decide cómo debe funcionar la paginación: si las personas pueden elegir el tamaño de página o si será fijo.
- redacta un prompt con las indicaciones necesarias para obtener la implementación que quieres.
- quizá tengas que iterar con GitHub Copilot, pidiendo cambios y aportando contexto. Así suele ser el trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
