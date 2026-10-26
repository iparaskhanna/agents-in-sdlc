---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Gerenciar agentes][previous-lesson]

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade — modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, enviar comentários para que o Copilot aprimore o trabalho.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a IA generativa. É sempre importante revisar o código gerado e seguir o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para suas alterações.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Os workflows do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive o acesso a servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem: o que adiciona documentação ao código. Use a interface padrão de PR do GitHub.com.

> [!NOTE]
> O PR pode exibir um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo chamadas a servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR exibirá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, incluindo os comentários TSDoc e outras documentações. As alterações exatas variam. Procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário: eles seguem os arquivos de instruções personalizados revisados no Exercício 2.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Se algum workflow estiver aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do PR. Aguarde a conclusão e confirme que as verificações do projeto para o aplicativo Astro foram aprovadas.

## Pedir alterações ao GitHub Copilot

O trabalho com o Copilot em um PR é interativo. Você pode mencioná-lo em comentários gerais ou comentários em linha do código. O Copilot verá os comentários e iniciará outra sessão para atendê-los. Como os resultados são probabilísticos, não há um prompt único a seguir. Você pode pedir, por exemplo:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, com uma descrição breve.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README descrevendo a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com outra pessoa. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Ele iniciará outra sessão para fazer as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir, aparecerá um novo commit no PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Volte à guia **Conversation** e role até a área de verificações.
6. Se os workflows estiverem aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

7. Aguarde as verificações do projeto para o aplicativo Astro e confirme que foram aprovadas.
8. **Opcional:** teste manualmente o recurso no Codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Você pode continuar iterando por meio de comentários na guia **Conversation**. Por exemplo, peça para ajustar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e TSDoc. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR exibirá **[WIP]**. Aguarde alguns minutos até a conclusão.

4. Selecione **Files changed** e revise o código gerado.
5. Examine a implementação, especialmente:
   - os controles de interface para alternar os modos.
   - o uso do armazenamento local para persistir preferências.
   - as alterações de CSS ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - o código JavaScript/TypeScript que gerencia a alternância de modos.

6. Volte à guia **Conversation** e role até a área de verificações.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Aguarde as verificações do projeto para o aplicativo Astro e confirme que foram aprovadas.
9. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou o Copilot na implementação das práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações.

## Exercício opcional: continue delegando

O agente de nuvem funciona bem quando recebe itens reais do backlog e você pode trabalhar em outras tarefas. Para praticar, registre mais issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes dos jogos.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar seu efeito no agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar uma sessão em andamento.
- **Revisou e aprimorou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos apresentam cenários relacionados em diferentes ambientes:

- **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP no IDE.
- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, agente personalizado de QA, MCP do Playwright, Agent Merge e delegação opcional ao agente de nuvem.

No seu próprio repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
