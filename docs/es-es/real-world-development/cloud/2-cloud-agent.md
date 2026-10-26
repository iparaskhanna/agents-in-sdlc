---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que debe actualizarse o funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en asuntos de mayor alcance.

En este ejercicio explorarás cómo utilizar el agente en la nube para:

- personalizar el entorno en el que genera código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de acotar claramente las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: apenas hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. En la página de detalles de cada juego se deberían mostrar otros títulos de la misma categoría para que la gente pueda seguir explorando. Todavía no necesitan una implementación pulida: quieren probar la experiencia de usuario. Esta tarea bloquea el trabajo, pero hay incidencias de mayor prioridad.

Ambos ejemplos son tareas que pueden quedar relegadas fácilmente y que conviene asignar al agente en la nube. El agente puede trabajar en segundo plano mientras el desarrollador se ocupa de otras tareas y, más tarde, revisar el resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al asignársela, Copilot crea un borrador de PR para seguir el progreso, prepara un entorno y empieza a trabajar. Puedes consultar su sesión mientras trabaja o cuando haya terminado. Cuando la solución esté lista para revisión, Copilot te mencionará en la PR.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en detalle, así que [acota cuidadosamente las tareas que asignes al agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente, revisa en modo de solo lectura los archivos de instrucciones incluidos en el repositorio para reconocer más adelante su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca en la PR comentarios TSDoc, convenciones de TypeScript y cabeceras de comentario. Estas directrices proceden de los archivos de instrucciones. Volverás a señalarlas al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

El desarrollo suele requerir un entorno concreto y scripts de configuración para que todo esté listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. De este modo, puede acceder a las herramientas y dependencias necesarias. El flujo ya está configurado para facilitar el taller y garantizar que Copilot tenga Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación utiliza Astro y Drizzle/libSQL).
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

      # Migra y rellena la base de datos SQLite local para que haya datos disponibles al compilar y probar.
      - name: Set up the database
        run: npm run db:setup
```

Es parecido a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos puntos importantes:

- Contiene un solo trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar desatendida, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot es especialmente útil para crear documentación, así que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano; revisarás los cambios en un ejercicio posterior.

1. Abre tu repositorio en una pestaña nueva de github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Abre la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno mediante el flujo de trabajo anterior, analizando el proyecto y decidiendo cómo abordar el problema.

13. Revisa la lista y las tareas que llevará a cabo.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Quizá tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código y su estado, se detiene a pensar y elegir un plan, y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharás esa posibilidad asignándole otra incidencia y, mientras tanto, añadiendo una funcionalidad a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja importante del agente en la nube es que permite repartir el trabajo: tú puedes ocuparte de una tarea y el agente de otra. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero ese tiempo puede dedicarse a otras tareas. Asígnasela al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
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
8. En el panel de la derecha, selecciona **Assign to Copilot**.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después aparecerá un grupo de 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software: no tienes que supervisarlo continuamente, sino revisar el resultado cuando termine. Ahora puedes crear y utilizar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. Puede ayudar a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno en el que se genera código.
- garantizar que las operaciones se realicen de forma segura.
- acotar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, crearás y utilizarás agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Lección siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
