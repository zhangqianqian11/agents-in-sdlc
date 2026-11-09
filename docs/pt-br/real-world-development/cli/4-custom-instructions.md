---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código dos filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma forma específica, essa orientação deve estar disponível para o Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou os filtros, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos aplicados a caminhos específicos chegam ao agente;
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas. Entre eles:

- Os comentários devem explicar a intenção e decisões que não sejam óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Os arquivos de instruções darão ao Copilot as informações necessárias para alinhar suas tarefas a essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos do projeto. Você pode especificar convenções de código, bibliotecas e tipos de comentários. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto adequado à tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O formato e o suporte a outros tipos de instruções variam conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui vários arquivos de instruções. Vamos explorá-los antes de fazer uma mudança para observar seu efeito.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Examine a descrição do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` na parte superior. Ele define um padrão glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

## Atualizar os arquivos de instruções para refletir as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que o TypeScript recém-gerado tenha comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir ao final da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, encerre e retome a conversa sobre os filtros para que o Copilot carregue as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa sobre os filtros ainda estiver aberta, insira `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na ramificação `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa sobre os filtros e confirme se a ramificação `game-filters-cli` está selecionada.
3. Peça ao Copilot para aplicar as instruções atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Insira `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão à funcionalidade. Você:

- explorou como as instruções do repositório e os arquivos aplicados a caminhos específicos chegam ao agente;
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para executar lint e testes de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
