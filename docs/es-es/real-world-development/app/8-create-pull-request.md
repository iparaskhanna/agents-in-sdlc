---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste personalmente la PR de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisar el cambio y confirmarás que la PR está combinada.

## Escenario

Durante el flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener en manos del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar la base cuando es necesario. Combina el cambio en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando se combina la PR.

Hasta ahora, has seleccionado **Merge pull request** manualmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad para editar código y combinar sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Deja que Agent Merge gestione la PR

Con todo el código creado y revisado, deja que Agent Merge gestione la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambia a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge. Se encargará de:

- crear la solicitud de incorporación de cambios con un título y una descripción.
- si has iniciado la sesión desde una incidencia, mencionarla en el cuerpo de la descripción.
- volver a aplicar la base o resolver conflictos de combinación con la rama de destino.
- supervisar la integración continua y comprobar que se superen todas las verificaciones.
- supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y actualizar el código para resolverlos.
- combinar automáticamente la PR cuando todo haya terminado correctamente, si así lo autorizas.

Indica a Agent Merge que combine la PR cuando todo se haya superado.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que está marcada la opción **Merge pull request**.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve los bloqueos que se indiquen. Confirma que el estado de la PR es **Merged** antes de continuar. Haber habilitado el permiso para combinar o tener un agente en ejecución no confirma que la combinación se haya completado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo, y gestionar el proceso de la PR. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- autorizaste Agent Merge solo después de revisar el cambio y confirmaste que la PR estaba combinada.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
