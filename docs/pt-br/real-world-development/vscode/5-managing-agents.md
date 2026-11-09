---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe mudanças, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff na conversa, aceitar ou rejeitar mudanças individuais e orientar o agente com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar um botão de alternância do modo claro ao lado do modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez outra solicitação: também quer um modo claro para as pessoas que acham o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão atual para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição do Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição do Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar as chamadas de ferramentas: leituras e edições de arquivos e comandos de terminal cuja execução ele solicita.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a mudança;
   - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando de terminal), o agente pausará e mostrará a opção **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Uma mensagem final resumirá as mudanças.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste pronto, você ampliará a mesma conversa para adicionar um botão de alternância do modo claro. Como continuará na mesma sessão, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as mudanças já feitas e propondo a implementação do modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface dos botões de uma forma que você não queira —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro botão; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois modos").

## Revisar o conjunto de mudanças

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, estilos e testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação por teclado, HTML semântico e persistência pelo armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou mudanças e enviou prompts para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho de um agente em tempo real;
- aceitou e rejeitou edições individuais;
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as mudanças produzidas pelo agente personalizado local durante o laboratório.

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
