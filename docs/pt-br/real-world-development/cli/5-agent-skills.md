---
title: "Lição 5 - Personalizar e usar uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Você já validou manualmente o código e usou arquivos de instruções para garantir que ele siga os padrões da equipe. E quanto aos testes, ao lint e às outras partes da integração contínua (CI)?

Para essas tarefas, **skills de agente** são ideais. Elas ajudam o Copilot a executar operações como essas da maneira correta.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos de apoio que o agente carrega quando necessário. Em geral, são pastas com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O Markdown contém frontmatter com nome e descrição, uma visão geral do que a skill faz e orientações sobre quando usá-la. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar em `.github/skills` no projeto, como recurso compartilhado pela equipe, ou na pasta de skills do usuário, `~/.copilot/skills`.

## Explorar a skill

Vamos explorar a skill `quality-checks` que a equipe Tailspin Toys criou para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início. A descrição ajuda o Copilot a saber quando chamar a skill.
3. Leia as instruções e observe como orientam o Copilot durante os testes e o lint.
4. Observe que ainda não há uma seção **Results output formatting**.

## Executar a skill antes de fazer uma alteração

As skills podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Peça ao Copilot que execute as três verificações.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie o prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalizar o relatório

Vamos melhorar o relatório para indicar o que foi executado, se passou, falhou ou foi bloqueado e o que as ferramentas informaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie o mesmo prompt usado antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato do relatório.
- recarregou e executou a skill.

Essa alteração será incluída no PR do recurso de filtragem. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
