---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introdução com Signalr 1. x e MVC 4 | Microsoft Docs'
author: bradygaster
description: Use o Signalr ASP.NET e o ASP.NET MVC 4 para criar um aplicativo de chat em tempo real.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579565"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Introdução com Signalr 1. x e MVC 4

por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tutorial mostra como usar o Signalr ASP.NET para criar um aplicativo de chat em tempo real. Você adicionará o Signalr a um aplicativo MVC 4 e criará um modo de exibição de chat para enviar e exibir mensagens.

## <a name="overview"></a>Visão geral

Este tutorial apresenta o desenvolvimento de aplicativos Web em tempo real com o ASP.NET Signalr e o ASP.NET MVC 4. O tutorial usa o mesmo código de aplicativo de chat que o [introdução o tutorial do signalr](tutorial-getting-started-with-signalr.md), mas mostra como adicioná-lo a um aplicativo MVC 4 com base no modelo de Internet.

Neste tópico, você aprenderá as seguintes tarefas de desenvolvimento do Signalr:

- Adicionar a biblioteca do Signalr a um aplicativo MVC 4.
- Criar uma classe de Hub para enviar conteúdo por push aos clientes.
- Usar a biblioteca do Signalr jQuery em uma página da Web para enviar mensagens e exibir atualizações do Hub.

A captura de tela a seguir mostra o aplicativo de chat concluído em execução em um navegador.

![Instâncias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

As

- [Configurar o projeto](#setup)
- [Executar o exemplo](#run)
- [Examinar o código](#code)
- [Próximas etapas](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar o projeto

Pré-requisitos:

- Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express. Se você não tiver o Visual Studio, consulte [downloads do ASP.net](https://www.asp.net/downloads) para obter a ferramenta gratuita de desenvolvimento do Visual Studio 2012 Express.
- Para o Visual Studio 2010, instale o [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Esta seção mostra como criar um aplicativo ASP.NET MVC 4, adicionar a biblioteca do Signalr e criar o aplicativo de chat.

1. 1. No Visual Studio, crie um aplicativo ASP.NET MVC 4, nomeie-o SignalRChat e clique em OK.

        > [!NOTE]
        > No VS 2010, selecione **.NET Framework 4** no controle suspenso de versão do Framework. O código do signalr é executado nas versões 4 e 4,5 do .NET Framework.

        ![Criar Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Selecione o modelo de aplicativo de Internet, desmarque a opção para **criar um projeto de teste de unidade**e clique em OK.

         ![Criar site da Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Abra as **ferramentas > Gerenciador de pacotes NuGet > console do Gerenciador de pacotes** e execute o comando a seguir. Esta etapa adiciona ao projeto um conjunto de arquivos de script e referências de assembly que habilitam a funcionalidade do Signalr.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Em **Gerenciador de soluções** expanda a pasta scripts. Observe que as bibliotecas de script para Signalr foram adicionadas ao projeto.

         ![Referências de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto, selecione **Adicionar | Nova pasta**e adicione uma nova pasta denominada **hubs**.
      6. Clique com o botão direito do mouse na pasta **hubs** e clique em **Adicionar |** E crie uma nova C# classe chamada **ChatHub.cs**. Você usará essa classe como um hub de servidor Signalr que envia mensagens para todos os clientes.

> [!NOTE]
> Se você usar o Visual Studio 2012 e tiver instalado a [atualização do ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), poderá usar o novo modelo de item do signalr para criar a classe Hub. Para fazer isso, clique com o botão direito do mouse na pasta **hubs** e clique em **Adicionar | Novo item**, selecione **classe de Hub do signalr (v1)** e nomeie a classe **ChatHub.cs**.

1. Substitua o código na classe **ChatHub** pelo código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Abra o arquivo **global. asax** para o projeto e adicione uma chamada para o método `RouteTable.Routes.MapHubs();` como a primeira linha de código no método `Application_Start`. Esse código registra a rota padrão para hubs de sinalização e deve ser chamado antes de registrar qualquer outra rota. O método `Application_Start` concluído é semelhante ao exemplo a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Edite a classe `HomeController` encontrada em **Controllers/HomeController. cs** e adicione o método a seguir à classe. Esse método retorna o modo de exibição de **chat** que será criado em uma etapa posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Clique com o botão direito do mouse no método de `Chat` que você acabou de criar e clique em **Adicionar exibição** para criar um novo arquivo de exibição.
5. No diálogo **Adicionar exibição** , verifique se a caixa de seleção está marcada para **usar um layout ou uma página mestra** (desmarque as outras caixas de seleção) e clique em **Adicionar**.

    ![Adicionar uma exibição](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edite o novo arquivo de exibição chamado **chat. cshtml**. Após a marcação &lt;H2&gt;, Cole a seguinte seção &lt;div&gt; e `@section scripts` bloco de código na página. Esse script permite que a página envie mensagens de chat e exiba mensagens do servidor. O código completo para o modo de exibição de chat aparece no bloco de código a seguir.

    > [!IMPORTANT]
    > Quando você adiciona o Signalr e outras bibliotecas de scripts ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar versões dos scripts mais recentes do que as versões mostradas neste tópico. Certifique-se de que as referências de script no seu código correspondam às versões das bibliotecas de script instaladas em seu projeto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Salve tudo** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração.
2. Na linha de endereço do navegador, acrescente **/Home/chat** à URL da página padrão do projeto. A página de chat é carregada em uma instância do navegador e solicita um nome de usuário.

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Insira um nome de usuário.
4. Copie a URL da linha de endereço do navegador e use-a para abrir mais duas instâncias do navegador. Em cada instância do navegador, insira um nome de usuário exclusivo.
5. Em cada instância do navegador, adicione um comentário e clique em **Enviar**. Os comentários devem ser exibidos em todas as instâncias do navegador.

    > [!NOTE]
    > Esse aplicativo de chat simples não mantém o contexto de discussão no servidor. O Hub transmite comentários para todos os usuários atuais. Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.
6. A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.

    ![Navegadores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução. Esse nó ficará visível no modo de depuração se você estiver usando o Internet Explorer como seu navegador. Há um arquivo de script chamado **hubs** que a biblioteca do signalr gera dinamicamente no tempo de execução. Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor. Se você usar um navegador diferente do Internet Explorer, também poderá acessar o arquivo de **hubs** dinâmicos navegando até ele diretamente, por exemplo http://mywebsite/signalr/hubs.

    ![Script de Hub gerado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinar o código

O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr: criar um hub como o objeto principal de coordenação no servidor e usar a biblioteca do Signalr jQuery para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de sinalização

No exemplo de código, a classe **ChatHub** deriva da classe **Microsoft. AspNet. signalr. Hub** . Derivar da classe **Hub** é uma maneira útil de criar um aplicativo signalr. Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts jQuery em uma página da Web.

No código do chat, os clientes chamam o método **ChatHub. Send** para enviar uma nova mensagem. O Hub, por sua vez, envia a mensagem a todos os clientes chamando **clientes. All. addNewMessageToPage**.

O método **Send** demonstra vários conceitos de Hub:

- Declare métodos públicos em um hub para que os clientes possam chamá-los.
- Use a propriedade **Microsoft. AspNet. signalr. Hub. clients** para acessar todos os clientes conectados a esse Hub.
- Chame uma função do jQuery no cliente (como a função `addNewMessageToPage`) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr e jQuery

O arquivo de exibição **chat. cshtml** no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um Hub do signalr. As tarefas essenciais no código estão criando uma referência para o proxy gerado automaticamente para o Hub, declarando uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e iniciando uma conexão de envio de mensagens para o Hub.

O código a seguir declara um proxy para um Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> No jQuery, a referência à classe de servidor e seus membros estão no camel case. O exemplo de código faz C# referência à classe **ChatHub** no jQuery como **ChatHub**. Se você quiser fazer referência à classe `ChatHub` no jQuery com a capitalização convencional do Pascal como C#faria no, edite o arquivo da classe ChatHub.cs. Adicione uma instrução `using` para fazer referência ao namespace `Microsoft.AspNet.SignalR.Hubs`. Em seguida, adicione o atributo `HubName` à classe `ChatHub`, por exemplo `[HubName("ChatHub")]`. Por fim, atualize sua referência do jQuery para a classe `ChatHub`.

O código a seguir mostra como criar uma função de retorno de chamada no script. A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente. A chamada opcional para a função `htmlEncode` mostra uma maneira de codificar o conteúdo da mensagem em HTML antes de exibi-la na página, como uma maneira de evitar a injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

O código a seguir mostra como abrir uma conexão com o Hub. O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página de bate-papo.

> [!NOTE]
> Essa abordagem garante que a conexão seja estabelecida antes que o manipulador de eventos seja executado.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu que o Signalr é uma estrutura para a criação de aplicativos Web em tempo real. Você também aprendeu várias tarefas de desenvolvimento do Signalr: como adicionar o Signalr a um aplicativo ASP.NET, como criar uma classe de Hub e como enviar e receber mensagens do Hub.

Para saber mais sobre os conceitos de desenvolvimento do Signalr avançado, visite os seguintes sites para código-fonte do Signalr e recursos:

- [Projeto do signalr](http://signalr.net)
- [GitHub e exemplos do signalr](https://github.com/SignalR/SignalR)
- [Wiki do signalr](https://github.com/SignalR/SignalR/wiki)
