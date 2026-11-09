---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise os filtros, as instruções, a skill atualizada, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação dos filtros, as atualizações das instruções, a atualização da skill, o perfil de garantia da qualidade (QA) e os testes estão salvos em uma única ramificação. Agora, revise tudo em conjunto e abra um pull request. Você mesclou o PR de avaliação por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de trabalho de filtros e qualidade em uma conversa e ramificação, e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionar toda a mudança na funcionalidade e as evidências de verificação;
- criar o PR dos filtros;
- ativar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do trabalho nos filtros, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o restante do trabalho no PR sem tirar da pessoa desenvolvedora o controle de autorização da mesclagem.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Quando ativado, o Copilot resolve os impedimentos do PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub se o repositório permitir.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de ativar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisá-lo em conjunto, criar o PR e deixar que o Agent Merge cuide do restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente inserindo `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme se o Copilot mostra uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de ramificações, filas de mesclagem, configurações do repositório ou permissões ausentes. Se houver um bloqueio, leia o motivo informado e, quando o repositório permitir, faça manualmente a mesclagem revisada.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, como a geração, o teste e a validação do código e o gerenciamento do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionou a mudança completa da funcionalidade e as evidências de verificação;
- criou o PR dos filtros;
- ativou o Agent Merge somente depois da revisão e confirmou a mesclagem do PR.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
