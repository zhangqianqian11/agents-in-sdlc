---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas do agente com pull requests (PRs) abertos. É possível explorar as operações realizadas e até orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem;
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como já havia um trabalho em andamento para atualizar o estilo do site e incluir alternância de modos, pareceu lógico adicionar essa funcionalidade. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código;
   - gerar APIs para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações já realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório;
   - criar o ambiente de trabalho do Copilot;
   - configurar servidores MCP;
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel do pull request que aparece à direita. Ele permite ver o PR e os arquivos alterados para acompanhar o trabalho.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato dependerá do nome escolhido pelo Copilot e do estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece a solicitação e a incorpora ao fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, seu centro para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Com esse painel, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona tarefas em andamento para ajustar o escopo ou dar orientações adicionais.

Você explorou estes conceitos:

- usar o painel de controle e a página de agentes para monitorar as tarefas do agente de nuvem;
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

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
