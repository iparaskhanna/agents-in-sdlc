---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o Copilot necesita información de fondo, ese contexto tiene que estar disponible. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices y ayudan a Copilot a entender no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos aplicables a rutas concretas.
- añadirás un nuevo estándar general al repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás un prompt de *antes y después*. El agente en la nube trabaja de forma asíncrona con incidencias de GitHub, así que es más difícil demostrar su efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las PR que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La parte superior de cada archivo debe incluir un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un archivo único que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general relevante para la mayoría de las conversaciones o solicitudes de CLI, como la pila tecnológica, una descripción del proyecto, buenas prácticas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo concretos, por ejemplo, para TypeScript o Astro, o para crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de código ni las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta [los formatos compatibles][custom-instructions-support].

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre la creación de archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices del proyecto, como qué se está desarrollando, su estructura y los estándares generales de programación.
- Usa archivos `*.instructions.md` para dar instrucciones sobre tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Utiliza lenguaje natural y directrices claras. Incluye ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te ayudará a descubrir qué funciona en cada proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, quizá observes que hay instrucciones para muchas tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); las lecciones correspondientes lo indicarán cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos que incluye este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a todas tus interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior contiene un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos pequeños y reutilizables, y señalar ejemplos para que Copilot los siga. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de comentarios de archivo.

## Añade un nuevo estándar del repositorio

Este será el único cambio que harás en este ejercicio: indicarás que la documentación del código debe escribirse mediante comentarios TSDoc y una cabecera de comentario a nivel de archivo. El agente en la nube utilizará estas directrices cuando le asignes incidencias más adelante.

Antes de editar, crea una rama (el agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia):

1. Abre el terminal de Codespaces y crea una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha explicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Fíjate en que describe los estándares de programación, pero todavía no incluye directrices para la documentación del código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en `.github/copilot-instructions.md` para aplicarse a todo el repositorio, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo general del repositorio es el lugar adecuado para estándares comunes como la regla de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización

El agente en la nube lee las instrucciones de la rama a la que se asigna la incidencia. En el ejercicio siguiente, Copilot creará una rama a partir de `main`, por lo que estos cambios deben llegar a `main` para que el agente los utilice.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar el cambio directamente en `main`. La rama se utiliza para reflejar cómo se trabajaría en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones de este proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Podrás ver cómo se aplica ese estándar en las PR que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
