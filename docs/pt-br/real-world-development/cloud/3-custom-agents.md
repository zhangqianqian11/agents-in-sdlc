---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes com arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, boas práticas, padrões de código e conhecimento específico do domínio para que ele execute determinados tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis: um agente de acessibilidade que aplica a conformidade com as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes) como parte do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para isso, ele poderia chamar skills como uma skill de workflow de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única forma certa de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com os agentes personalizados, você explorará:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontaram que o tema escuro atual é difícil de ler devido ao contraste insuficiente entre o texto e o plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, queremos implementar essa funcionalidade o mais rápido possível. Para isso, usaremos um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar seu conteúdo e entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as próximas seções, que apresentam:
   - responsabilidades principais ao gerar código para um site acessível;
   - boas práticas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - erros comuns a evitar.

## Criar e atribuir uma issue

O painel de agentes é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e redirecionar uma sessão em andamento ou fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. No campo **Title**, insira `Add high contrast mode to website`.
6. No campo **Description**, insira:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Atribuição do agente de nuvem com o agente personalizado Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] pelo painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
