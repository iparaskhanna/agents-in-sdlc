---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Quase todas as organizações enfrentam algum nível de dívida técnica: problemas de segurança não resolvidos, código legado que precisa ser atualizado ou solicitações de recursos paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para atualizar código e adicionar funcionalidades de maneira autônoma. Ao concluir, ele cria um pull request (PR) de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, é possível delegar tarefas trabalhosas e se concentrar em questões mais amplas.

Neste exercício, você vai explorar como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em mau estado: ela praticamente não existe. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo.

A equipe de design também quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros jogos relacionados — títulos da mesma categoria — para que as pessoas continuem explorando. Ainda não é necessário um recurso refinado; a equipe quer algo que possa usar para testar a experiência. Embora essa tarefa seja um bloqueio, outras issues têm prioridade maior no momento.

Esses tipos de tarefa acabam sendo adiados com facilidade e são bons candidatos para o agente de nuvem. Ele trabalha de forma assíncrona enquanto a pessoa desenvolvedora se concentra em outras tarefas e, depois, revisa o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um PR de rascunho para acompanhar o trabalho, configura um ambiente e começa a tarefa. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não elimina a necessidade de pensar. Mesmo operações aparentemente simples podem esconder complexidade; portanto, defina cuidadosamente o escopo das tarefas para o agente de nuvem, conforme as [práticas recomendadas][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação. Caso contrário, agora é um bom momento para conhecê-lo. Antes de atribuir tarefas ao agente de nuvem, examine os arquivos de instruções incluídos no repositório, sem alterá-los, para identificar seus efeitos posteriormente.

Abra os arquivos abaixo pela interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, especialmente as expectativas para convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no PR os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário definidos por esses arquivos. A revisão desses PRs será retomada em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código normalmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente de execução. Você pode personalizá-lo com um [workflow especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para disponibilizar as ferramentas e dependências necessárias. O workflow já foi preparado para este laboratório. Ele instala Node.js e as dependências do projeto, navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente após a conclusão delas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer outro workflow do GitHub, mas há alguns detalhes importantes:

- Ele contém um job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona sem aguardar uma tarefa do Copilot.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção da base de código e a integração de novas pessoas. O Copilot é especialmente útil para criar documentação, então esta é uma boa issue para atribuir ao agente de nuvem. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Abra a guia **Pull Requests**.
11. Abra o PR recém-gerado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde e atualize o navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente — executando o workflow descrito acima —, analisando o projeto e escolhendo uma abordagem.

13. Revise a lista e as tarefas propostas.
14. Role a linha do tempo do pull request até encontrar uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Navegue pela sessão em tempo real e observe como o Copilot resolve a tarefa: explora o código e seu estado, pausa para decidir a abordagem e cria código.

O trabalho pode levar alguns minutos. Uma das vantagens do agente de nuvem é justamente executar tarefas de forma assíncrona. Você aproveitará esse recurso atribuindo outra tarefa ao agente e, em seguida, trabalhará em uma funcionalidade do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

O agente de nuvem permite dividir o trabalho: você pode se concentrar em uma tarefa enquanto ele executa outra. Mesmo que a seção de jogos relacionados não demore muito, esse tempo pode ser usado em outras atividades. Vamos atribuí-la ao agente.

1. Volte ao repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot**.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente funciona de maneira semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona e tratar dívidas técnicas, criar recursos ou migrar código entre frameworks.

Você explorou:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, apresentadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
