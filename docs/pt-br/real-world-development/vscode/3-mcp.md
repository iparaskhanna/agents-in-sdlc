---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode acessar recursos externos ao editor. O Model Context Protocol (MCP) é um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de sua interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles acessam informações atualizadas por meio de recursos e executam ações em seu nome por meio de ferramentas.

Um servidor MCP faz a ponte entre o agente de IA e as ferramentas ou serviços externos, como APIs existentes e ferramentas locais (por exemplo, pacotes NPM). Cada servidor representa um conjunto de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: dá acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com o Playwright, incluindo navegação, preenchimento de formulários e seleção de botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para o ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cautela ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

A entrada `github` vem com o modelo do projeto e permitiu que o Copilot lesse o backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é necessário instalá-lo localmente. O VS Code se conecta por HTTP e solicita autenticação do GitHub na primeira utilização das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na máquina com `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, o que é necessário em um Codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: quando o Copilot precisa das ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Iniciar manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá confirmação na primeira utilização das ferramentas pelo Copilot.

## Testar o recurso de filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e relatará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas ou peça que o Copilot corrija o código antes de abrir um pull request.

Por baixo dos panos, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente analisa elementos estruturados e identificados — como botões, links e itens de lista — de maneira semelhante às tecnologias assistivas. Assim, uma verificação funcional rápida também oferece uma verificação básica de acessibilidade.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira utilização do Playwright MCP, talvez seja necessário baixar um navegador. Se estiver ausente, peça ao Copilot que execute `npx playwright install chromium` e tente novamente.

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para abrir um PR e permitir a revisão da equipe.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações realizadas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, peça que ele crie o PR, encontre a issue correspondente e associe-a.

1. Abra o painel Copilot Chat e selecione **New Chat**.
2. Confirme que **Agent** está selecionado na lista para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar sua conta.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou-o para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou sua branch e abriu um pull request.

Você usou um servidor MCP para testar um recurso, mas MCP é apenas uma maneira de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando disso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
