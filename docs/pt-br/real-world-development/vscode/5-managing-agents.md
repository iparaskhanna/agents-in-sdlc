---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Agentes personalizados][previous-lesson]

No exercício anterior, você usou um agente personalizado de acessibilidade para implementar o modo de alto contraste. Enquanto esse trabalho acontece, a equipe de design solicita também um modo claro para quem considera o tema padrão escuro demais. Em vez de iniciar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um modo claro junto ao modo de alto contraste.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada alteração aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role o chat para acompanhar as chamadas de ferramentas, leituras e edições de arquivos e comandos de terminal.
3. Quando o agente propuser uma edição:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para revertê-la e permitir outra tentativa.
4. Abra o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando de terminal —, o agente pausará e mostrará a solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final com o resumo das alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, amplie a mesma conversa para adicionar um modo claro. Como você continua na mesma sessão, o agente mantém o contexto dos componentes alterados, da abordagem de estilos e do padrão de armazenamento local.

1. Envie este prompt de acompanhamento no Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações anteriores e proporá mudanças para o modo claro.
3. Revise cada nova edição e selecione **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada — por exemplo, refatorar a interface dos controles de maneira que você não quer —, interrompa-o com o botão **Stop** ao lado da entrada do chat e envie um prompt para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar do zero. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS usando variáveis em vez de duplicar seletores"), corrigir o rumo ("não crie um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois controles").

## Revisar o conjunto de alterações

Antes do commit, revise rapidamente tudo o que o agente alterou.

1. Abra **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver componentes Astro, estilos e possíveis testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se os padrões de acessibilidade do agente estão presentes: atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel **Source Control**. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações feitas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
