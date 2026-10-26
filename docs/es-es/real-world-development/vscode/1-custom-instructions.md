---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene que Copilot tenga a su alcance las indicaciones necesarias. Hay varias formas de compartir contexto específico; una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos aplicables a rutas concretas.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar de editoriales) con las instrucciones actuales.
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo cambia el código generado.
- confirmarás los cambios de instrucciones y de la funcionalidad en `main` para que Copilot pueda usar las directrices nuevas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no respetar algunos estándares. Copilot es no determinista. El objetivo es observar la *tendencia* de cambio en el comportamiento tras actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys sigue directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La parte superior de cada archivo debe incluir un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones, darás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos. Puedes especificar convenciones de programación, bibliotecas y comentarios preferidos, así como crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general relevante para la mayoría de las interacciones, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y directrices globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan pautas para tareas o tipos de archivo específicos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md; consulta [los formatos compatibles][custom-instructions-support].

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices generales: qué se está desarrollando, la estructura del proyecto y sus estándares de programación.
- Usa archivos `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Utiliza lenguaje natural y directrices claras, con ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de utilizar la IA. Experimenta para descubrir qué funciona mejor en cada proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. En este proyecto hay, por ejemplo, directrices para las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada entorno ofrece una opción distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); las lecciones correspondientes la indicarán cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto ya incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Explóralos antes de hacer cambios.

1. Abre `.github/copilot-instructions.md`.
2. Observa la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas tus interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior define un patrón glob (relativo a la raíz del repositorio); por ejemplo, coincide con archivos de prueba TypeScript `**/*.test.ts`.
5. Lee las instrucciones para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones y a archivos del proyecto. Así se pueden dividir conjuntos extensos en archivos pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para comentarios TSDoc y cabeceras de comentario de archivo.

## Crea una rama para los cambios

Sigue estas buenas prácticas y crea una rama para el trabajo.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

Envía un prompt con las instrucciones actuales y, más adelante, vuelve a enviarlo para comparar los resultados.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo tenga el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ir la función auxiliar.
3. Selecciona **Agent** en el selector de agentes de Chat para que Copilot pueda aplicar cambios.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al archivo auxiliar como a sus pruebas.
6. Observa que la función propuesta tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Estas características proceden de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y la cabecera de comentario del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, quizá añada comentarios TSDoc aunque no se los hayas pedido. No pasa nada: el objetivo sigue siendo observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar del repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que aún no incluye directrices para la documentación en el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones pueden estar en `.github/copilot-instructions.md` para aplicarse a todo el repositorio o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo general del repositorio es el lugar adecuado para estándares de todo el proyecto, como la regla de comentarios que acabas de añadir.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para dar a Copilot el contexto adecuado.
3. Envía el **mismo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que se utilizará en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan mediante control de código fuente, igual que los demás archivos del proyecto.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que estén presentes las actualizaciones de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya cambiado para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios auxiliares adicionales (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios vinculada a la incidencia sobre los estándares de documentación. Pide a Copilot que encuentre la incidencia, cree una PR que la cierre y la combine.

> [!NOTE]
> Este es el primer ejercicio en que Copilot actúa en GitHub. La plantilla del proyecto ya incluye las herramientas de GitHub para tu entorno. La primera vez que Copilot utilice una, VS Code quizá te pida iniciar sesión; sigue los pasos para permitirlo. Más adelante aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia de actualización de las instrucciones y cree una PR desde la rama actual. Indícale que describa tanto los cambios en las instrucciones como la base de la función auxiliar de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa la operación y selecciona **Allow** si procede.
6. Después de crear la PR, pídele que la combine en `main` y que actualice la rama local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para utilizar GitHub y ejecutar comandos de shell, revisa la operación y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto aplica el nuevo estándar.
- confirmar las actualizaciones de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda continuar.

A continuación, aplicarás estas instrucciones en el [modo agente][next-lesson] para que Copilot añada una funcionalidad en varios puntos del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
