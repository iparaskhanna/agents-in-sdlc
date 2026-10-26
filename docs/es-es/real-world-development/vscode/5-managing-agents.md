---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Al utilizar el modo agente de GitHub Copilot, el agente trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo sucede en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- pedirás al agente, durante la sesión, que añada un control de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control de modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación, ampliarás el trabajo de la sesión existente.

## Supervisa el progreso de Copilot

La vista de Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en línea con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista de Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, recorre el chat para observar cada llamada a herramientas. Verás lecturas y ediciones de archivos, y los comandos del terminal que solicite ejecutar.
3. Cuando proponga una edición, las diferencias aparecerán en línea:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. Selecciona **Files changed** en la parte superior del chat para ver todos los archivos que ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que termine de implementar el modo de alto contraste. Al finalizar, mostrará un resumen de los cambios.

## Pide al agente que añada el modo claro

Cuando el modo de alto contraste esté listo, amplía la misma conversación para añadir un control de modo claro. Como continúas en la misma sesión, el agente conserva el contexto de los componentes modificados, los estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará el trabajo, explorará sus cambios anteriores y propondrá modificaciones para el modo claro.
3. Revisa cada cambio a medida que aparece y utiliza **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, refactoriza los controles de una forma que no te conviene—, detenlo con el botón **Stop** junto al campo de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Pide ajustes («simplifica el CSS; utiliza variables CSS en lugar de duplicar selectores»), corrige el enfoque («no añadas un tercer control; integra el modo claro en el panel actual») o amplía la tarea («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios propuestos

Antes de confirmar los cambios, revisa todo lo que haya modificado el agente.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se aplican las pautas del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición al llegar, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- orientaste la sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás tu trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que haya generado el agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
