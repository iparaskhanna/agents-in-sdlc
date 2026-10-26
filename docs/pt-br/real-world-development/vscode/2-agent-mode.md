---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Mesmo uma pequena atualização de um aplicativo costuma exigir alterações em vários arquivos e tarefas como executar testes. Uma pessoa desenvolvedora normalmente precisa localizar os arquivos, fazer as mudanças, executar testes, depurar, descobrir o que faltou e repetir o processo.

É para isso que serve o modo de agente do Copilot.

O modo de agente pode trabalhar de forma autônoma no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige problemas encontrados. Neste exercício, você usará o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente explora o projeto, identifica arquivos relevantes e coordena mudanças.
- o modo de agente implementa recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você criou um helper de editoras; agora concluirá as alterações restantes na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de alterar o código, explore o site para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter com o Drizzle ORM; não há uma API de back-end separada nem uma estrutura de interface executada no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um `<script>` Astro pequeno e específico, com APIs DOM padrão.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro.

1. Volte ao Codespace. Após o exercício anterior, você deve estar na branch `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, aparecerá um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site segurando <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor Web do Codespace.

### Explorar o site

Com o site em execução, explore os recursos:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá outra página com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as instruções indicam a seleção necessária.

O site inicial funciona, mas vamos adicionar novos recursos. O modelo do projeto criou um backlog de issues automaticamente. Peça ao GitHub Copilot que o mostre.

1. Selecione **Agent** na lista de agentes da exibição Chat. O agente planeja e implementa mudanças em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> Os autores do workshop não estão recomendando um modelo específico. O Claude Sonnet 4.5 foi usado ao desenvolver o workshop para tentar manter as sugestões consistentes. Como os LLMs são probabilísticos, as sugestões podem variar; isso é esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP e ainda assim concluir a tarefa.

3. Envie este prompt para consultar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo [de instruções da interface][ui-instructions], que orienta como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações para implementar funcionalidades:
   - visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos complexos em arquivos menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, são necessárias pelo menos três alterações:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Além disso, os testes precisam passar antes do merge. O modo de agente pode realizar essas tarefas.

1. Continue na conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente os requisitos da issue relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os helpers de dados, a interface e os testes. Em seguida, ele altera os arquivos e executa os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot pode levar alguns minutos para explorar, modificar arquivos e executar testes. Enquanto trabalha, o editor pode exibir os botões **Keep** e **Undo**; não é necessário selecioná-los até que o trabalho termine.

7. Quando solicitado, selecione **Continue** para executar os testes.
8. Podem ocorrer pausas e falhas temporárias nos testes. O Copilot alterna entre gerar código e testar até concluir a tarefa ou deixar de detectar erros.
9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendeu como:

- o modo de agente implementa recursos na interface e na camada de dados.
- o Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar as alterações e os testes gerados antes do merge.

Agora, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site. Considere as práticas recomendadas e os arquivos de instruções existentes, decida se o tamanho da página será configurável ou fixo e forneça contexto suficiente no prompt. Talvez seja necessário iterar e pedir alterações; esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
