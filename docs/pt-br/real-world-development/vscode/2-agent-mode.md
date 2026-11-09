---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir mudanças em vários arquivos e operações como executar testes. O fluxo comum envolve localizar os arquivos necessários, fazer as mudanças, executar testes, depurar, identificar um arquivo esquecido, fazer outra atualização e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele age de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças;
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- revisar as mudanças e os testes gerados antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer qualquer mudança, explore o site para conhecer os recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes. É um único aplicativo Astro, que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro que usam as APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o único servidor de desenvolvimento Astro. Para executá-lo no GitHub Codespace:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma ramificação para o trabalho nos filtros:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore os recursos principais:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que é necessário.

A implementação inicial do site funciona, mas queremos adicionar recursos. Vamos começar revisando o backlog. A configuração do projeto a partir do modelo criou um backlog de issues automaticamente. Peça ao GitHub Copilot para exibi-las.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa mudanças em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criá-lo e o incluímos nas instruções para tornar as sugestões relativamente consistentes. Como os LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie o prompt a seguir para perguntar sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens conforme a utilidade que acredita que terão para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto. Vamos usar o arquivo de instruções de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, capacidade de teste e acessibilidade;
   - links para instruções específicas de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso facilita a reutilização, a divisão de instruções complexas em partes menores e a inclusão de exemplos e modelos.

## Implementar a funcionalidade de filtros

Para concluir os filtros, será necessário atualizar pelo menos três partes do aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento dos filtros;
- atualizar a página de listagem de jogos para incluir a interface de filtros.

Além disso, os testes precisam ser aprovados antes da mesclagem. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para começar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da exibição Chat, confirme se **Agent** está selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorando o projeto e localizando os arquivos relevantes da interface, da camada de dados e dos testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, conforme a complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as mudanças; não é necessário selecionar esses botões enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Solicitação no painel Copilot Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. Tudo bem: o Copilot alterna entre geração de código e testes até terminar a tarefa sem detectar erros.

   ![Sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- implementar funcionalidades na interface e na camada de dados com o modo de agente;
- explorar o projeto, identificar arquivos relevantes e coordenar mudanças;
- revisar mudanças e testes gerados antes de mesclá-los à base de código.

Agora, vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra – Implementar paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use o que aprendeu neste exercício e peça ao Copilot para implementar esse recurso. Considere:

- seguir as boas práticas existentes, incluindo os arquivos de instruções;
- decidir se as pessoas poderão escolher o tamanho da página ou se ele será fixo;
- fornecer orientações suficientes no prompt para obter a implementação desejada;
- iterar com o GitHub Copilot, pedindo mudanças e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
