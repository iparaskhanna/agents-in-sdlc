---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores, has pedido al agente de GitHub Copilot en la nube que se ocupara de tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, también es importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones que han realizado y orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una función adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Ya que se estaban actualizando los estilos del sitio y añadiendo controles para cambiar el modo, parecía lógico incluir esta función. Indicarás al agente que también implemente el modo claro.

## Revisa las tareas del agente en la nube

Consulta el estado de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberían aparecer, entre otras, las tareas que has asignado a Copilot:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso y revisa las operaciones que ha realizado Copilot. Por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintas operaciones para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de PR de la derecha, donde puedes ver la solicitud de incorporación de cambios y los archivos modificados.

## Orienta al agente en la nube

Ahora pedirás a Copilot que incluya un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto dependerá de cómo lo haya nombrado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos, hasta que la sesión indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes que confirman el inicio del trabajo.
3. En el cuadro **Steer active session while Copilot is working**, introduce este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir tu mensaje.

El proceso tardará varios minutos. Puedes aprovechar la espera para repasar lo que has aprendido y explorado.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Puedes seguir las tareas activas y terminadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o darle indicaciones.

Has explorado estos conceptos:

- utilizaste Mission Control y la página de agentes para supervisar tareas del agente en la nube.
- redirigiste una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
