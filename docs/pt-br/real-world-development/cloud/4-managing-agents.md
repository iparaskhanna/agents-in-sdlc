---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que realizasse três tarefas distintas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você vê tarefas com pull requests (PRs) abertos, explora as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe decidiu que seria um bom momento para adicionar também um modo claro. Como o trabalho já atualiza os estilos do site e adiciona controles, você orientará o agente para implementar os dois modos.

## Revisar as tarefas do agente de nuvem do Copilot

Confira o estado atual das tarefas atribuídas ao agente de nuvem:

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel à esquerda. Você deverá encontrar tarefas como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as etapas realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado e a abordagem do trabalho do Copilot.

4. Observe também o painel de pull requests à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão de modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos, até que a sessão indique que a configuração terminou e o trabalho começou.
3. Na caixa **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem na página de agentes, com a caixa para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer a solicitação e incluí-la no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot continuará com a tarefa atualizada. Ele incorporará a solicitação depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso pode levar alguns minutos. Aproveite para refletir sobre o que aprendeu até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro para monitorar e orientar tarefas do agente de nuvem. Você acompanhou tarefas ativas e concluídas, revisou o trabalho e redirecionou uma sessão em andamento para ajustar o escopo.

Você:

- explorou o painel central e a página de agentes para monitorar tarefas do agente de nuvem.
- orientou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui os recursos de acessibilidade, você pode [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
