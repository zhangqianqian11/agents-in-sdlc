---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Defina-os em arquivos Markdown na pasta `.github/agents` do repositório e forneça instruções, boas práticas, padrões de código e conhecimento do domínio. Assim, as equipes podem registrar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** executa o trabalho; as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes). Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Ele poderia chamar skills de workflow de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para encontrar o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o plano de fundo. Para resolver a questão, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, queremos implementar essa funcionalidade rapidamente. Neste exercício, você:

- revisará um agente personalizado de acessibilidade existente;
- usará esse agente no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e crie uma ramificação nova a partir de `main` para o trabalho de acessibilidade (o PR dos filtros do Exercício 3 permanecerá separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as próximas seções, que apresentam:
   - responsabilidades principais ao gerar código para um site acessível;
   - boas práticas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - erros comuns a evitar.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista de agentes na parte inferior da exibição do Copilot Chat. Você pode selecionar um agente para limitar a sessão às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam conforme o exercício; cada etapa informa o que é necessário.

1. Na lista de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista de agentes na parte inferior da exibição Chat (ela mostra o agente ativo; por padrão, **default**).
3. Selecione **Accessibility agent** na lista.
4. Envie o prompt a seguir ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat explorará a base de código, proporá mudanças e aplicará edições aos arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem feitas, o indicador **Files changed** será atualizado com os arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo mudanças reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como definir agentes personalizados;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as mudanças à medida que são feitas e adicionando um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
