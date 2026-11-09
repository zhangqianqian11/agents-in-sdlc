---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar filtros a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo de uma organização e ajudar a construir as coisas certas da maneira certa. Esta é a primeira de várias lições nas quais você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar uma funcionalidade, garantirá que o código seja válido e que ela funcione como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma conversa e ramificação ao longo do fluxo de trabalho da funcionalidade. Normalmente, você usaria ramificações ou PRs diferentes para os vários tipos de arquivos, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir requisitos no modo Plan;
- implementar a funcionalidade no modo Autopilot;
- revisar o código;
- validar a funcionalidade manualmente em um navegador encaminhado.

Ao continuar o trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas é preciso decidir detalhes — como a combinação de categorias — antes de escrever o código. Você usará o modo Plan para resolver essas questões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos; na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreva o trabalho.
2. Criar um plano do que precisa ser implementado.
3. Criar e revisar o código.
4. Executar os testes para validá-lo.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos fluxos segue essa estrutura.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de verificação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere mudanças e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho nos filtros, volte ao Codespace e deixe o repositório e o terminal prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma ramificação para o trabalho de filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite trazer uma issue ou um pull request (PR) do GitHub para o contexto.

## Planejar a funcionalidade de filtros

O planejamento permite definir a abordagem para implementar uma funcionalidade ou executar tarefas antes de entregá-las ao Copilot. Para tarefas complexas, é sempre uma boa ideia reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, insira o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e definirá a melhor abordagem.

3. O Copilot poderá fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Assim como qualquer código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para conferir o resultado.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** ou **Autopilot**.
2. Insira `/diff` e inspecione a implementação dos filtros e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da exibição de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e pelas combinações definidas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu requisitos no modo Plan;
- implementou a funcionalidade no modo Autopilot;
- revisou o código;
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos aprofundar como gerar código que siga práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
