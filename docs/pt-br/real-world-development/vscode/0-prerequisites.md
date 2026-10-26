---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare o ambiente. Você criará uma cópia do repositório Tailspin Toys, iniciará um [Codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Crie uma cópia do repositório a partir do [modelo][template-repository]. A nova cópia conterá todos os arquivos necessários para os exercícios.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Crie sua cópia selecionando o botão **Use this template** na página do repositório e, depois, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**); você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar seu repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma.

## Criar um Codespace

Você usará um Codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento em nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, depois, o botão **+** para criar um Codespace.

    ![Crie um novo Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace leva alguns minutos, mas ainda é bem mais rápida do que instalar todos os serviços manualmente. Enquanto aguarda, você pode explorar outros recursos do GitHub Copilot.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos oferecem os pré-requisitos necessários para uma experiência tranquila. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, [instale-a no Visual Studio Code Marketplace][copilot-chat-extension]. Ou abra a exibição **Extensions** no Visual Studio Code, procure **GitHub Copilot Chat** e selecione **Install**.

Talvez seja necessário autenticar sua conta do GitHub para habilitar a extensão.

1. Volte ao Codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem como `Hello world` na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativá-lo.
4. Se você não estiver autenticado, aparecerá uma solicitação para entrar na conta do GitHub. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns! Você criou uma cópia do repositório do laboratório e iniciou a criação do Codespace, que usará para desenvolver código.

## Próxima etapa

Agora, coloque o Copilot para trabalhar. Continue para o [Exercício 1: Instruções personalizadas][next-lesson], onde você ensinará as convenções do projeto ao Copilot.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
