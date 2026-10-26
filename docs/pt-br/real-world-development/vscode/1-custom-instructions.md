---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada maneira — ou se há informações que o Copilot deve conhecer —, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] são uma das principais maneiras de fornecer orientações para geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas e arquivos de instruções específicos por caminho.
- gerar a primeira parte dos dados de filtragem (um helper de editoras) com as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código gerado seguir o novo padrão.
- integrar as instruções atualizadas e a parte do recurso à branch `main`, para que o Copilot possa usar as orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é probabilístico. O objetivo é observar a tendência de mudança após atualizar as instruções, não obter exatamente o mesmo código.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar modo escuro e ter um visual moderno.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Os arquivos de instruções permitem fornecer ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo de código e os requisitos do projeto. Você pode especificar convenções de código, bibliotecas e tipos de comentários, com instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: arquivo único enviado ao Copilot em todas as solicitações do repositório. Inclua contexto do projeto relevante para a maioria das interações, como pilha tecnológica, visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface e testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para geração de código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> Copilot Chat, Copilot CLI e agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a outros formatos de arquivos de instruções][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto ilustram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em orientações gerais, como descrição e estrutura do projeto e padrões globais de código.
- Use arquivos `*.instructions.md` para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas específicas.
- Escreva em linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não existe uma única maneira correta de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Neste projeto, há arquivos para várias tarefas, como [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar esses arquivos. Cada ambiente oferece o recurso de uma maneira — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição correspondente explicará como usá-lo.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizados do projeto

Leia os arquivos de instruções incluídos no repositório pelo editor ou pela interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Leia a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**, aplicáveis às interações com o Copilot.
3. Abra `.github/instructions` e explore os arquivos para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` define um padrão relativo à raiz do repositório. Neste caso, corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Leia as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções e arquivos existentes do projeto. Isso permite dividir conjuntos de instruções maiores em arquivos reutilizáveis menores e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` descreve os padrões de código, mas ainda não exige documentação no próprio código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Siga as práticas recomendadas e crie uma branch para as alterações.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes dos exercícios, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção do modo e do modelo varia conforme o exercício; as etapas indicam o que selecionar quando necessário.

Para observar o efeito das instruções personalizadas, envie um prompt antes de atualizá-las. Depois, atualize-as e envie o mesmo prompt novamente.

1. Feche as guias abertas no editor para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da exibição Chat, selecione **Agent** para permitir que o Copilot aplique alterações aos arquivos.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, muitas vezes tanto ao helper quanto aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode gerar comentários mesmo sem essa orientação. Tudo bem; o objetivo é observar a consistência depois da atualização das instruções.

## Adicionar um padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações gerais do projeto. Adicione um padrão para orientar o Copilot.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, próxima à linha 27. Ela descreve os padrões do projeto, mas ainda não exige documentação no código.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, instruções podem ser definidas para todo o repositório em `.github/copilot-instructions.md` ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários recém-adicionada.

## Reenviar o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione novamente `src/lib/publishers.ts` para direcionar o Copilot ao arquivo.
3. Envie o mesmo prompt de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são controlados pelo mesmo sistema de controle de versão que os demais arquivos.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme que as instruções atualizadas e as alterações do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de apoio (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch enviada, crie um pull request associado à issue sobre a atualização das instruções. O Copilot pode localizar a issue e criar um PR que a encerra.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub para o workspace. Na primeira utilização, o VS Code pode pedir que você entre na conta do GitHub; siga as instruções. Você aprenderá sobre essa conexão (Model Context Protocol) em outro exercício.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue sobre as instruções e crie um PR da branch atual, descrevendo tanto as instruções quanto a base do helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot localizará a issue e criará o PR.
5. Quando solicitado a permitir ações do GitHub em seu nome, revise a ação e selecione **Allow** conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot que faça o merge e retorne sua branch local para `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Ao receber solicitações para permitir ações do GitHub e comandos de shell, revise cada ação e selecione **Allow** conforme apropriado.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um padrão geral a `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código seguir o novo padrão.
- integrar as instruções atualizadas e a base do helper de filtragem à branch `main`.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] para adicionar um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
