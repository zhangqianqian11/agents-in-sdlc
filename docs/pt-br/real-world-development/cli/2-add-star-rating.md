---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revisar o resultado no navegador encaminhado e mesclá-lo como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, é hora de fazer sua primeira mudança no projeto. Ela será pequena: os dados dos jogos já incluem avaliações por estrelas, mas os cartões da página inicial ainda não as mostram. Você pedirá ao agente que as exiba, revisará a mudança e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma ramificação de funcionalidade;
- pedir ao agente que faça uma pequena mudança no projeto;
- revisar a mudança com `/diff`;
- executar o aplicativo para confirmar a mudança em um navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cartões da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba em cada cartão a avaliação existente — uma mudança pequena e independente, perfeita para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a ramificação atualmente selecionados no terminal, em vez de criar uma worktree exclusiva. As conversas salvas permitem retomar a discussão depois, enquanto os arquivos e a ramificação continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente; a atividade de ferramentas enquanto ele explora e edita arquivos; e as mudanças que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a mudança

Vamos iniciar uma nova conversa para implementar a funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, inicie-o com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI.

## Revisar o diff

Todas as mudanças geradas por IA precisam ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as mudanças diretamente no Copilot CLI.

1. Insira `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o cartão do jogo mostra a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verificar as mudanças

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para examinarmos a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cartões dos jogos mostram as avaliações de zero a cinco.
4. Volte ao Copilot e peça para parar o servidor que iniciou:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e mesclar seu primeiro pull request

Você criou a funcionalidade. Agora, abra um pull request (PR) para mesclar o código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Insira `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança com o GitHub Copilot CLI. Você:

- iniciou uma conversa focada em uma ramificação de funcionalidade;
- orientou o agente para fazer uma pequena mudança nos cartões dos jogos;
- revisou a mudança com `/diff`;
- executou o aplicativo para confirmar a avaliação por estrelas no navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pela issue de filtros e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
