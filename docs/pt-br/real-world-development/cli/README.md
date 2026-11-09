---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
description: "Crie, verifique e entregue duas mudanças na Tailspin Toys enquanto aprende sobre modos, personalizações, ferramentas MCP e automação de pull requests no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** coloca o GitHub Copilot no terminal como um assistente de programação com agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você continue trabalhando sem alternar para um editor gráfico.

O workshop acompanha um fluxo de trabalho contínuo na Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma mudança pequena para exibir avaliações por estrelas, revise o resultado no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtros, defina a abordagem no modo Plan, desenvolva no Autopilot e revise no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho nos filtros.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar os filtros no navegador.
7. Crie um agente personalizado de garantia da qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise a mudança completa dos filtros e use o Agent Merge para o PR da funcionalidade.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional ao agente de nuvem.

Para manter o foco do workshop, você criará dois PRs: primeiro, para as avaliações por estrelas; depois, para os filtros, as atualizações de instruções, a atualização da skill, o perfil de QA e os testes. O fluxo de filtros e qualidade usa uma única conversa e ramificação para que você possa aproveitar o trabalho ao explorar cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | ----------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e conheça o ambiente |
| [2. Adicionar avaliações por estrelas: uma melhoria rápida][ex2] | Primeira mudança | Exiba as avaliações existentes e indique quando elas não estão disponíveis; depois, mescle seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade a partir da issue, crie-a com Autopilot e revise-a no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as aos filtros |
| [5. Personalizar e usar a skill quality-checks][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar a funcionalidade com MCP do Playwright][ex6] | Observação no navegador | Configure o MCP na CLI e inspecione o comportamento dos filtros |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna as evidências finais de verificação |
| [8. Criar e mesclar o PR da funcionalidade][ex8] | Revisão e mesclagem | Revise a mudança completa, crie o PR e use o Agent Merge |
| [9. Comandos de barra no GitHub Copilot CLI][ex9] | Recursos da CLI | Explore contexto, modelos, compartilhamento e delegação opcional ao agente de nuvem |
| [10. Recapitulação e próximos passos][ex10] | Resumo | Revise o fluxo, as personalizações reutilizáveis e outros recursos |
| [Opcional: Incorporar o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Permissão para criar um repositório e um Codespace
- [ ] Familiaridade básica com operações de terminal ou linha de comando

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, confirme com o administrador se o Copilot CLI está habilitado.

## Começar

[**Comece pelos pré-requisitos →**][ex0]

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
