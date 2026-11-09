---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [codespace][codespaces] para trabalhar e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório que será usado no laboratório, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para os exercícios.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver fazendo o workshop em um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do repositório de modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você o usará mais adiante.

> [!NOTE]
> **Seu backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você usará essas issues no workshop; não é necessário criar nenhuma.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório recém-criado.
2. Selecione o botão verde **Code**.

    ![Selecionar o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um codespace.

    ![Criar um codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará alguns minutos, mas ainda será muito mais rápida do que instalar todos os serviços manualmente. Aproveite esse tempo para explorar outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [contêiner de desenvolvimento local][dev-containers]. Ambos incluem os pré-requisitos necessários. Se preferir trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deverá estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension]. Você também pode abrir a exibição Extensions no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub.

1. Volte ao codespace.
2. Se o Copilot Chat não estiver visível à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deverá ativar o Copilot Chat.
4. Se você não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do codespace que usará ao começar a escrever código.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para [Exercício 1: Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
