---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes dar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio que le ayuden a realizar ciertos tipos de trabajo. Los equipos pueden convertir su experiencia en agentes reutilizables: uno de accesibilidad que aplique las pautas de [WCAG][wcag], otro de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con al menos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la especialidad y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos. Ambos se definen principalmente con archivos Markdown y explican a una IA cómo realizar tareas. La diferencia más clara es que un **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad; para ello, podría recurrir a habilidades como la de flujos de trabajo de solicitudes de incorporación de cambios o la de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier función de IA, prueba y ajusta el resultado a tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás estos temas:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, con independencia de sus capacidades o preferencias visuales. Algunos comentarios recientes indican que a ciertas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar la función cuanto antes. Para ello utilizarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya tienes preparado un agente personalizado de accesibilidad. Revisa el archivo para entender cómo orientará a Copilot.

Vuelve a tu Codespace y abre el archivo del agente:

1. Abre `.github/agents/accessibility.md`.
2. Observa los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores y problemas frecuentes.

## Crea y asigna una incidencia

Mission Control es el lugar central para trabajar con agentes. Puedes asignar tareas al agente en la nube, supervisarlas e incluso redirigir el trabajo y ofrecer indicaciones adicionales. Asigna a Copilot la tarea de crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña de incidencias.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create**.
8. En el panel de la derecha, selecciona **Assign to Copilot**.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Cuadro de asignación del agente en la nube con las opciones de agente personalizado y accesibilidad resaltadas](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y orientar la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Lección siguiente: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
