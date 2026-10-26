---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Revisarás los cambios y, si es necesario, proporcionarás comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya se ha destacado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre debemos revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene permiso de lectura en el repositorio y permiso de escritura **únicamente** en la rama que utilizará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno temporal independiente para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que creó el agente: la documentación del código. Para revisarla, utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que veas un aviso que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es lo esperado: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para ver la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado y la documentación TSDoc que se ha añadido. Los cambios exactos pueden variar. Busca comentarios TSDoc, convenciones de TypeScript y cabeceras de comentario; proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si aparece un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay alguno, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración en una PR no tiene por qué ser unidireccional. Puedes mencionar a Copilot en comentarios de la solicitud o en comentarios sobre líneas concretas de código. Copilot leerá esos comentarios e iniciará otra sesión para responder. Como los resultados no son deterministas, no podemos indicar un texto concreto. Por ejemplo, podrías pedirle que:

- añada una cabecera breve a cada archivo de código para explicar su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que solicites un cambio a la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores o pide otra mejora de documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para hacer las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marca la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR y que finalizan correctamente. Las comprobaciones de la aplicación Astro pueden tardar unos minutos.
9. **Opcional:** cambia a esta rama en Codespaces para probar manualmente la función. Abre el terminal de tu Codespace y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama de Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes pedir cambios de forma iterativa al agente en la nube; por ejemplo, ajustar cuántos juegos aparecen o añadir cabeceras y comentarios TSDoc (esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Añade un comentario nuevo en la pestaña **Conversation** para que Copilot inicie otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa la PR implementada por el agente personalizado de accesibilidad. Debe incluir tanto el modo de alto contraste solicitado en el ejercicio 3 como el modo claro que pediste en Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR tendrá la etiqueta **[WIP]**. Espera unos minutos hasta que termine.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación, en especial:
   - los controles de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
6. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Confirma que las comprobaciones del proyecto para la aplicación Astro finalizan correctamente; pueden tardar unos minutos.
10. **Opcional:** cambia a la rama de Copilot en Codespaces para probar las funciones de accesibilidad. Abre el terminal y ejecuta lo siguiente, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los controles de alto contraste y modo claro. Comprueba que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones según las buenas prácticas. Si detectas problemas o posibles mejoras de accesibilidad, puedes mencionarlo en un comentario con **@copilot**, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es especialmente útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para practicar, crea algunas incidencias más en tu repositorio y asígnalas a Copilot. Por ejemplo:

- crear un formulario para que las personas interesadas puedan registrarse en la página de detalles de los juegos.
- implementar paginación en la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados** del repositorio para observar más adelante su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de GitHub Copilot en la nube** y observaste cómo preparaba su entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** para accesibilidad y añadiste controles para los modos de alto contraste y claro.
- **Utilizaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Revisaste e iteraste sobre las PR del agente**, mencionaste a `@copilot` para pedir cambios y aprobaste los flujos de trabajo.

## Repaso y próximos pasos

Has terminado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y una conexión opcional con el agente en la nube mediante `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
