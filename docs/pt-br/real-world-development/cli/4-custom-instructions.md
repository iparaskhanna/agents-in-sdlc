---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer desenvolver, mas também *como* o código deve ser estruturado. Agora que você implementou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como instruções do repositório e instruções específicas por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o efeito dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, sem apenas repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar propósito, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar corretos quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser mantidas.

Os arquivos de instruções permitem fornecer ao Copilot as informações necessárias para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot. Você pode especificar convenções de código, bibliotecas e tipos de comentários, criando instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivo de instruções:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações relevantes para o projeto como um todo.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizados do projeto

O projeto inicial já inclui arquivos de instruções. Explore-os antes de fazer uma alteração para observar o efeito.

1. Volte ao Codespace.
2. No editor do Codespaces, abra `.github/copilot-instructions.md`.
3. Leia a descrição breve do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo`, que define o padrão de arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos reutilizáveis menores, com referências a exemplos.

## Atualizar as instruções para refletir as diretrizes da equipe

Os arquivos existentes são um bom começo, mas falta instruir o Copilot a adicionar comentários TSDoc ao novo TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, próxima à linha 35.
2. Adicione este item ao fim da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, feche e retome a conversa de filtragem para que o Copilot use as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as instruções atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Você:

- explorou como as instruções do repositório e os arquivos específicos por caminho chegam ao agente.
- atualizou o arquivo de instruções para exigir os padrões de código.
- observou o efeito dos arquivos de instruções no código.

Em seguida, você personalizará e executará a skill reutilizável `quality-checks` para realizar testes e lint de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
