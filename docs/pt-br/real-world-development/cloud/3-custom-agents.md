---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents`, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica a conformidade com a [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em Markdown e orientam a IA sobre como realizar operações. A diferença mais simples é que um **agente personalizado** executa o trabalho, enquanto as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills ou outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes; ele poderia chamar uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira correta de criar um agente personalizado. Teste e itere para descobrir o que funciona melhor no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você conhecerá:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro devido ao contraste insuficiente entre o texto e o plano de fundo. Para tratar essa questão, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é fundamental, você quer implementar o recurso rapidamente usando um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise-o para entender como orientará o Copilot.

Volte ao Codespace e revise o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes:
   - responsabilidades principais ao gerar código para sites acessíveis.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir uma issue

O painel de agentes é o local central para trabalhar com os agentes do ambiente. Nele, você pode atribuir tarefas ao agente de nuvem, monitorá-las, redirecioná-las e fornecer orientações adicionais. Vamos pedir ao Copilot que implemente o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Add high contrast mode to website`.
6. Defina a **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Captura de tela da atribuição ao agente de nuvem, com o agente personalizado e Accessibility destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e domínios específicos. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis.

Você aprendeu:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [acompanhar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar a organização para usar agentes personalizados][org-custom-agents]
- [Preparar a empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
