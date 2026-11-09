---
title: "Lição 5 - Personalizar e usar a skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar os filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código é apenas uma parte do trabalho. Já validamos manualmente que o código funciona e usamos arquivos de instruções para garantir que ele siga os padrões. Mas e os testes, o lint e as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente;
- personalizar o formato dos resultados;
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente agrupam instruções reutilizáveis de tarefas, scripts executáveis e recursos de apoio, que o agente carrega sob demanda. No nível básico, uma skill é uma pasta com o mesmo nome e um arquivo Markdown chamado `SKILL.md`. O Markdown inclui frontmatter com nome e descrição, que definem a skill, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` da Tailspin Toys contém somente `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta pessoal de skills `~/.copilot/skills`.

## Explorar a skill

Vamos explorar a skill criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a decidir quando chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot nas etapas de teste e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes de fazer mudanças

As skills podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa sobre os filtros no modo Interactive.
2. Envie o prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalizar o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

Agora, vamos testar a mudança. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Insira:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Leia o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa mudança acompanhará os filtros no PR da funcionalidade. Em seguida, você permitirá que o Copilot interaja diretamente com o site [pelo servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Workflow de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
