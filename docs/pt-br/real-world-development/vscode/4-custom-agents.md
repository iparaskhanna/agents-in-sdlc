---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents`, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio para orientar o Copilot. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica a conformidade com a [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em Markdown e dizem à IA como realizar operações. A diferença mais simples é que o **agente personalizado** executa o trabalho, enquanto as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes; ele poderia chamar uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira correta de criar agentes personalizados. Como em qualquer uso de IA, teste e itere para descobrir o que funciona melhor no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você conhecerá:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre o texto e o plano de fundo. Para tratar essa questão, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, você quer implementar esse recurso rapidamente. Para isso, usará um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise o arquivo para entender como ele orienta o Copilot.

Volte ao Codespace e abra um terminal. Crie uma branch nova a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` na lista de agentes, na parte inferior da exibição Copilot Chat. Selecione um agente para aplicar suas instruções e ferramentas a uma sessão de chat.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção do modo e do modelo varia por exercício; cada etapa indica o que fazer quando necessário.

1. Na lista de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista de agentes na parte inferior da exibição Chat. Ela mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent**.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho do arquivo e um diff para revisão.
6. O indicador **Files changed** será atualizado à medida que o agente modificar arquivos.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot alterará arquivos reais do repositório, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes de IA especializados para tarefas e domínios específicos. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis.

Neste exercício, você aprendeu:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
