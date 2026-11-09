---
title: "Exercício 3 - Testar sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtros com o modo de agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real. Depois, publique sua ramificação e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que use o servidor para testar manualmente os filtros no navegador;
- publicar sua ramificação e abrir um pull request para o trabalho dos filtros.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é o padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de sua interação com o contexto, o LLM e as ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Assim, eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

O acesso é feito por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. O servidor gerencia a comunicação entre o agente e ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos disponíveis para o agente.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, incluindo navegação por páginas, preenchimento de formulários e seleção de botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Por segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração de MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto, por isso o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é preciso instalá-lo localmente. O VS Code se conecta por HTTP e, na primeira vez que o Copilot usa uma ferramenta, você se autentica no GitHub.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável, ideal para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, o servidor `playwright` é **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário no codespace, que não tem um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que não há um navegador, peça para executar `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas, o VS Code iniciará o servidor e solicitará que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir para confirmar que você confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciá-lo manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e solicitará sua aprovação na primeira vez que o Copilot precisar das ferramentas.

## Testar a funcionalidade de filtros

O [servidor MCP do Playwright][playwright-mcp-server] permite que o Copilot controle um navegador real. Em vez de testar o aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumi-los. Essa é uma maneira rápida de confirmar se a funcionalidade funciona como esperado, sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista) como faz uma tecnologia assistiva. Portanto, uma verificação funcional rápida também pode ajudar a avaliar a acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste os filtros:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o resumo aos critérios de aceitação da issue. Se algo parecer errado, faça perguntas adicionais ou peça que corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a ramificação e criar um pull request

Depois de confirmar que a funcionalidade funciona, abra um pull request (PR) para que a equipe possa revisá-la. Primeiro, publique a ramificação `filtering-vscode`.

1. Abra o painel **Source Control** no codespace e revise as mudanças feitas pelo Copilot.
2. Prepare as mudanças selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as mudanças realizadas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a ramificação ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive no github.com e com a interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue correspondente e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Na lista de agentes, confirme se **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitado que você se autentique. Siga as instruções.
5. Observe o Copilot pesquisando as issues, encontrando a correta e criando o PR.
6. Selecione o link criado pelo Copilot para revisar o PR, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar os filtros no navegador antes da entrega;
- publicou sua ramificação e abriu um pull request para o trabalho dos filtros.

Você usou um servidor MCP para *testar* uma funcionalidade, mas essa é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
