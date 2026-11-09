---
title: "Lição 6 - Validar a funcionalidade com MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar os filtros no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código é apenas uma parte do trabalho. Também precisamos usar dados e serviços externos e disponibilizar automações adicionais para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos incluídos na CLI e acesse mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível;
- pedir ao agente que controle um navegador e explore os filtros.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária, automatizando ainda mais as mudanças e aumentando a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

O acesso a ferramentas e recursos é feito por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com o seguinte comando:

   ```plaintext
   /mcp add
   ```

3. Para o nome, insira `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Pedir ao Copilot para explorar a funcionalidade com Playwright

Antes, você confirmou manualmente se a funcionalidade funciona. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário pedir explicitamente ao Copilot que use o servidor MCP: normalmente ele o identificará sozinho. Mas indicar a ferramenta pode ajudar a direcioná-lo, garantir resultados mais consistentes e economizar tokens.

2. Acompanhe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar sua funcionalidade em um navegador real a partir do Copilot CLI. Você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse os filtros.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
