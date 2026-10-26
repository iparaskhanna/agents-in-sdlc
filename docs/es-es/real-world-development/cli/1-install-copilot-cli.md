---
title: "Lección 1 - Instala GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en Codespaces, familiarízate con la herramienta y localiza la incidencia de filtrado precargada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Permite delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- localizarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a utilizar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI incorpora esa capacidad al terminal, donde muchos desarrolladores ya trabajan. En esta lección lo instalarás, te autenticarás y lo dejarás listo para el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI mostrará este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio Tailspin Toys y acepta seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Puede que ya estés autenticado mediante tu sesión de GitHub en Codespaces. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarízate con la herramienta

Los comandos que escribes en el prompt normal del shell se ejecutan directamente en Codespaces. Cuando inicias Copilot CLI, puedes escribir instrucciones en lenguaje natural para el agente y utilizar comandos de barra para controlar la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que localice la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de hacer un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama activa en el terminal; no crea un árbol de trabajo automáticamente. Antes de cada cambio crearás una rama de funcionalidad.

## Utiliza el atajo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas fuera de los permisos ya establecidos. En este taller, volverás a iniciarlo con `--yolo`, un atajo que has aceptado y que elimina esas solicitudes de confirmación dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Utilízalo solo en un entorno aislado, como Codespaces o una máquina virtual; no lo configures como valor predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de GitHub de lectura y escritura que utilizarás en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. Codespaces limita el acceso al equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta sencilla sobre el proyecto para confirmar que la conversación funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, utilizarás `copilot --resume` para volver a la misma conversación de la funcionalidad y a su rama.

## Resumen y próximos pasos

¡Enhorabuena! En esta lección:

- instalaste GitHub Copilot CLI con npm.
- te autenticaste con tu cuenta de GitHub.
- confiaste en el repositorio del taller y probaste una conversación.
- localizaste la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [harás tu primer cambio acotado][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
